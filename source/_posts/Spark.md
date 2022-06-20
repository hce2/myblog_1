---
title: Spark Installation on Windows
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: Setting
---
# step 01. 자바 설치

자바를 설치한다. 

- 설치파일 : **[Java SE 8 Archive Downloads (JDK 8u211 and later)](https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html)**
- 오라클 로그인이 필요 할 수도 있다.

다운로드 파일을 관리자로 실행한다. Next 버튼 클릭 후, 아래 파일에서 경로를 수정한다.

- 파일 경로에  `Program Files` 공백이 있는데, 이러한 공백은 환경 설치 시 문제가 될 수 있다.

![Untitled](images/Spark/Untitled.png)

C드라이브에 `jdk` 파일을 생성하고 경로를 아래와 같이 변경한다.

![Untitled](images/Spark/Untitled%201.png)

C드라이브에 `jre` 파일을 생성하고 자바의 런타임 환경의 폴더도 변경해준다.

![Untitled](images/Spark/Untitled%202.png)
-
# step 02. Spark 설치

## (1) ****설치파일 다운로드****

Spark를 설치한다.

- 설치 주소 : h**[ttps://www.apache.org/dyn/closer.lua/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz](https://www.apache.org/dyn/closer.lua/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz)** (2022년 1월 기준)
- 아래의 ‘The object is in our archive’를 다운 받는다.

![Untitled](images/Spark/Untitled%203.png)

## ****(2) WinRAR 프로그램 다운로드****

`.tgz` 압축파일을 풀기 위해서는 `WinRAR` 을 설치한다.

- 설치 파일: **[https://www.rarlab.com/download.htm](https://www.rarlab.com/download.htm)**
- `WinRAR x64 (64 bit) 6.11` 을 다운 받는다.

![Untitled](images/Spark/Untitled%204.png)

## ****(3) spark 폴더 생성 및 파일 이동****

C드라이브에 spark 폴더를 생성 후, spark-3.2.0-bin-hadoop3.2 폴더 내 모든 파일을 복사한다.

![Untitled](images/Spark/Untitled%205.png)

## ****(4) log4j.properties 파일 수정****

• `conf` - `[log4j.properties.template` 파일을 메모장으로 연다.

![Untitled](images/Spark/Untitled%206.png)

아래에서 `INFO` → `ERROR` 로 변경한다.

- 작업 실행 시, 출력하는 모든 logs 값들을 없앨 수 있다.

![Untitled](images/Spark/Untitled%207.png)

# step 03. ****winutils 설치****

이번에는 스파크가 윈도우 로컬 컴퓨터가 Hadoop으로 착각하게 만들 프로그램이 필요하다.

- 설치파일: **[https://github.com/cdarlint/winutils](https://github.com/cdarlint/winutils)**
    - 버전에 맞는 파일에 들어가 winutils.exe 파일을 다운로드 받는다.
- 필자는 3.2.0 버전을 다운로드 받았다.

![Untitled](images/Spark/Untitled%208.png)

C드라이브에서 `winutils\bin` 폴더를 차례로 생성한 후, 다운로드 받은 파일을 저장한다.

![Untitled](images/Spark/Untitled%209.png)

이 파일이 Spark 실행 시, 오류 없이 실행될 수 있도록 파일 사용 권한을 얻도록 한다.

- C드라이브 하단에, `tmp\hive` 폴더를 차례대로 생성을 한다.
    
    ![Untitled](images/Spark/Untitled%2010.png)
    

- 이 때에는 CMD 관리자 권한으로 파일을 열어서 실행한다.

```python
C:\Windows\system32>cd c:\winutils\bin
c:\winutils\bin> winutils.exe chmod 777 \tmp\hive
```

# step 04. ****환경변수 설정****

시스템 환경변수를 설정한다.

- 시스템 환경 변수 편집에 들어가 환경 변수는 클릭한다.
- 각 사용자 계정에 `사용자 변수 - 새로 만들기 버튼`을 클릭한다.

![Untitled](images/Spark/Untitled%2011.png)

SPARK_HOME 환경변수를 설정한다.

![Untitled](images/Spark/Untitled%2012.png)

JAVA_HOME 환경변수를 설정한다.

![Untitled](images/Spark/Untitled%2013.png)

HADOOP_HOME 환경변수를 설정한다.

![Untitled](images/Spark/Untitled%2014.png)

PATH 변수를 편집한다. 

![Untitled](images/Spark/Untitled%2015.png)

아래의 코드를 추가한다.

- %SPARK_HOME%\bin
- %JAVA_HOME%\bin

![Untitled](images/Spark/Untitled%2016.png)

# step 05. ****파이썬 환경설정****

Python 환경설정을 추가한다.

![Untitled](images/Spark/Untitled%2017.png)

# step 06. Spark Test

CMD를 관리자로 열고 `c:\spark` 폴더로 경로를 설정 한다.

- `pyspark` 를 입력한다.

![Untitled](images/Spark/Untitled%2018.png)

이번에는 해당 `[README.md](http://README.md)` 파일을 불러와서 아래 코드가 실행되는지 확인한다.

```python
>>> rd = sc.textFile("README.md")
>>> rd.count()
109
```