---
comments: true
---

# Docker

Docker是非常重要的工具。当你需要部署服务的时候，用Docker准没错，能有效解决不同平台兼容性问题。

你可能经常遇到下面的几种情况：

“这个项目安装起来好麻烦啊，这么多步骤得弄上半天”

“诶，这个代码在我这儿运行就没事儿啊，怎么到你那儿就报错啊”

而这正是Docker技术大显身手的场景。通过将应用程序及其依赖封装为轻量级容器，Docker实现了：

- 开发/测试/生产环境的绝对一致性
- 依赖关系的完全隔离与管理
- 秒级的环境搭建与销毁能力
- 跨平台（Windows/macOS/Linux）的标准化运行

采用Docker后，团队不再需要维护冗长的环境配置文档，新人接入时间可从小时级缩短至分钟级。更重要的是，它能彻底杜绝"环境差异"导致的运行异常，让开发者真正实现"Build once, run anywhere"的理想工作模式。

# Docker基本介绍

[什么是Docker？看这一篇干货文章就够了！](https://zhuanlan.zhihu.com/p/187505981)

# 安装和配置

由于不可名状的原因，在国内使用Docker会有很多阻碍，Docker下载不了，镜像拉不下来之类的网络问题。可以参考以下文章：

[国内用户一键安装Docker并配置镜像源](https://www.yeluohuakai.com/posts/2024/09/4d4367cb)

[完美解决Docker pull时报错：https://registry-1.docker.io/v2/-CSDN博客](https://blog.csdn.net/qingzhumuqingfeng/article/details/144094325)

# 使用教程

[Docker 教程 | 菜鸟教程](https://www.runoob.com/docker/docker-tutorial.html)

Docker其实并不难，只需要知道这几个常用命令就可以了

### **一、镜像管理**

| 命令 | 说明 | 示例 |
| --- | --- | --- |
| `docker pull <镜像名>` | 从仓库下载镜像 | `docker pull nginx:latest` |
| `docker images` | 查看本地所有镜像 | `docker images` |
| `docker build -t <镜像名> .` | 根据 Dockerfile 构建镜像 | `docker build -t myapp:v1 .` |
| `docker rmi <镜像ID>` | 删除指定镜像 | `docker rmi abc123` |
| `docker image prune` | 清理无用镜像 | `docker image prune -a` |

**小贴士**：

- 镜像名格式为 `名称:标签`（如 `ubuntu:20.04`），默认标签为 `latest`
- 构建镜像时，确保在 Dockerfile 所在目录执行命令！

---

### **二、容器管理**

| 命令 | 说明 | 示例 |
| --- | --- | --- |
| `docker run [参数] <镜像名>` | 创建并启动容器 | `docker run -d -p 80:80 --name myweb nginx` |
| `docker ps` | 查看运行中的容器 | `docker ps -a`（显示全部） |
| `docker start/stop <容器名>` | 启动/停止容器 | `docker stop myweb` |
| `docker restart <容器名>` | 重启容器 | `docker restart myweb` |
| `docker exec -it <容器名> /bin/bash` | 进入容器终端 | `docker exec -it myweb bash` |
| `docker logs <容器名>` | 查看容器日志 | `docker logs -f myweb`（实时追踪） |
| `docker rm <容器名>` | 删除容器 | `docker rm myweb` |

**常用参数**：

- `d`：后台运行（守护进程）
- `p 宿主机端口:容器端口`：端口映射（如 `p 8080:80`）
- `v 宿主机路径:容器路径`：目录挂载（数据持久化）
- `-name`：指定容器名称（否则随机命名）
- `e`：设置环境变量（如 `e MYSQL_ROOT_PASSWORD=123`）

---

### **三、网络与存储**

| 命令 | 说明 | 示例 |
| --- | --- | --- |
| `docker network ls` | 查看所有网络 | `docker network ls` |
| `docker volume create` | 创建数据卷 | `docker volume create mydata` |
| `docker volume ls` | 查看所有数据卷 | `docker volume ls` |

---

### **四、实用组合命令**

1. **删除所有停止的容器**：
    
    ```bash
    docker container prune
    ```
    
2. **一键清理无用资源**（镜像/容器/网络/缓存）：
    
    ```bash
    docker system prune -a
    ```
    
3. **复制文件到容器**：
    
    ```bash
    docker cp 本地文件路径 容器名:容器路径
    ```
    
4. **查看容器资源占用**：
    
    ```bash
    docker stats
    ```
    

---

### **五、新手常见场景**

1. **快速启动一个 Web 服务**：
    
    ```bash
    docker run -d -p 8080:80 --name webserver nginx
    ```
    
    访问 `http://localhost:8080` 即可看到 Nginx 页面！
    
2. **通过命令行进入容器内部调试**：
    
    ```bash
    docker exec -it webserver bash

    // 基于ubuntu的container一般是这么进去查看的
    docker exec -it ubuntu /bin/bash
    ```
    
3. **保存容器修改为新镜像**：
    
    ```bash
    docker commit webserver mynginx:v2
    ```
    

---

**总结**：

- 镜像 = 模板，容器 = 根据模板运行的实例
- 操作顺序：下载镜像 → 运行容器 → 管理容器 → 清理资源
- 遇到问题多用 `docker logs` 看日志，善用 `docker exec` 进容器调试