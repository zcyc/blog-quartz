---
title: "Running Jenkins in Docker"
---


```yaml
version: "3"
services:
  docker_jenkins:
    restart: always
    image: jenkins/jenkins:lts
    container_name: docker_jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - ./data/:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
      - /usr/bin/docker:/usr/bin/docker
      - /usr/lib/x86_64-linux-gnu/libltdl.so.7:/usr/lib/x86_64-linux-gnu/libltdl.so.7
```

启动的时候会报错

```yaml
touch: cannot touch '/var/jenkins_home/copy_reference_file.log': Permission denied
Can not write to /var/jenkins_home/copy_reference_file.log. Wrong volume permissions?
```

解决方案是

```yaml
mkdir ./data
sudo chown -R 1000 ./data
```
