# Pixiu （基于docker 部署前后端分离 部署项目 后端 “转载的当笔记用“）


### 目的

搭建一套基于云上环境的应用(ip访问版)

#### 准备云资源

- RDS （mysql）
- ECS  (虚拟服务器)

#### 环境依赖

- 服务器上需要部署好 git  docker 以及 docker-compose

#### 开始部署

- 数据库初始化

```bash
## 下载后端项目
cd ~
git clone https://github.com/caoyingjunz/pixiu.git
## 根据项目下的 pixiu/docs/sql.md 来进行数据库的初始化
cd pixiu/docs/
cat sql.md
## 对数据库执行初始化操作
```

- 部署后端服务

```bash
cd ~/pixiu/
## 构建docker镜像
cat Dockerfile ## 查看Dockerfile文件，并理解
## 执行构建命令
docker build -t  xxxx:latest .
## 编辑配置文件
vim config.yaml  ## 修改数据库配置
## 编写docker-compose.yaml文件
vim docker-compose.yaml
version: "3.9"
services:
  pixiu-backend:
    image: xxxx:latest
    container_name: pixiu-backend
    restart: always
    network_mode: host
    volumes:
    - ./config.yaml:/etc/pixiu/config.yaml
## 启动后端服务
docker-compose up -d 
## 查看日志，观察服务是否正常启动
docker logs -f containername 
```

- 部署前端服务

```bash
## 下载前端项目
cd ~
git clone https://github.com/pixiu-io/dashboard.git
cd dashboard/
## 构建docker镜像
ls docker ##里面包含nginx.conf Dockerfile等文件,可以理解理解
## 执行镜像构建命令
docker build  -t xxx:xxx  -f docker/Dockerfile .
## 编写docker-compose.yaml文件
vim docker-compose.yaml
version: "3.9"
services:
  pixiu-frontend:
    image: xxx:xxx
    container_name: pixiu-frontend
    restart: always
    network_mode: host
## 启动前端服务
docker-compose up -d 
## 查看日志，观察服务是否正常启动
docker logs -f containername 
```

- 浏览器查看页面

```bash
http://$ecs_ip
```

