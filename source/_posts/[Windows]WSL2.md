---
title: [Windows]WSL2 설치하는 방법
date: 2022-05-02 17:10:05
tags:
- Hexo
- Github
- blog
- 헥소
categories: Setting
---

# step 01. 버전 확인

WSL을 설치하려면 Windows 10의 20H1 이상 버전이어야 한다.  20H1, 20H2, 21H1 버전을 사용해야 설치가 가능하다.

Windows + S 키를 클릭하고 PC 정보를 검색해서 실행한다.

![Untitled](images/[Windows]WSL2/Untitled.png)

스크롤을 내리면 Windows 사양을 확인할 수 있다. 여기서 버전이 20H1, 20H2, 21H1, 21H2 혹은 그보다 높은 버전인지 확인한다.

![Untitled](images/[Windows]WSL2/Untitled%201.png)

# setp 02. **DISM으로 WSL 관련 기능 활성화**

Windowsn + S 키로 Windows Terminal이나 PowerShell을 검색한 후 오른쪽 버튼을 눌러 ’관리자로 실행’을 선택한다.

DISM(배포 이미지 서비스 및 관리) 명령어로 `Microsoft-Windows-Subsystem-Linux`
 기능을 활성화한다.

```powershell
$ dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

배포 이미지 서비스 및 관리 도구
버전: 10.0.19041.844

이미지 버전: 10.0.19044.1586

기능을 사용하도록 설정하는 중
[==========================100.0%==========================]
작업을 완료했습니다.
```

다음으로 dism 명령어로 `VirtualMachinePlatform` 기능을 활성화한다.

```powershell
$ dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

배포 이미지 서비스 및 관리 도구
버전: 10.0.19041.844

이미지 버전: 10.0.19044.1586

기능을 사용하도록 설정하는 중
[==========================100.0%==========================]
작업을 완료했습니다.
```

작업이 정상적으로 완료되었는지 메시지 확인 후 재부팅을 해준다. 터미널이 관리자 권한이 아닌 경우 작업이 실패하기 때문에 꼭 관리자 권한으로 실행해야 한다. 

# setp 03. **WSL2 Linux 커널 업데이트**

다음으로 WSL2 Linux 커널 업데이트를 진행한다.

- 설치 파일 : [x64 머신용 최신 WSL2 Linux 커널 업데이트 패키지](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

![Untitled](images/[Windows]WSL2/Untitled%202.png)

윈도우 터미널을 열고, 다음 명령어를 실행해, 기본적으로 사용할 WSL 버전을 2로 변경해준다.

```powershell
$ wsl --set-default-version 2
```

# setp 04. **마이크로소프트 스토어에서 리눅스 설치**

마이크로소프트 스토어(Microsoft Store) 앱을 열고 Ubuntu를 검색한 후, 설치한다.

![Untitled](images/[Windows]WSL2/Untitled%203.png)

설치가 끝나고 앱이 실행되면 터미널이 하나 열리고 설치가 자동적으로 진행된다. 앱이 자동으로 실행되지 않는다면 Windows +S 키를 입력하고 Ubnutu를 검색해서 관리자 모드로 실행해준다.

![Untitled](images/[Windows]WSL2/Untitled%204.png)

조금 시간이 지나면 우분투에서 사용할 사용자 이름과 패스워드를 지정하는 입력창이 나타난다. 사용하고자 하는 사용자 이름과 패스워드를 입력한다.

![Untitled](images/[Windows]WSL2/Untitled%205.png)

다시 터미널을 실행하여 WSL을 관리하기 위한 `wsl` 명령어를 사용할 수 있다. `wsl -l -v` 로 현재 설치된 리눅스를 확인한다.

```powershell
$ wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

앞의 `*` 는 디폴트 머신을 의미한다. 여기서 버전 컬럼은 WSL의 버전을 의미한다. 버전에 2가 출력 되는지 확인한다.

