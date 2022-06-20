---
title: Setting up Apache-Airflow in Windows using WSL2
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: Setting
---
# **Step 1. Install pip on WSL**

airflow를 설치하기 위해 pip를 설치한다.

```powershell
$ sudo apt install python3-pip
[sudo] password for username:
```

# **Step 2. Install virtualenv package**

virtualenv 라이브러리를 설치한다.

```powershell
$ sudo pip3 install virtualenv
```

# **Step 3. Create a virtual environment**

C드라이브에 airflow 폴더를 생성한다. 해당 디렉터리로 이동한 후, 가상환경을 생성한다.

```powershell
$ virtualenv venv
```

가상환경에 접속한다.

```powershell
$ source venv/bin/activate
```

이번에는 .bashrc 파일을 수정한다.

```powershell
$ vi ~/.bashrc
```

파일을 연 후, 다음과 같은 코드를 추가한다.

```powershell
export AIRFLOW_HOME=/mnt/c/airflow
```

파일을 닫을 때는 ESC → :wq 순서대로 작성한다.

![Untitled](images/Apache-Airflow/Untitled.png)

수정된 코드를 업데이트 하기 위해서는 아래와 같이 반영한다.

```powershell
$ source ~/.bashrc
```

실제로 코드가 반영되었는지 확인하기 위해서는 다음과 같이 확인해본다.

```powershell
echo $AIRFLOW_HOME
/mnt/c/airflow
```

# **Step 4. Apache Airflow 설치**

PostgreSQL, Slack, Celery 패키지를 동시에 설치하는 코드를 작성한다.

```powershell
$ pip3 install 'apache-airflow[postgres, slack, celery]'
```

에어플로 실행 위해 DB 초기화를 해줘야 한다.

```powershell
$ airflow db init
```

실제로 잘 구현이 되었는지 확인하기 위해 webserver를 실행한다.

- ‘-p’로 임의의 포트 번호를 정해준다.

```powershell
$ airflow webserver -p 8081
```

다음으로 일정 주기로 데이터 흐름이 실행되게 하려면 Scheduler가 필요하다.

```powershell
$ airflow scheduler
```

그리고, 해당 링크 **[http://localhost:8081/login/](http://localhost:8081/login/)** 에 접속하면 아래와 같은 화면이 나타난다.

![images/Apache-Airflow/Untitled.png](images/Apache-Airflow/Untitled.png)

그런데, 여기에서 문제는 username을 생성하지 않았다. 따라서, username을 추가하도록 한다.

- username, password, firstname, lastname, email을 추가한다.

```powershell
# $ airflow users create --username admin --password your_password --firstname your_first_name --lastname your_last_name --role Admin --email your_email@some.com
$ airflow users create --username airflow --password airflow --firstname xiromjr --lastname airflow --role Admin --email your_email@some.com
```

그리고, 다시 Webserver와 Scheduler를 동시에 실행한다. 로그인 화면에서 로그인을 진행하면 정상적으로 다양한 DAGs 파일이 나타난 것을 확인할 수 있다.

![images/Apache-Airflow/Untitled%201.png](images/Apache-Airflow/Untitled%201.png)

# **Step 5. Default 예제 제거하기**

만약에 load_examples를 없애고 싶다면 어떻게 해야할까?

먼저, `airflow.cfg` 파일을 열고, `load_examples = True` 로 되어 있는 것을 `load_examples = False` 로 변경한다.

![images/Apache-Airflow/cfg.png](images/Apache-Airflow/cfg.png)

그 후에, 다시 터미널로 돌아와서 `airflow db reset` 실행한다. Proceed? y 선택

- 모든 터미널을 멈추고 reset을 실행해야 한다.

```powershell
$ airflow db reset
/mnt/c/airflow-test/venv/lib/python3.8/site-packages/airflow/configuration.py:276: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.
  if StrictVersion(sqlite3.sqlite_version) < StrictVersion(min_sqlite_version):
DB: sqlite:////mnt/c/airflow-test/airflow.db
This will drop existing tables if they exist. Proceed? (y/n)
.
.
.
INFO  [alembic.runtime.migration] Running upgrade 7b2661a43ba3 -> be2bfac3da23, Add has_import_errors column to DagModel
INFO  [alembic.runtime.migration] Running upgrade be2bfac3da23 -> c381b21cb7e4, add session table to db
INFO  [alembic.runtime.migration] Running upgrade c381b21cb7e4 -> 587bdf053233, adding index for dag_id in job
WARNI [airflow.models.crypto] empty cryptography key - values will not be stored encrypted.
INFO  [airflow.models.dagbag.DagBag] Filling up the DagBag from /mnt/c/airflow-test/dags
```

이제 `webserver` 를 다시 실행한다.

- 새로운 로그인 화면이 나오는 것이 정상이다.

```powershell
$ airflow webserver -p 8080
```

![images/Apache-Airflow/Untitled%202.png](images/Apache-Airflow/Untitled%202.png)

이제 접속하면 아래와 같이 아무런 예제가 떠오르지 않는 것을 확인할 수 있다.

![images/Apache-Airflow/Untitled%203.png](images/Apache-Airflow/Untitled%203.png)