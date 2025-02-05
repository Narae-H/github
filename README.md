# 코드 버전 관리 (Git & GitHub)
- [`Git`](#git)과 [`GitHub`](#github)을 활용해 효과적으로 코드 버전 관리 가능
- 로컬에서는 Git으로 변경 이력을 관리하고, 원격 저장소(GitHub)에서는 백업 및 협업을 진행

<br/>
<br/>

# Git
- 로컬에서 쓸 수 있는 파일 버전 관리 프로그램
- 소스 코드 변경 사항을 추적하고 버전을 관리

## Git 명령어 정리

| 카테고리       | 설명        | 명령어        | 간략한 설명 |
|--------------|-------------|-------------|------------|
| **기본 기능**  | 초기화       | [`git init`](#init) | Git 저장소를 초기화 (로컬 저장소 생성) |
|              | 추가         | [`git add <파일>`](#add) | 변경된 파일을 스테이징 영역에 추가 |
|              | 상태 확인    | [`git status`](#status) | 현재 작업 상태 확인 |
|              | 커밋         | [`git commit -m "메시지"`](#commit) | 스테이징된 변경 사항을 저장 |
|              | 되돌리기     | [`git restore <파일>`](#restore) | 변경된 파일을 마지막 커밋 상태로 되돌림 |
|              | 이력 확인    | [`git log`](#log) | 커밋 이력 확인 |
|              | 차이 비교    | [`git diff`](#diff) | 변경된 내용을 비교 |
| **되돌리기 기능** | 커밋 되돌리기 | `git revert <커밋ID>` | 지정한 커밋을 되돌리고 새로운 커밋 생성 |
|              | 소프트 리셋  | `git reset --soft <커밋ID>` | 지정한 커밋으로 되돌리되, 변경 사항은 유지 |
|              | 하드 리셋    | `git reset --hard <커밋ID>` | 지정한 커밋으로 되돌리고, 변경 사항도 삭제 |
|              | 스테이징 취소 | `git restore --staged <파일>` | 스테이징된 변경 사항을 취소 |
| **브랜치 & 병합** | 브랜치 확인   | `git branch` | 현재 브랜치 목록 확인 |
|              | 브랜치 생성   | `git branch <브랜치명>` | 새 브랜치 생성 |
|              | 브랜치 이동   | `git checkout <브랜치명>` | 해당 브랜치로 이동 |
|              | 브랜치 이동   | `git switch <브랜치명>` | 새로운 방식의 브랜치 이동 (checkout 대체) |
|              | 브랜치 병합   | `git merge <브랜치명>` | 지정한 브랜치를 현재 브랜치에 병합 |
|              | 히스토리 정리 | `git rebase <브랜치명>` | 병합 대신 커밋을 다시 적용 (히스토리 정리) |
| **원격 저장소** | 저장소 복제   | `git clone <URL>` | 원격 저장소를 로컬로 복제 |
|              | 푸시         | `git push` | 로컬 변경 사항을 원격 저장소에 업로드 |
|              | 풀          | `git pull` | 원격 저장소에서 변경 사항을 가져와 병합 |

## Git 명령어 자세히

### init
```sh
git init # Git 저장소 초기화
```
- Git **저장소를 새롭게 초기화** 하는 명령어
- 이 명령어를 실행하면 **현재 디렉토리에 .git 폴더가 생성**되면서, 해당 프로젝트가 Git으로 관리되는 로컬 Git 저장소가 됨.
- 최초로 한번만 실행하면 되지만, 설정이나 변경등의 이유로 git init 명령어 다시 사용해도 무방.


### add
```sh
git add app2.txt         # 1개만 스테이징 
git add app.txt app2.txt # 2개 이상의 파일 스테이징
git add .                # 수정된 파일(삭제, 추가는 포함안됨)만 스테이징
git add -A               # 모든 변경 사항(추가, 수정, 삭제된 파일)을 스테이징
```
- 변경된 파일을 스테이징 영역에 추가하는 역할  (스테이징된 파일만 커밋가능)

<details>
<summary> 스테이징 영역이란?</summary>
- 스테이징 영역은 Git이 커밋할 파일을 추적하는 공간
- 스테이징 영역에 파일을 추가하면, 그 파일은 다음 커밋에 포함될 준비 상태
- 파일을 git add로 스테이징 하지 않으면, 해당 파일은 커밋에 포함되지 않음
</details>


### status
```sh
git status
```
- Git 저장소의 현재 상태를 확인하는 명령어. 예를 들어, 수정된 파일, 추가된 파일, 삭제된 파일 등을 보여줌.
- 상태 종류:
  - **Untracked files** (추적되지 않는 파일): 새로 생성된 파일이지만, Git이 그 파일을 아직 관리하지 않고 있다는 뜻
  - **Changes not staged for commit** (커밋 대기 중이지 않은 변경사항): 변경되었지만 아직 스테이징되지 않은 파일
  - **Changes to be committed** (커밋 대기 중인 변경사항): 스테이징 영역에 추가되어 커밋 대기 중인 파일
  - **Deleted files** (삭제된 파일)
  - **Branch 상태**: 현재 체크아웃된 브랜치가 확인


### commit
```sh
git commit -m  '[message]'  # -m: message 해당 커밋이 무엇을 했는지, 변경 사항을 간단히 설명
git commit -a               # -a: amend 수정된 모든 파일(추가된 파일은 추가안됨)을 스테이징 없이 자동으로 커밋에 포함시킬 수 있음.
git commit -am '[message]'  # 스테이징과 커밋을 동시에. but, 새롭게 추가된 파일은 스테이징 안됨.
```

- 변경 사항을 로컬 저장소에 저장하여 프로젝트의 변경 이력을 기록하는 역할
- `git add` 명령어로 스테이징 영역에 추가해야, `git commit`으로 변경사항 커밋 가능
- 커밋 메세지 작성 규칙
  - 간결하고 명확한 메시지
  - 명령형 동사 사용 ex. Add feature, fix bug, Refactor code
  - 50자 이하

// [ ] 여기서 부터 정리 시작
### restore
// Restore: 스테이징된 파일을 취소
git restore app.txt
git restore . <- 스테이징된 모든 파일을 취소

### log
// History 보기
// 다만 입력 후엔 Vim 에디터가 켜져서 q 키로 종료
git log
git log --all --oneline
git log -all -oneline --graph

### diff
// Differences 찾아줌: text based
git diff

// 시각적으로 다른 점 보여줌 (H: left, K: down, K: up, L: right)
git difftool

### revert
// 특정 커밋을 취소
git revert [커밋아이디]
git revert [커밋아이디] [커밋아이디]
git revert HEAD <- 방금 커밋한 커밋 취소

// 특정 커밋 시점으로 전부 다 되돌림: 매우 주의해야 함. 히스토리 전부 다 없애고 특정 시점으로 전부 다 돌아가기 때문에.
git reset --hard [커밋아이디]


### reset
// 파일 복구, commit복구, 과거로 이동
// 커밋된 파일 상태로 복구 또는 커밋아이디 시점으로 복구
git restore [파일이름]
git restore --soure [커밋아이디]
git restore --staged [파일이름] <- staging된 파일 취소


<br/>
<br/>

# GitHub
- Git을 기반으로 한 클라우드 기반 코드 온라인 저장소
- 코드 저장소(remote repository) 관리 및 협업 지원
- 오픈소스 프로젝트, 팀 협업, 코드 리뷰(PR, Issues) 기능 제공
<br/>
<br/>

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
// [ ] 여기 추가 필요: https://jeunna.tistory.com/109 참고


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
    - [로컬에 코드가 있고 최초로 GitHub에 코드올리는 경우](#로컬에-코드가-있고-최초로-github에-코드올리는-경우)
    - [이미 GitHub에 연결되어있으나 원격 저장소를 바꿔야하는 경우](#이미-github에-연결되어있으나-원격-저장소를-바꿔야하는-경우)


#### 로컬에 코드가 있고 최초로 GitHub에 코드올리는 경우
1. Repository 생성

2. 로컬에서 Git 초기화
    ```sh
    cd /your-project/path # 내 프로젝트 디렉터리로 이동
    git init              # git 초기화
    ```

3. VS Code에서 원격 저장소 주소를 SSH를 통해 연결
    ```sh
    # git remote set-url origin git@{Host}:{원격저장소에서':'다음부분}
    git remote add origin git@test01-github.com:user01/test-repository.git

    # 만약 잘못된 주소로 원격 저장소를 설정했고 다시 변경하고 싶다면, 아래 명령어 입력
    git remote set-url origin git@test01-github.com:user01/test-repository.git
    ```

4. User 정보 입력
    ```sh
    # 현재 유저이름과 이메일 확인
    git config --local user.name
    git config --local user.email

    # 해당 프로젝트에서 쓰고 싶은 유저로 설정
    git config --local user.email "user01@test.com"
    git config --local user.name "User 01" # 위의 이메일로 로그인 했을 때 나오는 유저 이름으로 설정
    ```

5. 원격 저장소 확인
    ```sh
    git remote -v
    ```

6. 필요하다면, `.gitignore` 파일 수정

7. 변경 사항 스테이징 및 커밋
    ```sh
    git add .                       # 모든 변경 사항 추가
    git commit -m "Initial commit"  # 최초 커밋
    ```  

8. 로컬 브랜치를 원격 저장소에 푸시
    ```sh
    git branch -M main        # 브랜치 이름을 main으로 설정
    git push -u origin main   # 원격 저장소에 main 브랜치 푸시
    ```

    > <details>
    > <summary> <strong>[Error]</strong> fatal: refusing to merge unrelated histories </summary>
    >
    > 만약 GitHub Repository에 파일이 있다면 pull 먼저하고 push 해야함. <br/>
    > 강제 병향을 하고 싶다면 아래 명령어 입력
    > ```sh
    > git pull origin main --allow-unrelated-histories
    > ```
    > </details>


#### 이미 GitHub에 연결되어있으나 원격 저장소를 바꿔야하는 경우
1. VS Code에서 원격 저장소 주소를 SSH로 변경
    ```sh 
    # 정보예시
    # 원격 저장소: git@github.com:user01/test-repository.git
    # Host: test01-github.com 

    # 원격 저장소 변경
    # git remote set-url origin git@{Host}:{원격저장소에서':'다음부분}
    git remote set-url origin git@test01-github.com:user01/test-repository.git
    ``` 

2. 원격 저장소 확인
    ```sh
    git remote -v
    ```

3. 원하는 원격 저장소에 원하는 유저로 commit 됐는지 확인
