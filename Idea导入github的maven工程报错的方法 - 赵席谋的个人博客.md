# Idea导入github的maven工程报错的方法 - 赵席谋的个人博客
1.  使用idea打开pom文件，并作为项目打开
2.  这时候点击锤子按钮进行编译，会报各种奇奇怪怪的错误，一招搞定  
    选中maven的这个按钮：`Execution Maven Goal`:  
    ![](https://github.com/Junqian-Hao/doc/blob/main/image/2024-2-5%2017-04-50/5909ab0b-05d6-4b62-acab-63866b6fd323.png?raw=true)
      
    执行命令：`mvn idea:idea` 执行完成后再点击idea的build命令，显示成功，打开看对应的java文件，也没有任何编译错误。就是这么舒服  
    ![](https://github.com/Junqian-Hao/doc/blob/main/image/2024-2-5%2017-04-50/94ff9045-599d-4f42-a9ec-bb5d905cc2e4.png?raw=true)
    

如果执行命令的时候jar包无法下载，或者瞎子啊速度慢，需要使用阿里云的镜像仓库。  
修改idea的maven配置指定settings.xml地址。  
![](https://github.com/Junqian-Hao/doc/blob/main/image/2024-2-5%2017-04-50/31d54292-9f2f-4404-a8c3-aaaa8dac06fb.png?raw=true)
  
setting.xml内容如下：需要自行配置`<localRepository>/Users/ximouzhao/.m2/repository</localRepository>`的地址为对应的你的机器的地址喔

```null
<?xml version="1.0" encoding="UTF-8"?>
<settings
    xmlns="http://maven.apache.org/SETTINGS/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    
    <localRepository>/Users/ximouzhao/.m2/repository</localRepository>
    
    <servers></servers>
    
    <mirrors>
        
        <mirror>
            <id>aliyun</id>
            <mirrorOf>central</mirrorOf>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        </mirror>
    </mirrors>
    
    <profiles>
        
        <profile>
            
            <id>lovecto_profile</id>
            
            <repositories>
                <repository>
                    <id>aliyun</id>
                    <name>aliyun</name>
                    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                        <updatePolicy>always</updatePolicy>
                    </snapshots>
                </repository>
            </repositories>
            
            <pluginRepositories>
                <pluginRepository>
                    <id>thirdparty_repository</id>
                    <name>thirdparty_repository</name>
                    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                        <updatePolicy>always</updatePolicy>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
    
    <activeProfiles>
        <activeProfile>lovecto_profile</activeProfile>
    </activeProfiles>
</settings>
```