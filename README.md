## 🚀 Jenkins Setup Guide

### 1. Jenkins Controller (Docker)

**Build Image**
```bash
docker build -t ltscustomjenkins -f Dockerfile.JenkinsController .
```

**Create Volume**
```bash
docker volume create jenkins_home
```

**Run Container**
```bash
docker run --name jenkins_master -dit --restart=always \
    -p 8080:8080 -p 50000:50000 \
    -v jenkins_home:/var/jenkins_home \
    -v /var/run/docker.sock:/var/run/docker.sock \
    ltscustomjenkins
```

**Add Docker Privileges for Jenkins User**
```bash
docker exec -it -u root jenkins_master bash
chmod 666 /var/run/docker.sock
gpasswd -a jenkins docker
```

**(Optional) Rebuild Image from Container**
```bash
docker commit jenkins_master ltscustomjenkins
```

---

### 2. Jenkins Agent (Docker)

เพิ่ม node ใน Jenkins Controller GUI จากนั้นนำข้อมูลต่าง ๆ มาใส่แทนใน `Dockerfile.JenkinsAgent` จากนั้นค่อยทำขั้นตอนด้านล่าง

**Build Image**
```bash
docker build -t jenkins-agent -f Dockerfile.JenkinsAgent .
```

**Create Volume**
```bash
docker volume create jenkins_agent_home
```

**Run Container**
```bash
docker run -d --name jenkins-agent \
    -v jenkins_agent_home:/home/jenkins/agent \
    -v /var/run/docker.sock:/var/run/docker.sock \
    --restart unless-stopped \
    jenkins-agent
```