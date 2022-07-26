## Github：在markdown文件内显示图片

思路：检查图片的路径

问题一：图片是本地路径，如何添加到markdown文件内

步骤一：在Github中建立一个仓库，将本地图片上传到该仓库中

步骤二：在Github中选中上传的文件，并复制图片地址

![Github在markdown文件内显示图片1](https://github.com/afroginawell/BlogImages/blob/main/Github%E5%9C%A8markdown%E6%96%87%E4%BB%B6%E5%86%85%E6%98%BE%E7%A4%BA%E5%9B%BE%E7%89%871.jpg)

步骤三：把刚复制的图片的地址更新到Github中的markdown文件中对应位置，即可

如果以上步骤完成后，markdown文件中仍显示不了图片，请在Github中打开刚上传的图片，检查是否可以正常显示图片，如果图片无法正常显示，执行问题二的步骤

问题二：Github网页上图片显示失败

前提：针对windows系统

步骤一：用记事本打开 C:\Windows\System32\drivers\etc\hosts文件

步骤二：修改hosts文件

​	在文件末尾添加一段IP和域名的映射，通过此种方式直接找到图片对应的ip地址。在文件末尾添加以下内容，可以直接复制到host文件中，复制完保存

```
# Github Start 
140.82.113.3      github.com
140.82.114.20     gist.github.com

151.101.184.133    assets-cdn.github.com
151.101.184.133    raw.githubusercontent.com
```

​	保存完，刷新Github显示图片的页面，此时图片应该显示成功，再检查markdown文件，发现图片也显示成功。

*参考*

```
# Github Start 
140.82.113.3      github.com
140.82.114.20     gist.github.com
 
151.101.184.133    assets-cdn.github.com
151.101.184.133    raw.githubusercontent.com
199.232.28.133     raw.githubusercontent.com 
151.101.184.133    gist.githubusercontent.com
151.101.184.133    cloud.githubusercontent.com
151.101.184.133    camo.githubusercontent.com
199.232.96.133     avatars.githubusercontent.com
151.101.184.133    avatars0.githubusercontent.com
199.232.68.133     avatars0.githubusercontent.com
199.232.28.133     avatars0.githubusercontent.com 
199.232.28.133     avatars1.githubusercontent.com
151.101.184.133    avatars1.githubusercontent.com
151.101.108.133    avatars1.githubusercontent.com
151.101.184.133    avatars2.githubusercontent.com
199.232.28.133     avatars2.githubusercontent.com
151.101.184.133    avatars3.githubusercontent.com
199.232.68.133     avatars3.githubusercontent.com
151.101.184.133    avatars4.githubusercontent.com
```

