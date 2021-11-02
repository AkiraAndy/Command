# Command

## Linux
1. 始終打開sudo權限
```
$ sudo -s
```

2. 查看系統log
```
$ dmesg -c
```

3. 查看port使用狀態
```
$ sudo netstat -tulpn
```

4. 背景執行並印出log
```
$ nohup ./orion &>orion.log &
```

5. Add arm toolchain compile file path on linux env path.
```
$ export PATH=/mnt/c/Users/user/Documents/DS-5\ Workspace/ToolChains/gcc-arm-8.2-2018.08-x86_64-arm-linux-gnueabihf/bin:$PATH
```
檢查是否新增成功
```
$ echo $PATH
```

---
## Nginx
1. 安裝Nginx
```
$ sudo apt-get update
$ sudo apt-get install nginx
```
*Nginx config file is at: /etc/nginx/nginx.conf

2. 移除Nginx
```
$ sudo apt-get remove nginx nginx-common
$ sudo apt-get purge nginx nginx-common
$ sudo apt-get autoremove
```

3. Start, Stop, Restart Nginx
```
$ sudo systemctl start nginx
$ sudo systemctl stop nginx
$ sudo systemctl restart nginx
```

### 使用docker image快速安裝
1. 下載Nginx image
```
$ docker pull nginx
```

2. 建立一個index.html並放置在~/galaxy/nginx，內容如下:
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Docker nginx</title>
</head>
<body>
  Hello world!
</body>
</html>
```

3. 替換nginx首頁，創建並啟動Nginx container
```
$ docker run --name nginx-container -p 7777:80 -v ~/galaxy/nginx:/usr/share/nginx/html:ro -d nginx
```

4. 在瀏覽器輸入localhost:7777會看到Hello world!

---
## Docker
1. 新增docker到sudo用戶組
```
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
```
如果以上沒用的話
```
$ sudo chown root:docker /var/run/docker.sock
$ sudo chown -R root:docker /var/run/docker
```
or
```
$ sudo chmod 777 /var/run/docker.sock
```

2. 查看volume list
```
$ docker volume list
```

### docker-compose
1. 安裝docker-compose
```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. 修改docker-compose执行权限
```
$ sudo chmod +x /usr/local/bin/docker-compose
```

3. 检查docker-compose是否安装成功
```
$ docker-compose --version
```

4. 啟動容器
```
$ docker-compose up CONTAINER_NAME
```
在背景執行
```
$ docker-compose up -d CONTAINER_NAME 
```

5. 停止所有容器
```
$ docker-compose stop
```

6. 關閉並刪除所有容器
```
$ docker-compose down
```

### Container
1. 創建容器
```
$ docker create it --name=CONTAINER_NAME CONTAINER_NAME 
```
創建並進入容器
```
$ docker run --rm --name=CONTAINER_NAME -it CONTAINER_NAME  sh
```

2. 啟動容器
```
$ docker start CONTAINER_NAME 
```
or
```
$ docker run -it --name=CONTAINER_NAME CONTAINER_NAME 
```
啟動容器至指定的port
```
$ docker run --name CONTAINER_NAME -p HOST_PORT:CONTIAINER_PORT -d IMAGE_NAME
```
啟動並進入容器
```
$ docker run -it CONTAINER_NAME:TAG sh
```

3. 進入容器

多個terminal共用一個進程
```
$ docker attach CONTAINER_NAME 
```
單獨啟動一個sh進程 (較常用)
```
$ docekr exec -it CONTAINER_NAME sh
```
or
```
$ docekr exec -it CONTAINER_NAME bash
```

4. 查看容器

查看運行中容器
```
$ docker ps
```
查看終止狀態容器
```
$ docker ps -a
```

5. 終止容器
```
$ docker stop CONTAINER_NAME 
```

6. 重啟容器
```
$ docker restart CONTAINER_NAME
```

7. 刪除容器

刪除停止狀態容器
```
$ docker rm CONTAINER_NAME
```
刪除運行中容器
```
$ docker rm -f CONTAINER_NAME
```
刪除所有容器
```
$ docker kill $(docker ps -a -q)
```

8. 導出/載入容器
```
$ docker export CONTAINER_NAME > CONTAINER_NAME.tar
$ docker load -i CONTAINER_NAME.tar
```

9. 退出容器
```
$ exit
```

10. 複製容器檔案到主機
```
$ docker cp CONTAINER_NAME:/path/to/file1 /hostPath/to/file2
```
複製主機目錄到容器
```
$ docker cp /hostPath/to/folder CONTAINER_NAME:/path/
```

11. 掛載目錄
```
$ docker run -d -P --name CONTAINER_NAME -v /host/folder:/path/folder
```

### Image
(不指定 tag, default 為 latest)
1. 拉取Image
```
$ docker pull IMAGE_NAME
```

2. 查看Image
```
$ docker images
$ docker image ls
```

3. 建立Image
   從當前目錄的Dockerfile構建Image
   ```
   $ docker build -t IMAGE_NAME .
   ```
   從運行中容器提交為Image
   ```
   $ docker cocmmit IMAGE_NAME IMAGE_NAME:TAG
   ```

4. 重新命名Image
```
$ docker tag IMAGE_NAME:TAG NEW_NAME:TAG
```

5. 刪除Image
```
$ docker rmi IMAGE_NAME
$ docker rmi IMAGE_NAME:TAG
$ docker image rm IMAGE_NAME
```
刪除沒用的Image
```
$ docker image prune
$ docker system prune
```

6. 載入Image
```
$ docker load -i IMAGE_NAME.tar
```

### Repository
1. 推送Image到Docker hub遠端倉庫

```
$ docker login
```
重新命名Image
```
$ docker tag IMAGE_NAME REPOSITORY_NAME/IMAGE_NAME
```

2. 推送到自己的遠端倉庫
```
$ docker push REPOSITORY_NAME/IMAGE_NAME
```

3. 推送Image到私有倉庫

啟動官方提供的本地倉庫Image
```
$ docker run -d -p 5000:5000 --name registry registry:2.7
```
重新命名
```
$ docker tag IMAGE_NAME localhost:5000/IMAGE_NAME
```

4. 推送到本地倉庫
```
$ docker push localhost:5000/IMAGE_NAME
```
從本地倉庫拉取Image
```
$ docker pull localhost:5000/busybox
```
持久化Image儲存到主機目錄
```
$ docker run -v /var/lib/registry/data:/var/lib/registry -d -p 5000:5000 --name registry registry:2.7
```

### Network
1. 列出所有docker network
```
$ docker network ls
```

2. 建立network
```
$ docker network create NETWORK_NAME
```

3. 刪除network
```
$ docker network rm NETWORK_NAME
```

4. 測試容器間網路是否連接
```
$ docker exec -it galaxy_elderwand2 ping -c 5 galaxy_silverstone
```

---
## KVM virsh
1. 列出所有VM
```
$ virsh list --all
```

2. 設定自動啟動
```
$ virsh autostart vmName
```

3. 列出自動啟動的VM
```
$ virsh list --autostart
```

4. 取得vnc埠號
```
$ sudo virsh vncdisplay vmName
```