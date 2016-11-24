### golang下载地址

http://golangtc.com/download



### 安装完成后配置环境变量

GOROOT为go的安装地址，通常情况默认安装在/usr/local/go
GOPATH为go项目的工作目录
PATH为系统变量
echo $PATH能够打印出对应值



在个人目录下，编辑.bash_profile文件
在文件末尾添加
export GOROOT=/usr/local/go
export GOPATH=/Users/yangfeiyu/Desktop/gotest:/Users/yangfeiyu/IdeaProjects
export PATH=$PATH:$GOROOT

source .bash_profile使其生效



### 安装完后测试是否正常安装

go version显示版本信息



### go的第一个程序Hello World

~~~import  &quot;fmt&quot; 

~~~

~~~func main() {

~~~

~~~fmt.Println(&quot;Hello World!&quot;)

~~~

~~~}

~~~