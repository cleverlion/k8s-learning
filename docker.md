#镜像
docker search
docker pull xx
docker images
docker rmi

#容器：
docker run 
	-d 后台启动
	--name 起的名称
	-p 端口映射 8080:80
docker ps 
docker ps -a
docker stop
docker start
docker restart
docker stats
docker log id/name
docker exec
	-it name /bin/bash 交互模式
	exit 退出容器
docker rm

#分享
#保存镜像
docker commit
	-m 
	eg: docker commit -m "xx" mynginx myginx:v1.0
docker save
	-o 打包的文件名
	eg: docker save -o myginx.tar myginx:v1.0
docker load
	-i 文件路径，被打包的文件名

#上传社区
docker login
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG](用户名/mynginx)
docker push TARGET_IMAGE[:TAG]
