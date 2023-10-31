# Mac下tunnelblick自动填充OTP - 赵席谋的个人博客
### 需求

公司的tunnelblick每次连接的时候都需要输入Google Authenticator，在使用时有时会遇到连接频繁中断的情况，每次重连都需要，如图所示，最下面的Security code我们需要输入 Google Authenticator生成的code  
![](https://ximouzhao.com/wp-content/uploads/2023/06/image-1688216345988.png)

### 实现

1.  安装oath-toolkit，这是一个命令行工具，可以通过OTP Secret生成Google Authenticator code，我们需要通过这个工具自动生成Google Authenticator code
    
    ```null
    brew install oath-toolkit
    ```
    
2.  查看oath-toolkit的安装的目录,并记录下来
    
    ```null
    ➜  which oathtool
    /opt/homebrew/bin/oathtool
    ```
    
3.  创建`.tblk`文件夹，这个文件夹是用来给Tunnelblick传递配置参数的文件夹
4.  将`*.ovpn`文件放入`.tblk`文件夹，例如
    
    ```null
    cp ~/Your_Dir/profile.ovpn my-vpn.tblk/
    ```
    
5.  在`my-vpn.tblk`文件夹中创建脚本`password-append.user.sh`，内容如下（将{Your OTP Secret}替换为你自己的秘钥）：
    
    ```null
    #!/bin/bash
    /opt/homebrew/bin/oathtool --totp -b -d 6 {Your OTP Secret}
    ```
    
6.  增加执行权限
    
    ```null
    chmod +x password-append.user.sh
    ```
    
7.  将`my-vpn.tblk`拖动到Tunnelblick中，第一次登录时填写用户名和密码并设置保存到key chain，不需要再填写Security code即可连接
8.  后续的连接就不需要填写任何东西即可登录，并且可以享受Tunnelblick的自动连接功能

### 参考文档

[https://gist.github.com/vlastikcz/50445200f840b71cf908076fb9a845d0](https://gist.github.com/vlastikcz/50445200f840b71cf908076fb9a845d0)  
[https://tunnelblick.net/cUsingScripts.html#tunnelblick-vpn-configuration-scripts](https://tunnelblick.net/cUsingScripts.html#tunnelblick-vpn-configuration-scripts)

文章导航
----