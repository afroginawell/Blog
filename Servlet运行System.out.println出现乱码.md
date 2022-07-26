## Servlet：运行System.out.println出现乱码

前提：理想情况所有程序使用utf-8编码，并使用IDEA编辑器

思路一：System.out.println()输出的变量内容本身就是乱码

​	尝试使用System.out.println("你好中国") 看是否输出内容是否为乱码。如果是乱码，说明是Servlet和Tomcat交互时，Servlet提交的编码格式与Tomcat解码的编码格式不一致导致；如果不是，就是输出变量在传递的过程中某一处出现了问题。

问题一：Servlet提交的编码格式与Tomcat解码的编码格式不一致导致

​	本质上Servlet文件就是java文件，如果在不使用Tomcat运行java文件，并输出内容无乱码的情况，很可能是Tomcat解码出现问题。可以使用以下代码来验证是否java文件和Tomcat所使用的编码格式

```java
// 普通java文件的程序
public static void main(String[] args) throws Exception{	
    String charset = "utf-8";	// 正确编码
    String str = "中文";	//测试用例
    boolean flag = str.equals(new String(str.getBytes(), charset));	//语句需要处理异常，可选择throws Exception
    System.out.println(flag);
}

// Servlet文件内的程序
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String charset = "utf-8";	// 正确编码
    String str = "中文";	//测试用例
    boolean flag = str.equals(new String(str.getBytes(), charset));	//语句需要处理异常，可选择throws Exception
    System.out.println(flag);
}
```

​	如果普通java文件程序得到的结果flag为False，修改编码为utf-8的方式如下（即IDEA配置文件格式为UTF-8）：

步骤一：File->Setting->Editor->File Encodings

![img](https://github.com/afroginawell/Blog/edit/main/Servlet%E8%BF%90%E8%A1%8CSystem.out.println%E5%87%BA%E7%8E%B0%E4%B9%B1%E7%A0%81.md)

步骤二：修改encodings.xml文件

​	每当创建一个新的项目都会在该项目中生成一个新的.idea文件夹，该文件夹中也会生成新的encodings.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project version="4">
  <component name="Encoding" native2AsciiForPropertiesFiles="true" defaultCharsetForPropertiesFiles="UTF-8">
    <file url="file://$PROJECT_DIR$/src/main/java" charset="UTF-8" />
    <file url="PROJECT" charset="UTF-8" />
  </component>
</project>
```

步骤三：File->Settings->Build,Execution->Compiler->Java Compiler

设置Additional command line parameters选项为 -encoding utf-8

![img](C:\Users\hp\Desktop\blog_images\Servlet运行System.out.println出现乱码2.jpg)

​	三个步骤搞完，再运行之前的检验编码的代码，查看是否修改成功。

​	如果Servlet文件程序得到的结果flag为False，修改编码为utf-8的方式如下（即IDEA配置Tomcat编码为UTF-8）：

步骤一：Run/Debug Configuration->tomcat

![image-20220726080324786](C:\Users\hp\Desktop\blog_images\Servlet运行System.out.println出现乱码4.jpg)

将VM options设置为-Dfile.encoding=UTF-8，适用于以后所有tomcat文件都改为UTF-8编码 

![img](C:\Users\hp\Desktop\blog_images\Servlet运行System.out.println出现乱码3.jpg)

如果仅仅只是针对当前项目需要使用UTF-8编码，可选中当前项目的tomcat进行配置，将VM options设置为-Dfile.encoding=UTF-8

![img](C:\Users\hp\Desktop\blog_images\Servlet运行System.out.println出现乱码5.jpg)

如果不是IDEA编辑器，或者想要一劳永逸所有编辑器tomcat都采用UTF-8编码，可以设置一个环境变量

变量名：`JAVA_TOOL_OPTIONS`
变量值：`-Dfile.encoding=UTF-8`

![img](C:\Users\hp\Desktop\blog_images\Servlet运行System.out.println出现乱码6.jpg)

配置完环境变量，就可以删除刚刚设置的tomcat中的内容，重启idea，查看配置完的效果，利用测试编码格式的代码进行测试

问题二：输出变量在传递的过程中某一处出现了问题

​	这个问题需要具体分析变量的传递过程

​	简单做法是写一个过滤器Fileter，将所有tomcat运行程序进行编码的设置

```java
// SetCharacterEncodingFilter.java文件
@WebFilter(filterName = "SetCharacterEncodingFilter")	// 文件名随意
public class SetCharacterEncodingFilter implements Filter {
    private final String encoding = "utf-8";

    public void destroy() {
    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        req.setCharacterEncoding(encoding);
        resp.setContentType("text/html;charset="+encoding);
        chain.doFilter(req, resp);
    }

    public void init(FilterConfig config) throws ServletException {

    }

}

// xml文件中的配置信息
<!--filter-name是你的filter文件的文件名-->
<filter-mapping>
<filter-name>SetCharacterEncodingFilter</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
```

​	这个配置完，基本上能解决大部分在变量传递过程中的出现的编码问题。
