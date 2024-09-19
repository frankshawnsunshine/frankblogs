# 1 Mac上安装多个JDK
## 1.1 下载JDK
mac芯片是Apple的下载ARM64 DMG Installer
## 1.2 查看安装目录
```
#访问JDK安装目录
cd /Library/Java/JavaVirtualMachines

#查看安装的JDK版本
ls -al
```
## 1.3 配置环境变量
### 1.3.1 打开环境变量文件
```
# 输入
cd ~

# 打开环境变量配置文件
open .bash_profile

# 报错：.bash_profile does not exist.
# 第一次配置环境变量，先创建文件
touch .bash_profile

# 再次执行打开环境变量配置文件
open .bash_profile
```
### 1.3.2 配置JDK多版本环境变量
```
# 复制如下内容粘贴到.bash_profile中，
# 因为我是安装了2个，所以配置了2个版本
# 你自己是安装了几个版本就配置几个
export JAVA_8_HOME=$(/usr/libexec/java_home -v1.8)
export JAVA_17_HOME=$(/usr/libexec/java_home -v17)

alias java8='export JAVA_HOME=$JAVA_8_HOME'
alias java17='export JAVA_HOME=$JAVA_17_HOME'

export JAVA_HOME=$JAVA_17_HOME
# 记得保存，可以用快捷键 cmd + s
```
### 1.3.3 检查环境变量
```
# 配置文件立即生效
source .bash_profile

# 查看 JAVA_HOME 目录
echo $JAVA_HOME

# 查看 JDK 版本信息
java -version
```
### 1.3.4 JDK版本切换
我们定义了2个别名：java8和java17，其中默认配置为 jdk17。

二者要相互切换，在终端中输入命令即可，如下
```
#切换到JDK8：
java8
# 查看 JDK 版本信息
java -version

#切换到JDK17：
java17
# 查看 JDK 版本信息
java -version
```
# 2 Mac Maven环境安装
1. 配置路径文件，在命令行终端输入下一名命令
`open ~/.bash_profile`
2. 将maven添加到系统环境变量
```
#maven
export MAVEN_HOME=/Users/frankshawn/Environment/apache-maven-3.9.9
export PATH=$MAVEN_HOME/bin:$PATH
```
3. 让系统环境变量强制生效
`source ~/.bash_profile`
4. 查看maven是否配置生效
`mvn -version`
5. 配置maven本地仓库
修改maven文件中的conf文件夹下的settings.xml文件
```
<!-- 本地仓库配置 -->
<localRepository>/Users/frankshawn/Environment/LocalRepository</localRepository>
```
配置阿里云仓库
```
<!-- 配置阿里云镜像仓库 -->
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>
</mirror>
```
# 3 Mac安装Git
1. 
```
brew install git

zsh: command not found: brew
```
2.
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

curl: (7) Failed to connect to raw.githubusercontent.com port 443 after 2 ms: Couldn't connect to server
```
3.
`/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"`
4. 
```
git -v

git version 2.39.3 (Apple Git-146)
```
# 4 Mac安装MySQL8.0
1. 下载tar.gz
2. 解压后，修改名称为mysql，放到`/usr/local/`目录下
3. 打开.bash_profile配置环境变量
```
#mysql
export MYSQL_HOME=/usr/local/mysql
export PATH=$MYSQL_HOME/bin:$MYSQL_HOME/support-files:$PATH 
```
4. 让环境变量生效
`source ~/.bash_profile`
5. 修改mysql安装目录所属用户与用户组，使用如下命令：`sudo chown -R root:admin mysql`
6. 然后执行`sudo mysqld --initialize  --user=mysql`命令来初始化，会初始化一个密码，此密码是一个root密码，注意：此处的--user=mysql对应的是mysql用户，如果是root系统用户则使用--user=root如下信息：
7. MySQL 5.7.18及以上不再自带默认配置文件。手动写一个就行：使用如下命令：`sudo vim /etc/my.cnf`，然后输入如下内容：
```
[mysqld]
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
pid-file=/usr/local/mysql/data/mysql.pid
log_error=/usr/local/mysql/data/mysql_error.log
```
8. 保存之后修改权限，使用命令修改：`sudo chmod 664 /etc/my.cnf` 
9. 然后执行`mysql.server start`来启动
10. 输入命令`mysql -u root -p`，输入刚刚的密码
11. 修改密码
`alter user'root'@'localhost' IDENTIFIED BY '123abcABC';`
12. 允许远程登录
```
use mysql;
select user,host from user;
update user set host="%" where user="root";
flush privileges;
```
# 5 Mac安装Redis
1. 下载redis安装包：https://redis.io/download/
2. 解压后，在该目录下执行编译测试指令
`sudo make test`
3. 安装
`sudo make install
`
4. 启动
`redis-server
`
5. 连接Redis
`redis-cli -p 6379`






