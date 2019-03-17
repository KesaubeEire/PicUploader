# 严正声明

因为各种原因我没有Fork,但是我非常感谢作者的博客和GitHub,为我指明了道路,Watch+Star一条龙服务.

[原博客](https://www.xiebruce.top/17.html#4) [原GitHub](https://github.com/xiebruce/PicUploader)

再次感谢作者,为了方便我就暂且保留了作者的Readme.

PicUploader
===============

![PicUploader-logo.png](https://img.xiebruce.top/2018/09/19/781e669d020efbde43dc952eb802293b.png)

PicUploader 是一个用php编写的帮助你快速上传你的图片到七牛云/腾讯云/网易云/百度云/阿里云/京东云/又拍云/sm.ms/Imgur/Ucloud/QingCloud并自动把地址拼接成markdown格式放到剪贴板的小工具，配置完成后，要获取一个可用于markdown的图片外链只需要：右击图片→点击`你的自定义菜单`→到markdown编辑器中粘贴！支持图片压缩，添加文字/图片水印！

![这是一个用于演示的gif图，如果你看到这行文字说明它没加载出来，请点我直接跳转链接查看吧，或者使劲强刷](https://github.com/xiebruce/PicUploader/blob/master/PicUploader%20demonstration.gif)

## 一、 注册存储服务器账号
目前支持七牛云/腾讯云/网易云/百度云/阿里云/京东云/又拍云/sm.ms/Imgur/Ucloud/QingCloud共11种，以下服务选择一种就可以，如果是自己搭建的博客，建议参考我这篇文章：[使用nginx负载均衡+多个云的免费额度打造免费markdown图床](https://www.xiebruce.top/644.html)
### 注册七牛云
- [点击前往七牛云](https://www.qiniu.com)
- [查看注册七牛云对象存储教程](https://www.xiebruce.top/web-development/117.html)
### 注册腾讯云
- [点击前往腾讯云](https://cloud.tencent.com)

### 注册网易云
- [点击前往网易云](https://www.163yun.com)

### 注册百度云
- [点击前往百度云](https://www.163yun.com)

### 注册京东云
- [点击前往京东云](https://www.163yun.com)

### 注册阿里云
- [点击前往阿里云](https://www.163yun.com)

### 注册又拍云
[点击查看又拍云注册使用教程](https://www.xiebruce.top/web-development/570.html)
注意，请不要开启全球cdn，开国内的就可以，国外cdn超贵。

### 无需注册直接使用sm.ms
- 直接使用是上传到[sm.ms](http://sm.ms)，经过测试，在国内(不含港澳台)使用sm.ms时好时坏，除非开科学上网，否则不稳定，如果小齿轮一直转个不停，基本上就是上传失败超时了。
- 设置代理：可以上传的情况下不建议设置代理，如果上传超时、报错，则设置代理试试，直接在配置文件填写代理的ip+端口即可，如（127.0.0.1:1087），不过设置代理后上传速度会稍慢（具体快慢取决于你的代理速度）:
``` php
//https://sm.ms
'smms' => [
    'baseUrl' => 'https://sm.ms/api/',
    //代理地址，如果使用shadowsocks做代理，ip填http://127.0.0.1（或直接填127.0.0.1）即可，
    //端口从『偏好设置→HTTP→监听端口』找，留空或注释掉表示不使用代理
    'proxy' => 'http://127.0.0.1:1087',
],
```
- 注意sm.ms单张图片不能超过5M，但由于我有做压缩，一般不会超过5M，如果上传后粘贴没有内容或者报错，请查看日志，很有可能是这张图片压缩后超过了5M，或者图片是超过5M的gif图（gif图不压缩）
### 注册Imgur
- 先注册Imgur：[https://imgur.com/register](https://imgur.com/register)
- 然后创建Application，这一步能拿到clientID和clientSecret：
[https://api.imgur.com/oauth2/addclient](https://api.imgur.com/oauth2/addclient)
![Xnip2018-11-11_03-56-35.png](https://i.loli.net/2018/11/11/5be737f29d355.png)
- 虽然注册了Imgur，但用我这个工具上传后，你还是无法从你的Imgur账号上找到你上传的图片，原因是我不是网页应用，所以无法做授权(Imgur不提供其他授权方式，所以上面拿到的clientSecret并没有什么用处)，所以，用Imgur做图床还是相当于匿名图床。
- 另外，因为Imgur是国外的，如果上传报错，请使用科学上网工具，并在配置文件中打开代理的注释，填写你的代理端口。

### 注册Ucloud
[点击前往注册Ucloud](https://www.ucloud.cn)

### 注册QingCloud
[点击前往注册QingCloud](https://console.qingcloud.com/signup)

## 二、下载使用
### 1.下载PicUploader
- 使用git下载：
```bash
git clone https://github.com/xiebruce/PicUploader.git
```
- 或者直接下载： [PicUploader](https://github.com/xiebruce/PicUploader/archive/master.zip)
注意下载后把它解压放到一个相对稳定一点的目录，不能放在『下载』里面，因为这样你可能随手删除了。（以后最好不要移动，否则要改配置比较麻烦）

### 2.填写配置
- 把`PicUploader/config/config.php`文件`command+D`复制一份，命名为`config-local.php`，然后在`config-local.php`中修改你的配置(配置你的七牛云/腾讯云/网易云/百度云/阿里云/京东云/又拍云/Imgur/Ucloud/QingCloud其中一个)即可。
- 如果使用sm.ms，那就不需要修改配置了，直接使用！
- 以配置七牛云存储为例，在config-local.php文件中找到以下代码，填好AK/SK/bucket/domain四个参数，如果不知道参数怎么来，请查看[注册七牛云对象存储教程](https://www.xiebruce.top/uncategorized/117.html)
``` php
//Qiniu Cloud
'qiniu' => [
    //七牛云AppKey
    'AK' => 'ASGZ******************************o0AoF',
    //七牛云AppSecret
    'SK' => 'UoC******************************JylkEy',
    //七牛云对象存储空间名
    'bucket' => 'm*****n',
    //七牛云外链域名
    'domain' => 'http://pe5s*********clouddn.com',
    //七牛优化参数，直接加在链接后面，但是不会优化原图，只会优化输出的图片，如果不需要可以不配置该项（即注释掉）
    // 'optimize' => '?imageMogr2/thumbnail/800x/strip/quality/80',
],
```

- 搜索`storageType`，并在`=>`后设置您当前要使用哪个云，如我要使用七牛云：
```php
'storageType' => 'Qiniu',
```
- 支持同时上传到多个云，只需逗号隔开即可，比如我现在就同时上传到6个云：
```php
'storageType' => 'Qiniu,Tencent,Netease,Upyun,Qingcloud,Ucloud',
```
也许你会觉得奇怪，同时上传到那么多云干嘛？看了这篇文章你就知道了：[使用nginx负载均衡+多个云的免费额度打造免费markdown图床](https://www.xiebruce.top/644.html)

- 水印配置直接参考配置文件的注释

## 三、使用『自动操作/Automator』新建一个workflow服务
### 1.打开『自动操作/Automator』
或者像下面gif图演示的那样用『聚焦搜索』来打开
![](https://img.xiebruce.top/2018/08/30/a808c59b097d5877e650dc7ced31977d.png)

### 2.macOS请按下图操作
注意每一个需要选择的地方都不要选错，其中最后一步保存的文件名，将出现在右键菜单上，你可以自己取一个自己喜欢的名字，比如『获取Markdown外链』。
![添加一个服务.gif](https://github.com/xiebruce/PicUploader/blob/master/Create%20a%20service.gif)

``` php
export LC_CTYPE="zh_CN.UTF-8"
/usr/bin/php /Users/xxxxx/www/PicUploader-master/index.php "$@" | pbcopy
```
注意：
- 注意`/usr/bin/php`这个路径，最好在终端用`which php`命令来查看它的路径，确认一下是否正确，如果执行`which php`命令后输出的不是`/usr/bin/php`，则以`which php`命令输出的为准。
- 把/Users/xxxxx/www/PicUploader-master/index.php 的路径换成你自己的index.php路径，怎么看你的index.php路径呢？打开终端，把index.php文件拖到终端上，终端就会显示出它的路径。
- 特别注意：Mac自带php缺少freetype扩展，导致gd库处理图片的时候报错，如果你是做php开发的，那这个问题你肯定解决了，如果不是，那你需要解决这个问题,先用这下边这个命令看看有没有内容输出，如果什么都没有输出，那就确定是缺少freetype
``` shell
php -i | grep configure | grep freetype
```
- 确定缺少freetype之后，如果你不用水印功能，则可以不理会，如果你要使用水印功能，请使用以下命令重装php(否则会报错)，重装后，上面的`/usr/bin/php`请直接改成`/usr/local/bin/php`，不需要看`which php`的结果了：
``` shell
brew install php
```

### 3.试试右击任意图片
看，最后一个按钮是不是就是你刚刚保存的Services名称？什么？你电脑上没看见？那是因为你电脑上超过5个这种类似的菜单，它就自动收到二级菜单下了，其实很多菜单根本用不到，你可以到`系统偏好设置`→`键盘`→`快捷键`→`服务(Services)`里面找到对应的按钮，把它们的勾去掉就行。  
![Xnip2018-09-07_23-58-26.png](https://img.xiebruce.top/2018/09/08/66b431b893bc5b2abc456b6bf4f0b6ee.png)

### 4.Windows系统配置
- 安装php环境，如果你没有安装过，建议安装[phpstudy](http://phpstudy.php.cn)，注意按提示安装对应的VC++运行环境
- 解压『add-right-click-menu.zip』，右击执行解压出来的`.bat`文件，点击`以管理员身份运行`，输入php.exe的路径和PicUploader的index.php的路径，然后即可出现右键菜单。
<p align="center"><img src="https://img.xiebruce.top/2018/12/06/0f70d6ad3be4c1708f6b4d39415ad2a6.jpg" title="Xnip2018-12-06_13-49-49.jpg" alt="Xnip2018-12-06_13-49-49.jpg"></p>
<p align="center"><img src="https://img.xiebruce.top/2018/12/06/d021219e77443e46228d6e6246670b89.jpg" title="Xnip2018-12-06_14-31-38.jpg" alt="Xnip2018-12-06_14-31-38.jpg"></p>
<p align="center"><img src="https://img.xiebruce.top/2018/12/06/7900185c1aaf78c5c5a689a4389a93cf.jpg" title="Xnip2018-12-06_14-32-49.jpg" alt="Xnip2018-12-06_14-32-49.jpg"></p>

添加后就有右键菜单了
<p align="center"><img src="https://img.xiebruce.top/2018/12/06/1667c3905cc3e9bf1cf35620a4b0d5b1.jpg" title="Xnip2018-12-06_13-52-37.jpg" alt="Xnip2018-12-06_13-52-37.jpg"></p>

添加右键菜单的原理是往注册表添加一个项
<p align="center"><img src="https://img.xiebruce.top/2018/12/06/cd7fe301737a9901a19cce78f2010519.jpg" title="Xnip2018-12-06_14-25-24.jpg" alt="Xnip2018-12-06_14-25-24.jpg"></p>

- 右击图片→点击自定义菜单，如果正常，一般会出现一个黑框并稍微停留才消失(因为上传需要一定时间)，然后你就可以去粘贴了，但如果黑框闪一下就没了，那多半是有错误，请检查php.exe是否可以直接在cmd运行，如果不行，请直接到注册表中查看路径是否正确，如果路径没问题，那就是php.exe跟系统不兼容，换个版本试试，也有可能缺少vc++库之类的。
- 如果PicUploader或php.exe换了路径，你可以重新执行`.bat`工具，重新添加一次即可。
- Windows系统原因，无法同时上传多张，所以在Windows系统一次只能上传一张。

### 5. mweb配置
mweb提交图片是一张图片发起一个post请求，多张图片则会循环发起多个请求，所以需要配置一个http服务器来接收该post请求并处理上传。
环境要求：nginx + php-fpm 或 apache + php
环境并不是搭建在远程服务器，而是直搭建在你本地电脑即可，至于怎样搭建nginx+php运行环境或apache+php运行环境，请自行百度。

以下是nginx配置文件（注意修改root路径为你的PicUploader目录实际路径）
```nginx
server {
    listen 80;
    server_name api.picuploader.com;

    access_log /usr/local/var/log/nginx/api.picuploader.com.error.log combined;
    error_log /usr/local/var/log/nginx/api.picuploader.com.error.log error;

    root /path/to/PicUploader;

    location / {
        index index.php;
        try_files $uri $uri/ index.php$is_args$args;
    }

    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        include fastcgi.conf;
    }
}
```

此外记得要把nginx配置的域名添加到hosts文件中：
```
127.0.0.1 api.picuploader.com
```

点击mweb的偏好设置→发布(publishing)→点击右边最下面一个`自定义(custom)`：
<p align="center"><img src="https://img.xiebruce.top/2019/01/12/e793f89b6057b6cb3938fec071d59b8b.jpg" title="Xnip2019-01-12_21-29-30.jpg" alt="Xnip2019-01-12_21-29-30.jpg"></p>

按下图填写：
<p align="center"><img src="https://img.xiebruce.top/2019/01/12/364177f976b82e0508291cfbbcc9188a.jpg" title="Xnip2019-01-12_21-32-44.jpg" alt="Xnip2019-01-12_21-32-44.jpg"></p>

写好文章后，点击顶部菜单栏中的的发布(publish)或编辑器右上角的分享按钮→上传本地图片到...(Upload Local Images to...)：
<p align="center"><img src="https://img.xiebruce.top/2019/01/12/d50d050be893e19b338afe3dd0df063a.jpg" title="Xnip2019-01-12_21-38-34.jpg" alt="Xnip2019-01-12_21-38-34.jpg"></p>

如下图，点击Upload Images：
<p align="center"><img src="https://img.xiebruce.top/2019/01/12/38e6ded515aaddada287dc2a65f096d3.jpg" title="Xnip2019-01-12_21-48-23.jpg" alt="Xnip2019-01-12_21-48-23.jpg"></p>
上传完成后，即会出现地址图片地址，你可以点`New Document`建立一篇新文章，该文章就是你写的这篇文章，只不过图片全部被替换为线上地址了，然后你就可以发布这篇文章了，当然你也可以点`Copy Markdown`或`Copy HTML`复制markdown或html到其他地方(比如你的博客后台)去发布。

## 三、开始使用
### 1.右击任意一张图片
如果一切正常，你对着图片右击，并点击右键菜单中的`获取Markdown链接`后，你将会在Mac顶部工具栏那里看到一个小齿轮在转动，说明Services正在执行，图片正在上传，当小齿轮停止转动(不需要等它消失)，即说明上传已经完成，同时右上角会弹出通知，此时剪贴板已经有你上传的图片地址了，直接到markdown编辑器`command+v`粘贴试试吧
![](https://img.xiebruce.top/2018/08/29/79fe0db0bfbecca78bde951f90554fb4.png?imageMogr2/thumbnail/50x/strip/quality/80)
### 2. 为你创建的服务创建快捷键
`系统偏好设置`→`键盘`→`快捷键`→`服务(Services)`→找到你的自定义菜单添加快捷键即可，当然喜欢用鼠标搞定的童鞋也可以不创建。  
![](https://img.xiebruce.top/2018/08/30/549a81ea2ab503d2585406497f8d61d3.png?imageMogr2/thumbnail/800x/strip/quality/80)
### 3.查看上传日志
因为在配置文件中的`logPath`可以配置日志显示在桌面， 'logPath' => 'desktop' ，所以不出意外，你的桌面装会有一个如下图所示的文件夹，这里保存了你上传的图片的Markdown外链地址，你在七牛云（或者其他服务器）是能找到对应的图片的，如果你觉得哪张图片不想要，可以在对应服务器中把它删除了。
<p align="center"><img src="https://img.xiebruce.top/2018/11/19/cc344641965d5d79aa6c2aba8f408552.jpg" title="Xnip2018-11-19_12-59-59.jpg" alt="Xnip2018-11-19_12-59-59.jpg"></p>

这个日志文件也是markdown格式，所以你可以用mweb之类的markdown编辑器查看它，但是有个问题当你用Mweb打开它之后，如果再上传一张图片，这个文件里肯定会增加一条你上传的记录，但是在Mweb中却不会刷新，必须关掉重新打开才能看的到，这个特别麻烦，建议用[typora](https://typora.io/)这个软件来打开日志，这个会自动刷新，而且免费。

日志是倒序添加的，即最新上传的图片在最前面，方便一时忘了粘贴，可以找到地址，因为在服务器中文件多的话比较难找到。


## 四、注意事项
### 1.关于config里的imgWidth选项
- 由于各个云服务器免费空间有限(七牛云免费10G,好像也不小了哈哈)，而且大文件加载特别慢，所以有些大图片需要压缩后上传，由于试过按质量压缩对图片影响较大，所以选择按宽度压缩（高度会保持等比例适应）。
- 如果要设置压缩图片，建议imgWidth设置为800或1000，别担心分辨率低的图压缩后看不清，即使设置了该选项，分辨率比较低的图片也是不会被压缩的。
- 由于gif图片实际上是由很多张图片组合在一起的，它的压缩比较麻烦，需要Mac装一些额外的库，所以暂时选择不压缩gif（也就是说即使你设置了imgWidth为800或者1000，也不会压缩gif图片）
- 如果你用的是七牛云，则返回的图片地址中『?imageMogr2/thumbnail/800x/strip/quality/80』这部分是七牛云的压缩参数(如果你配置了的话)，也就是说这部分参数你可以去掉直接查看你上传的图，其中thumbnail/800x表示设置宽度为800，strip表示去除一些exif信息以减小图片大小，quality/80表示设置图片质量为原来的80%，其实还可以在最后加个/format/webp这样图片加载会快很多，因为webp能在最大程度还原图片清晰度的同时把图片体积降到最小。

### 2.安装管理工具
因为图片上传到七牛云后，在七牛的后台实在无法直观的浏览和管理图片，如果你希望方便管理你的图片，可以下载两款谷歌浏览器插件：
- [qiniu upload files](https://chrome.google.com/webstore/detail/qiniu-upload-files/emmfkgdgapbjphdolealbojmcmnphdcc) 用来管理七牛云上的图片
- [七牛云图床](https://chrome.google.com/webstore/detail/%E4%B8%83%E7%89%9B%E4%BA%91%E5%9B%BE%E5%BA%8A/fmpbbmjlniogoldpglopponaibclkjdg) 一款可以让你直接上传图片到七牛云的工具，但是它没有分文件夹。
- 腾讯COS官方MacOS客户端：[点击前往下载](https://cloud.tencent.com/document/product/436/11366)
- 百度BOS官方MasOS客户端：[点击前往下载](https://cloud.baidu.com/doc/BOS/BOSCLI.html#.E4.B8.8B.E8.BD.BD)
- 如果是又拍云，因为又拍云没有网页管理工具，只能自己用ftp工具查看。

### 3.上传完成无法弹出通知问题
- 一般情况下，上传完成后右上角就会弹出通知，如果你没有弹出，有可能是因为系统没更新的问题，像我的系统版本一直是10.13.1，我的就不弹，但是我让别人试就会弹（后来我更新10.14(Mojave)系统后，就能正常弹出提示了）。
- 如果你的系统也没更新，那有可能也不会弹通知，那就只能看工具栏上的小齿轮，小齿轮停止转动说明上传完成（不需要等小齿轮消失，停止转动即已完成），然后就可以去markdown编辑器粘贴了。

### 4、说个不幸的消息
- 七牛云默认域名是测试域名，只能使用30天，30天后该域名会被回收，所以你需要绑定自己的域名，但绑定自己的域名需要域名已经备案，而域名备案是跟网站一块的，就是说如果你只买了域名没有自己的网站的话，是无法备案的，这时你可以看看有没有朋友有已备案的域名，叫他给你解析一个二级域名也行，因为一个人一般只用一两个二级域名，所以会有很多二级域名可用的。
- 又拍云也提供默认测试域名，不过我问过又拍客服，又拍云的测试域名是不会回收的，也就是你可以一直使用，不需要有自己已备案的域名，但缺点是，又拍云的默认域名提供的cdn服务器比较少，也就是说，你用又拍云默认的域名可能加载图片会相对比较慢（相对绑定自己的域名来说），但具体有多慢，我没有试过，所以也是建议你绑定自己的域名，当然了，又拍云绑定自己的域名，同样要求域名已备案，国内所有云都有这个要求！

## 五、更新日志
### 2019-01-12 v2.6.2版本
- 添加mweb支持
- 添加网页版上传支持
### 2018-12-06 v2.6.1版本
- 添加支持Windows系统（理论上Linux系统一样可用，只要你能通过右键菜单或快捷键调用上传命令）
### 2018-11-24 v2.6版本
- 添加青云QingCloud及Ucloud的支持
- 修改结构，去除冗余代码
- 添加支持同时上传到多个云
### 2018-11-15 v2.5.2版本
- 用户指定日志位置后，只创建日志软链接(快捷方式)，日志原文件存储在项目目录下的logs目录中
### 2018-11-13 v2.5.1版本
- 修复压缩无效的bug
- 修复压缩临时文件未删除的问题
- 添加压缩质量配置
### 2018-11-11 v2.5版本
- 添加支持Imgur
### 2018-10-20 v2.4.1版本
- 上传图片到sm.ms添加支持使用HTTP代理(如shadowsocks)
- fix bug
### 2018-10-20 v2.4版本
- 添加水印支持（支持文字水印、图片水印）
- 去掉了上传到sm.ms同时返回了删除链接的问题，但日志里还是会有删除链接
### 2018-09-21 v2.3版本
- 修改config结构
- 使用变量类名动态识别调用不同存储类型对应的类以精简代码
- 在每个类里动态调用配置文件
### 2018-09-19 v2.2.2版本
- 添加又拍云支持
### 2018-09-08 v2.2.1版本
- 解决图片文件名为中文时粘贴乱码或者无法粘贴的问题
- 修改了README.md中说的不清楚的地方，部分英文菜单改用中文
### 2018-09-06 v2.2版本
- 添加支持百度云
- 添加支持京东云
- 添加支持阿里云
### 2018-09-06 v2.1版本
- 添加支持网易云
- bug fix
### 2018-09-06 v2.0版本
- 完全重构代码
- 添加支持腾讯云
- 添加上传日志路径配置（可配置到桌面方便随时查看）
### 2018-09-01 v1.1版本
- 添加支持[https://sm.ms](https://sm.ms)，默认上传到sm.ms
- 添加mardownWithLink链接类型（点击图片后能跳转到图片链接）
- fix bug log时间设置成东八区
### 2018-08-30 v1.0版本
由于个人写markdown需要，目前现有的一些笔记软件都因为各种原因传图并不是很方便，于是写了这个小工具，想到可能有跟我一样需求的童鞋，就顺手发出来供有需要的童鞋使用，如有处理不好的地方在所难免，希望大家能批评指正！

## 六、参考资料
[七牛云phpsdk文档](https://developer.qiniu.com/kodo/sdk/1241/php)  
[百度知道：没有个人网站怎么注册七牛云存储](https://zhidao.baidu.com/question/714797122999158885.html)  
[sm.ms Api](https://sm.ms/doc/)  
[GuzzleHttp官方文档](http://docs.guzzlephp.org/en/stable/quickstart.html#post-form-requests)  
[腾讯云phpsdk文档](https://cloud.tencent.com/document/product/436/12266)  
[网易云phpsdk文档](https://www.163yun.com/help/documents/15677641583214592)  
[阿里云phpsdk文档](https://help.aliyun.com/document_detail/32101.html?spm=a2c4g.11186623.4.2.70d8c839FzL6jV)  
[京东云phpsdk文档](https://www.jdcloud.com/help/detail/1910/isCatalog/1)  
[百度云phpsdk文档](https://cloud.baidu.com/doc/BOS/PHP-SDK.html#.E5.AE.89.E8.A3.85SDK.E5.B7.A5.E5.85.B7.E5.8C.85)  
[又拍云phpsdk文档](https://github.com/upyun/php-sdk)  
[Imgur API](https://apidocs.imgur.com/#2078c7e0-c2b8-4bc8-a646-6e544b087d0f)  
[google](http://www.google.com/ncr)  
[百度](http://www.baidu.com)  

