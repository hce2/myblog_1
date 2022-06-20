---
title: Apache NiFi 설치와 설정 in WSL2
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: Setting
---
wsl2에서 JAVA 설치 한다.

```powershell
$ sudo apt-get update && sudo apt-get upgrade
$ sudo apt install openjdk-11-jre-headless
$ vi ~/.bash_profile
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

curl을 이용해서 NiFi를 현재 경로에 내려받는다.

```powershell
$ sudo wget https://downloads.apache.org/nifi/1.16.0/nifi-1.16.0-bin.tar.gz
```

.tar.gz 파일의 압축을 푼다.

```powershell
$ sudo tar xvzf nifi-1.16.0-bin.tar.gz
```

압축 파일을 푼 다음에는 cd nifi-1.16.0 폴더에 접속을 한다.

```powershell
$ cd nifi-1.16.0/bin
```

ls를 실행해서 **[nifi-env.sh](http://nifi-env.sh/)** 파일이 있는지 확인하고 있다면, vi 에디터로 연다.

- .bash_profile에서 한 것처럼 동일하게 자바 환경변수를 잡아준다.

```powershell
$ sudo vi nifi-env.sh
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
```

그리고, **[nifi-env.sh](http://nifi-env.sh/)** 파일을 실행한다.

```powershell
$ sudo ./nifi.sh start

Java home: /usr/lib/jvm/java-11-openjdk-amd64
NiFi home: /nifi-1.16.0

Bootstrap Config File: /nifi-1.16.0/conf/bootstrap.conf
```

webserver 주소를 확인한다.

```powershell
/nifi-1.16.0/conf$ cd .. 
/nifi-1.16.0/conf$ vi nifi.properties
```

![https://dschloe.github.io/img/settings/apache_nifi_wsl2/Untitled.png](https://dschloe.github.io/img/settings/apache_nifi_wsl2/Untitled.png)

NiFi 화면에 접속한다. **[https://127.0.0.1:8443/nifi/login](https://127.0.0.1:8443/nifi/login)**

![https://dschloe.github.io/img/settings/apache_nifi_wsl2/Untitled%201.png](https://dschloe.github.io/img/settings/apache_nifi_wsl2/Untitled%201.png)

로그인을 하려면, 기본적으로 Username과 Password를 별도로 입력해야 한다.

1. username : XXXX
2. password : 1234567890123

```powershell
$ sudo ./bin/nifi.sh set-single-user-credentials XXXX 1234567890123
```

그리고 재 실행을 해본다.

```powershell
sudo ./bin/nifi.sh start
```

정상적으로 username과 password를 입력했다면, 아래와 같은 화면에 접속한 것을 확인할 수 있다.

![https://dschloe.github.io/img/settings/apache_nifi_wsl2/Untitled%202.png](https://dschloe.github.io/img/settings/apache_nifi_wsl2/Untitled%202.png)