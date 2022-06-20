---
title: Airflow 데이터 파이프라인 구축 예제
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: Setting
---
# **Step 01. Dags 폴더 생성**

프로젝트 폴더(=airflow) 하단에 Dags 폴더를 만든다.

- dags 폴더를 확인한다.

```powershell
$ ls
airflow.cfg  airflow.db  dags  logs  venv  webserver_config.py
```

# **Step 02. 가상의 데이터 생성**

이번 테스트에서 사용할 라이브러리가 없다면 우선 설치한다.

```powershell
$ pip3 install faker pandas
```

faker 라이브러리를 활용하여 가상의 데이터를 생성한다. (파일 경로 : data/step01_writecsv.py)

```powershell
from faker import Faker
import csv
output=open('data.csv','w')
fake=Faker()
header=['name','age','street','city','state','zip','lng','lat']
mywriter=csv.writer(output)
mywriter.writerow(header)
for r in range(1000):
    mywriter.writerow([fake.name(),fake.random_int(min=18, max=80, step=1), fake.street_address(), fake.city(),fake.state(),fake.zipcode(),fake.longitude(),fake.latitude()])
output.close()
```

생성된 후, 파일을 확인하도록 한다.

```powershell
xiromjr@xiromjr:/mnt/c/airflow-test/data$ ls
data.csv  step01_writecsv.py
```

# **Step 03. csv2json 파일 구축**

이번에는 CSV와 JSON 변환 파일을 구축하는 코드를 작성한다. (파일 경로 : dags/**[csv2json.py](http://csv2json.py/)**)\

주요 목적 함수 csvToJson()의 역할은 `data/data.csv` 파일을 불러와서 `fromAirflow.json` 파일로 변경하는 것이다.

DAG는 csvToJson 함수를 하나의 작업으로 등록하는 과정을 담는다. 작업의 소유자, 시작일시, 실패 시 재시도 횟수, 재시도 지연시 시간을 지정한다.

- 자세한 옵션은 도움말을 참조한다. **[https://airflow.apache.org/docs/apache-airflow/stable/concepts/dags.html](https://airflow.apache.org/docs/apache-airflow/stable/concepts/dags.html)**

`print_starting >> csvJson` 에서 `>>` 는 하류 설정 연산자라고 부른다. (동의어 비트 자리이동 연산자)

```powershell
# import libaries
import datetime as dt 
from datetime import timedelta 

from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator

import pandas as pd 

# load data
def csvToJson():
    df = pd.read_csv('data.csv')
    # print(df.info())
    for i, r in df.iterrows():
        print(r['name'])

    # Export JSON
    df.to_json('fromAirflow.json', orient="records")

default_args = {
    'owner' : 'evan', 
    'start_date' : dt.datetime(2020, 3, 18), 
    'retries' : 1, 
    'retry_delay' : dt.timedelta(minutes=5)
}

# 참조 : https://ponyozzang.tistory.com/402
with DAG('MyCSVDAG', 
            default_args=default_args, 
            schedule_interval=timedelta(minutes=5), 
            # '0 * * * *'
        ) as dag:
    print_starting = BashOperator(task_id = "starting", 
                                  bash_command='echo "I am reading the CSV now......"')
    
    CSVJson = PythonOperator(task_id = "convertCSVtoJson", 
                             python_callable=csvToJson)

print_starting >> CSVJson

# if __name__ == "__main__":
#    csvToJson()
```

# **Step 04. Airflow Webserver 및 Scheduler 동시 실행**

이제 웹서버와 스케쥴러를 동시에 실행한다. (터미널을 2개 열어야 함에 주의한다.)

```powershell
$ airflow webserver -p 8080
$ airflow scheduler
```

이제 WebUI를 확인하면 정상적으로 작동하는 것을 확인할 수 있다.

![Untitled](images/Airflow/Untitled.png)