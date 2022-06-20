---
title: ElasticSearch & Kibana 설치 in WSL2
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: Setting
---
# **Step 1. 사전 필수 패키지 설치**

우선 시스템 패키지를 업데이트 하고, HTTPS와 관련된 패키지를 설치한다.

```powershell
$ sudo apt update
$ sudo apt install apt-transport-https
```

자바를 설치한다.

- 이미 설치가 되어 있다면 버전만 확인한다.

```powershell
$ sudo apt install openjdk-11-jdk
```

설치한 버전을 확인한다.

```powershell
$ java -version
openjdk version "11.0.14.1" 2022-02-08
OpenJDK Runtime Environment (build 11.0.14.1+1-Ubuntu-0ubuntu1.20.04)
OpenJDK 64-Bit Server VM (build 11.0.14.1+1-Ubuntu-0ubuntu1.20.04, mixed mode, sharing)
```

자바 환경 변수를 설정하기 위해 아래와 같이 에디터를 입력한다.

```powershell
$ sudo vi /etc/environment
```

그리고 아래와 같이 추가한다.

```
JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
```

환경변수를 업데이트 한다.

```powershell
$ source /etc/environment
```

실제 경로가 나오는지 확인한다.

```powershell
$ echo $JAVA_HOME
/usr/lib/jvm/java-11-openjdk-amd64
```

# **Step 2. ElasticSearch 설치**

GPG Keys를 확인하여 설치를 진행한다.

```powershell
$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
OK
```

라이브러리를 아래와 같이 추가한다.

```powershell
$ sudo sh -c 'echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list'
```

이제 elasticsearch를 설치한다.

```powershell
$ sudo apt-get update
Hit:1 https://artifacts.elastic.co/packages/7.x/apt stable InRelease
Hit:2 http://security.ubuntu.com/ubuntu focal-security InRelease
Hit:3 http://archive.ubuntu.com/ubuntu focal InRelease
Hit:4 http://archive.ubuntu.com/ubuntu focal-updates InRelease
Hit:5 http://archive.ubuntu.com/ubuntu focal-backports InRelease
Reading package lists... Done

$ sudo apt-get install elasticsearch
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  elasticsearch
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 312 MB of archives.
After this operation, 517 MB of additional disk space will be used.
Get:1 https://artifacts.elastic.co/packages/7.x/apt stable/main amd64 elasticsearch amd64 7.17.2 [312 MB]
Fetched 312 MB in 8s (40.9 MB/s)
Selecting previously unselected package elasticsearch.
(Reading database ... 32942 files and directories currently installed.)
Preparing to unpack .../elasticsearch_7.17.2_amd64.deb ...
Creating elasticsearch group... OK
Creating elasticsearch user... OK
Unpacking elasticsearch (7.17.2) ...
Setting up elasticsearch (7.17.2) ...
### NOT starting on installation, please execute the following statements to configure elasticsearch service to start automatically using systemd
 sudo systemctl daemon-reload
 sudo systemctl enable elasticsearch.service
### You can start elasticsearch service by executing
 sudo systemctl start elasticsearch.service
warning: usage of JAVA_HOME is deprecated, use ES_JAVA_HOME
Created elasticsearch keystore in /etc/elasticsearch/elasticsearch.keystore
Processing triggers for systemd (245.4-4ubuntu3.15) ...
```

# **Step 3. Elasticsearch 서비스 시작**

이번에는 elasticsearch 서비스를 시작한다.

```powershell
$ sudo systemctl start elasticsearch
```

만약 아래와 같은 에러 메시지가 뜬다면, 추가해야 한다.

- 참조 : **[https://askubuntu.com/questions/1379425/system-has-not-been-booted-with-systemd-as-init-system-pid-1-cant-operate](https://askubuntu.com/questions/1379425/system-has-not-been-booted-with-systemd-as-init-system-pid-1-cant-operate)**

```
System has not been booted with systemd as init system (PID 1).
Can't operate. Failed to connect to bus: Host is down
```

다음 명령어를 추가한다.

```powershell
$ sudo -b unshare --pid --fork --mount-proc /lib/systemd/systemd --system-unit=basic.target
$ sudo -E nsenter --all -t $(pgrep -xo systemd) runuser -P -l $USER -c "exec $SHELL"
```

서비스가 가능하도록 한다.

```powershell
$ sudo systemctl enable elasticsearch
Synchronizing state of elasticsearch.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable elasticsearch
Created symlink /etc/systemd/system/multi-user.target.wants/elasticsearch.service → /lib/systemd/system/elasticsearch.service.
```

서비스를 시작한다.

```powershell
$ sudo systemctl start elasticsearch
```

실제 서비스가 작동하는지 확인한다.

```powershell
$ curl -X GET "localhost:9200/"
{
  "name" : "XXXX",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "vATWEOO1T9yOFGLc7G3L4w",
  "version" : {
    "number" : "7.17.2",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "de7261de50d90919ae53b0eff9413fd7e5307301",
    "build_date" : "2022-03-28T15:12:21.446567561Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

만약 위와 같은 메시지가 뜨면, 윈도우 화면에서 재 확인해본다.

![Untitled](images/ElasticSearchKibana/Untitled.png)
images/ElasticSearchKibana
# **Step 4. Kibana 설치 및 서비스 시작**

우선 kibana를 설치한다.

```powershell
$ sudo apt-get install kibana
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  kibana
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 286 MB of archives.
After this operation, 771 MB of additional disk space will be used.
Get:1 https://artifacts.elastic.co/packages/7.x/apt stable/main amd64 kibana amd64 7.17.2 [286 MB]
Fetched 286 MB in 7s (42.5 MB/s)
Selecting previously unselected package kibana.
(Reading database ... 34067 files and directories currently installed.)
Preparing to unpack .../kibana_7.17.2_amd64.deb ...
Unpacking kibana (7.17.2) ...
Setting up kibana (7.17.2) ...
Creating kibana group... OK
Creating kibana user... OK
Created Kibana keystore in /etc/kibana/kibana.keystore
Processing triggers for systemd (245.4-4ubuntu3.15) ...
```

서비스를 활성화 한다.

- 서비스를 시작하고, 확인해본다.

```powershell
$ sudo systemctl start kibana
$ sudo systemctl status kibana
● kibana.service - Kibana
     Loaded: loaded (/etc/systemd/system/kibana.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2022-04-09 00:41:40 KST; 12s ago
       Docs: https://www.elastic.co
   Main PID: 4203 (node)
      Tasks: 11 (limit: 9370)
     Memory: 670.7M
     CGroup: /system.slice/kibana.service
             └─4203 /usr/share/kibana/bin/../node/bin/node /usr/share/kibana/bin/../src/cli/dist --logging.dest=/var/lo>

Apr 09 00:41:40 evan systemd[1]: Started Kibana.
```

# **Step 5. Kibana WebUI 확인**

**[http://localhost:5601/](http://localhost:5601/)** 에서 확인해본다.

![https://dschloe.github.io/img/settings/elasticsearch_kibana_wsl2/Untitled%201.png](https://dschloe.github.io/img/settings/elasticsearch_kibana_wsl2/Untitled%201.png)