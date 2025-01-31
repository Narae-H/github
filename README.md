# Git 이란?




# Git 계정 여러개 사용
**방법1**: [SSH Key](#ssh-key-생성-및-설정) <br/>
**방법2**: Git Credential Manager (GCM) <br/>

GCM은 SSH 설정보다 간단하지만, VS Code의 확장기능이나 git push 명령어를 사용할때 기존 로그인 정보를 삭제하고 다시 로그인해야 하는 번거로움이 있어 사용하기 불편하므로, `SSH 키를 사용하여 설정`하자.
<br/>

## SSH Key 생성 및 설정
### 사전준비 - SSH Client
1. SSH Client 설치 확인
    - PowerShell 또는 CMD(명령 프롬프트) 열기
    - 아래 명령어 입력
      ```sh
      ssh
      ```
    - SSH Client 가 설치되어 있으면 아래와 같은 사용방법이 출력됨
      ```
      usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface] [-b bind_address]
           [-c cipher_spec] [-D [bind_address:]port] [-E log_file]
           [-e escape_char] [-F configfile] [-I pkcs11] [-i identity_file]
           [-J destination] [-L address] [-l login_name] [-m mac_spec]
           [-O ctl_cmd] [-o option] [-P tag] [-p port] [-Q query_option]
           [-R address] [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
           destination [command [argument ...]]
      ```

2. 설치가 안되어 있는 경우만 설치. 설치 된 경우는 **3번**으로.
    - Windows에서 **Optional features** 검색하여 열기
    - **Add a feature** 선택
    - **OpenSSH Clkient** 선택 > **Add**

3. OpenSSH SSH Server 실행
    - Windows에서 **Services** 검색하여 실행
    - **OpenSSH Authentication Agent** 더블 클릭하여 실행
    - **Startup type**을 **Automatic**으로 변경
    - **Service status**를 **Running**인것 확인 (Running 아니라면 Start 버튼 눌러서 실행)
    - **OK**

### 사전준비 - Git Bash
Git Bash를 설치하면, Windows 환경에서 `Linux 커맨드 사용`가능
1. Git bash 설치
    - [https://git-scm.com/](https://git-scm.com/) 접속
    - 중간에 **Download** 버튼눌러서 다운로드하고 설치

### SSH 키 생성 및 설정
1. [사전준비-Git Bash](#사전준비---git-bash)에서 설치한 프로그램 실행

2. 아래 명령어 실행
    ```sh
    pwd       # 현재 작업 중인 디렉토리 경로 확인
    cd ~/.ssh # .ssh 폴더로 이동   
    ls -al    # 현재 디렉토리에 있는 모든 파일 및 폴더 확인
    ```

3. SSH Key 생성
    ```sh
    # 추가 하고 싶은 계정의 SSH Key를 생성
    # ssh-keygen -t rsa -C {GitHub로그인이메일} -f {키파일이름}
    ssh-keygen -t rsa -C "user01@test.com" -f id_rsa_github_test01
    ```
    <sup>- 패스프레이즈는 입력하지 않고 Enter를 누르면 비밀번호 없이 사용 (패스프레이즈를 설정하면 보안이 더 강화되지만 매번 입력해야 함)</sup>

4. 생성된 키 확인: SSH 키를 만들면 기본적으로 ~/.ssh 폴더에 SSH Key 하나당 두 개의 파일이 생성
    - 탐색기에서 `C:\Users\{YourName}\.ssh\` 열기
    - 키쌍이 생성된 것 확인
        ```
        개인 키: id_rsa_github_test01
        공개 키: id_rsa_github_test01.pub (pub: public의 약자)
        ```

5. 공개키 복사
    - .pub으로 생성된 파일을 메모장으로 열어서 내용 전부 복사 <br/>

6. GitHub에 SSH Key 추가
    - [GitHub](https://github.com/) 로그인: **3번**에서 입력한 이메일로 로그인 ex. user01@test.com
    - **Settings** > **SSH and GPG keys** 선택
    - **New SSH Key** 선택
    - 아래 처럼 입력 > **Add SSH Key** 선택
      ```properties
      Ttile: {키이름}                   # id_rsa_github_test01
      key Type: Authentication Key
      Key: {5번에서 복사한 내용 붙여넣기}  # ssh-rsa .....
      ```

7. 추가하고 싶은 계정 갯수만큼, `3.SSH Key 생성 ~ 6. GitHub에 SSH Key 추가` 반복

### SSH Key 관리 파일 생성 
1. SSH Config 파일 생성
    - `C:\Users\{YourName}\.ssh\`에서 `config` 파일을 열거나 생성 (주의. config 파일은 파일 확장자 없이 그냥 config)
    - config 파일을 메모장으로 열기
    - 아래 같은 패턴으로 입력하고 파일 저장
      ```properties
      ## 'SSH 키 생성 및 설정'에서 생성한 키 만큼 계속해서 추가하면 됨
      ## Host: 별칭
      ## IdentityFile: SSH Key 경로+이름
      ## HostName, User: 건들지 말기

      # 계정 01
      Host test01-github.com 
          HostName github.com 
          User git
          IdentityFile ~/.ssh/id_rsa_github_test01

      # 계정 02
      Host test02-github.com
          HostName github.com
          User git
          IdentityFile ~/.ssh/id_rsa_github_test02

      # 계정 0n
      Host test0n-github.com
          HostName github.com
          User git
          IdentityFile ~/.ssh/id_rsa_github_test0n
      ```

### 생성된 SSH 키를 SSH 서비스에 추가
1. SSH Agent 실행
    ```sh
    eval "(ssh-agent -s)"  # SSH Agent를 실행하고 필요한 환경 변수 설정
    ```

2. SSH Agent 실행 확인
    ```sh
    # 숫자(988)는 계속 바뀜. 아래와 같이 나오지 않는다면 SSH Agent 재실행: eval "(ssh-agent -s)"
    Agent pid 988  
    ```

3. SSH Key를 SSH Agent에 등록
    ```sh
    # 위에서 생성한 키 전부 추가
    ssh-add ~/.ssh/id_rsa_github_test01
    ssh-add ~/.ssh/id_rsa_github_test02

    # 잘 등록되었는지 확인
    ssh-add -l 
    ```

4. SSH Key를 SSH Agent에 등록 확인
    ```sh
    # 등록한 Key 갯수만큼 아래와 같이 뜬다면 정상
    3072 SHA 256: sdfjalkdjfkjadkfjakdjf/aksdjflksdjfk user01@test.com (RSA)
    3072 SHA 256: sdfjalkdjfkjadkfjakdjf/aksdjflksdjfk user02@test.com (RSA)
    ```

### SSH Key 연결 확인
1. SSH 프로토콜 사용하여 특정 호스트의 특정 사용자로 접속
    ```sh
    # ssh -T git@{'SSH Key 관리 파일 생성'에서 입력한 Host}
    ssh -T test01-github.com
    ``` 
    <small>**test01-github.com**: SSH를 통해 test01-github.com의 git 사용자로 접속</small> <br/>

2. 접속 확인
    ```properties
    # 성공
    # username: config 파일에서 설정한 SSH Key에 연결된 GitHub 계정의 사용자명
    Hi {username}! You've successfully authenticated, but GitHub does not provide shell access.

    # 실패
    No such host
    ```
<br/>

## SSH Key를 VS code에서 사용

### 유저 설정
#### 방법 1. 프로젝트 별로 로컬 유저 설정
1. VS Code 실행 > 터미널 오픈 (Ctrl + `) 
2. 현재 유저이름과 이메일 확인
    ```sh
    git config --local user.name
    git config --local user.email
    ```
3. 해당 프로젝트에서 쓰고 싶은 유저로 설정
    ```sh
    git config --local user.email "user01@test.com"
    git config --local user.name "User 01" # 위의 이메일로 로그인 했을 때 나오는 유저 이름으로 설정
    ```

#### 방법 2. 특정 폴더에서는 특정 유저로 로그인되도록 설정
!!!!! 추가 필요
https://jeunna.tistory.com/109

### 원격 저장소(remote) 설정
1. VS Code 실행 > 터미널 오픈 (Ctrl + `) 
2. 현재의 원격 저장소 이름과 주소 확인
  ```sh
  # 현재는 아마도 'https://'로 시작하는 주소로 설정되어 있음.
  # SSH 설정 후에는 더 이상 'https://'로 시작하는 주소를 이용하여 통신 불가능
  git remote -v
  ```

3. 원격 저장소의 SSH 주소 확인
    - [GitHub](https://github.com/) 접속
    - 원하는 Repository로 이동
    - `< >Code` 버튼 선택 
    - Local > SSH 선택
    - 주소 복사 ex. git@github.com:{유저이름}/{리포지토리이름}.git

4. 상황에 따라 아래 중 선택하여 진행
    - [기존에 GitHub 연결 안되어 있고 최초로 커밋하는 경우](#기존에-github-연결-안되어-있고-최초로-커밋하는-경우)
    - [사용하고 있는 프로젝트의 원격 저장소를 바꾸는 경우](#사용하고-있는-프로젝트의-원격-저장소를-바꾸는-경우)


#### 기존에 GitHub 연결 안되어 있고 최초로 커밋하는 경우



#### 사용하고 있는 프로젝트의 원격 저장소를 바꾸는 경우
1. VS Code에서 원격 저장소 주소를 SSH로 변경
    ```sh 
    # 정보예시
    # 원격 저장소: git@github.com:user01/test-repository.git
    # Host: test01-github.com 

    # 원격 저장소 변경
    # git remote set-url origin {Host}:{원격저장소에서':'다음부분}
    git remote set-url origin test01-github.com:user01/test-repository.git
    ``` 

2. 원격 저장소 확인
    ```sh
    git remote -v
    ```

3. 원하는 원격 저장소에 원하는 유저로 commit 됐는지 확인




--------------------
!!!! 아래 부분부터 정리해서 `#### 기존에 GitHub 연결 안되어 있고 최초로 커밋하는 경우`로 올리면 됨.
<사용하는법>
git remote -v # 현재의 원격 저장소 이름과 주소 확인
git remote set-url origin my-github.com:Narae-H/study-nodejs.git  # 원격 저장소 주소 업데이트
git remote set-url origin my-github.com: # 원격 저장소 주소 업데이트


<로컬 코드를 GitHub 리포지토리에 최초로 푸시>
-  SSH 설정이 제대로 되어 있다면 새로운 리포지토리를 GitHub에서 생성하고, 로컬 프로젝트에서 이를 연결하여 푸시하면 됩니다.

1. 로컬 Git 초기화
vs code로 해당 프로젝트 열고 아래 명령어 입력
git init

2. GitHub 리포지토리 연결 (최초 푸시)
GitHub에서 새로운 리포지토리를 생성하고, 생성된 리포지토리의 SSH URL을 사용하여 로컬 프로젝트와 연결합니다.
GitHub에서 리포지토리 생성: GitHub에서 새 리포지토리를 생성합니다 (예: your-repository-name).
리모트 리포지토리 연결: GitHub 리포지토리를 로컬 Git 프로젝트와 연결합니다.
git remote add origin git@account1:your-username/your-repository-name.git
git remote add origin git@hh:H-H-Lawyers/hhlaw.com.au.git

git remote set-url origin git@hh:H-H-Lawyers/hhlaw.com.au.git

 git branch -m master main

로컬 변경 사항을 Git에 추가:
git add .
git commit -m "Initial commit"

최초 푸시:
git remote set-url origin git@github.com-userA:github계정/repo이름.git 
git remote add origin git@github.com:H-H-Lawyers/hhlaw.com.au.git
(브랜치 변경: git remote set-url origin git@github.com:H-H-Lawyers/hhlaw.com.au.git)
git remote set-url origin git@hh:H-H-Lawyers/hhlaw.com.au.git
git push origin main

