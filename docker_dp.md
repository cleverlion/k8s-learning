一、镜像操作
docker images	列出本地所有镜像
docker pull <镜像名>	从仓库拉取镜像（如 docker pull nginx）
docker push <镜像名>	将镜像推送到仓库
docker search <关键词>	在 Docker Hub 上搜索镜像
docker rmi <镜像ID>	删除本地镜像（先删除依赖的容器）
docker tag <源镜像> <目标标签>	给镜像打标签
docker build -t <名称:标签> .	根据 Dockerfile 构建镜像（-t 指定名称）
docker history <镜像名>	查看镜像的构建历史

二、容器操作
docker run <镜像>	创建并启动容器
docker run -d --name <容器名> <镜像>	后台运行，指定容器名
docker run -it <镜像> bash	交互式运行（进入容器内部）
docker run -p 宿主机端口:容器端口 <镜像>	端口映射
docker run -v 宿主机目录:容器目录 <镜像>	挂载数据卷
docker ps	列出运行中的容器
docker ps -a	列出所有容器（包括已停止）
docker stop <容器ID/名>	停止容器
docker start <容器ID/名>	启动已停止的容器
docker restart <容器ID/名>	重启容器
docker rm <容器ID/名>	删除容器（需先停止）
docker rm -f <容器ID/名>	强制删除运行中的容器
docker exec -it <容器ID/名> bash	进入运行中的容器（退出用 exit）
docker logs <容器ID/名>	查看容器日志
docker logs -f <容器ID/名>	实时跟踪日志
docker inspect <容器ID/名>	查看容器详细信息（JSON 格式）
docker top <容器ID/名>	查看容器内的进程
docker cp <容器ID:源路径> <宿主机目标路径>	从容器复制文件到宿主机
docker cp <宿主机源路径> <容器ID:目标路径>	从宿主机复制文件到容器

三、容器生命周期批量操作
docker stop $(docker ps -q)	停止所有运行中的容器
docker rm $(docker ps -aq)	删除所有容器（包括已停止）
docker rm -f $(docker ps -aq)	强制删除所有容器
docker prune	删除所有已停止的容器（交互式确认）
docker container prune -f	直接删除所有已停止容器（不确认）

四、镜像/容器清理
命令	说明
docker image prune	删除 dangling 镜像（无标签）
docker image prune -a	删除所有未被使用的镜像（小心）
docker system prune	清理停止的容器、无用网络、dangling 镜像、构建缓存
docker system prune -a --volumes	彻底清理所有未使用资源（包括数据卷）

五、数据卷操作
命令	说明
docker volume ls	列出所有数据卷
docker volume create <卷名>	创建数据卷
docker volume inspect <卷名>	查看数据卷详情
docker volume rm <卷名>	删除数据卷
docker volume prune	删除所有未使用的数据卷

六、网络操作
命令	说明
docker network ls	列出所有网络
docker network create <网络名>	创建自定义网络（bridge 类型）
docker network inspect <网络名>	查看网络详情
docker network connect <网络名> <容器名>	将容器连接到网络
docker network disconnect <网络名> <容器名>	断开容器与网络的连接
docker network rm <网络名>	删除网络
docker network prune	删除所有未使用的网络

七、Docker Compose（多容器编排）
命令	说明
docker-compose up -d	启动服务（后台）
docker-compose down	停止并删除容器、网络
docker-compose down -v	同时删除数据卷
docker-compose ps	查看服务状态
docker-compose logs -f	实时查看日志
docker-compose exec <服务名> bash	进入某个服务容器
docker-compose restart	重启所有服务
docker-compose build	重新构建镜像

八、系统信息
命令	说明
docker version	显示客户端和服务端版本信息
docker info	显示系统级详细信息（容器数、镜像数、存储驱动等）
docker system df	查看磁盘使用情况（镜像、容器、卷）
docker events	实时打印 Docker 守护进程事件

九、常用参数速查
参数	适用命令	说明
-d	run	后台运行
-it	run, exec	交互式终端（常用于 bash）
-p <宿主机端口>:<容器端口>	run	端口映射
-v <宿主机目录>:<容器目录>	run	挂载宿主机目录
--name <名称>	run	指定容器名称
--rm	run	容器退出后自动删除
-e KEY=value	run	设置环境变量
--network <网络名>	run	指定容器使用的网络
-w <工作目录>	run	指定容器内的工作目录
-u <用户ID>	run	以指定用户身份运行
--restart=always	run	容器异常时自动重启
--memory="256m"	run	限制内存使用
--cpus="0.5"	run	限制 CPU 使用

十、实用组合技巧
目的	命令
进入容器调试	docker exec -it <容器名> bash 或 sh
查看容器 IP	docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <容器名>
删除所有未使用镜像	docker image prune -a -f
实时查看所有容器资源占用	docker stats
复制文件到容器	docker cp ./index.html <容器名>:/usr/share/nginx/html/
查看容器日志最后 100 行	docker logs --tail 100 <容器名>
停止并删除容器（不询问）	docker rm -f <容器名>
