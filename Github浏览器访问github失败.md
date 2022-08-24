# Github：浏览器访问github失败

解决方法：

步骤一：获取github网站的真实ip地址

https://tool.chinaz.com/ 站长工具网站

找到DNS查询，检索框内输入www.github.com进行检索，在出现的结果中选择TTL值最小的一个对应的响应IP，复制下来

![Github浏览器访问github失败1](https://github.com/afroginawell/BlogImages/blob/main/Github浏览器访问github失败1.png)

![Github浏览器访问github失败2](https://github.com/afroginawell/BlogImages/blob/main/Github浏览器访问github失败2.PNG)

步骤二：修改本机电脑host文件

针对windows操作系统，打开“C:\Windows\System32\drivers\etc”，用记事本打开“hosts”文件，在文件最后添加“【复制下来的响应IP】 www.github.com”

以上图为例：在hosts文件最后添加"20.205.243.166 github.com"

步骤三：刷新电脑dns缓存

ctrl+r打开控制命令符，输入cmd，打开cmd.exe程序，输入:ipconfig/flushdns"，按下键盘回车键，出现下图所示即可

![Github浏览器访问github失败3](https://github.com/afroginawell/BlogImages/blob/main/Github浏览器访问github失败3.PNG)

步骤四：浏览器访问www.github.com，出现欢迎界面，表示设置成功
