---
title: VSCode Remote WLS 연동
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: Setting
---

# step01. VSCode 설치

VSCode를 설치한다.****

- 설치 주소 : **[https://code.visualstudio.com/download](https://code.visualstudio.com/download)**
- 관리자로 실행할 것이기 때문에 **System Installer**로 ****다운로드 받는다.

![Untitled](images/VSCode/Untitled.png)

설치 시, 환경변수 체크란만 잘 확인한다.

![Untitled](images/VSCode/Untitled%201.png)

설치 후 재부팅을 한다.

# step 02. ****Remote WSL 연동****

VSCode를 실행한 후, 우측에 Extension 버튼을 클릭한다.

![Untitled](images/VSCode/Untitled%202.png)

검색창에서 Remote WSL을 검색 후, 설치를 진행한다.

![Untitled](images/VSCode/Untitled%203.png)

모두 클릭 후, Mark Done을 선택한다

![Untitled](images/VSCode/Untitled%204.png)

Open Folder를 클릭한다.

![Untitled](images/VSCode/Untitled%205.png)

WSL에서 설치했던 airflow 폴더를 선택하여 해당 프로젝트를 연다.

- 체크란에 체크를 하고 Yes를 누른다.

![Untitled](images/VSCode/Untitled%206.png)

메뉴바에 Terminal을 선택 후, 화면 하단에서 WSL이 있는지 확인한다.

![Untitled](images/VSCode/Untitled%207.png)

WSL을 클릭하면 아래와 같이 터미널이 변경된 것을 확인할 수 있다.

![Untitled](images/VSCode/Untitled%208.png)

이제 간단한 Python 파일을 실행해본다.

- main.py

```powershell
#!/mnt/c/Programmings/airflow-test/venv/bin/python3
#-*- coding: utf-8 -*-

def main():
    print("Hello, World!")
    print("Hello, World!")

if __name__ == "__main__":
    main()
```

이제 실행하면 정상적으로 출력이 되는 것을 확인할 수 있다.

```powershell
username@username:/mnt/c/Programmings/airflow-test$ source venv/bin/activate
(venv) username@username:/mnt/c/Programmings/airflow-test$ python3 main.py 
Hello, World!
Hello, World!
```
