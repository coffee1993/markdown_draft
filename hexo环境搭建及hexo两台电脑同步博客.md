# hexo环境搭建及多个设备同步博客解决方案。

场景:以前在学校使用自己的笔记本来更新hexo博客，近日入职之后，公司电脑想同步家里的hexo博客。

## win7下的hexo环境搭建
hexo环境搭建包含git工具的使用和nodejs的安装，在通过npm安装hexo，然后再进行配置。

### Git工具下载

[Git下载](https://git-scm.com/)

下载安装即可。

### Git 配置代理

有些公司的git访问github是需要配置代理的。

git config --global http:proxy http://proxy.tencent.com:8080
git config --global https:proxy http://proxy.tencent.com:8080

### Nodejs下载安装

[Nodejs下载](https://nodejs.org/en/)

尽量使用默认路径, 以保证环境变量的自动配置.

配置环境变量：
npm -> C:\Program Files\nodejs\node_modules\npm

### npm 安装 hexo

npm 在公司也要配置代理才能使用：

- npm设置代理

npm config set proxy http://web-proxy.oa.com:8080
npm config set https-proxy https://web-proxy.oa.com:8080

配置镜像
npm config set registry http://registry.cnpmjs.org

- 安装hexo

npm install -g hexo-cli
npm install hexo -save

### 部署hexo

创建一个用于存放博客源代码的文件夹，命名随意。

用git bash here 打开 ，cd 文件夹名，执行初始化操作

hexo init

hexo new "hellohexo"
创建一个文件hellohexo.md

部署：

hexo generater

hexo server

hexo deploy
