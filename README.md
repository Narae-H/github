# 코드 버전 관리 (Git & GitHub)
- [`Git`](#git)과 [`GitHub`](#github)을 활용해 효과적으로 코드 버전 관리 가능
- `로컬에서는 Git`으로 변경 이력을 관리하고, `원격 저장소인 GitHub`에 저장하여 팀 협업
<br/>
<br/>

# Git VS GitHub
## Git
- **로컬**에서 코드의 변경 사항을 추적하고 관리
- 소스 코드 변경 사항을 추적하고 버전을 관리

## GitHub
- Git을 기반으로 한 **클라우드**기반 코드 온라인 저장소 및 협업 플랫폼
- 원격 저장소를 제공하고 팀 협업을 돕는 기능을 포함
<br/>
<br/>

## `Git` VS `GitHub`
| 구분     | <center>`Git`</center> | <center>`GitHub`</center> |
|----------|--------------------------------------|------------------------------------------------|
| 개념     | 소스 코드 변경 사항을 추적하고 버전을 관리 | Git을 기반으로 한 클라우드 코드 저장소 및 협업 플랫폼 |
| 저장 위치 | 로컬(내 컴퓨터) | 원격(인터넷 서버) |
| 주요 기능 | 버전 관리, 브랜치 생성 및 병합 | 코드 저장, 협업, 이슈 트래킹, CI/CD |
| 네트워크 | 필요 없음 (로컬 작업 가능) | 필요함 (원격 저장소 접근) |

<br/>
<br/>

# Repository (저장소)
- `Git Repository`: 프로젝트의 소스 코드, 변경 이력, 브랜치 등을 저장하는 공간
- `.git`: Git 저장소의 정보가 담겨 있는 숨김 폴더
- `.git` 폴더가 있는 디렉토리는 `Git Repository`로 인식
- `.git` 폴더가 삭제되면 `Git Repository`의 모든 정보 사라짐.
<br/>
<br/>

# Git/GitHub 명령어
## 1. 최초 설정
Git 저장소를 처음 만들거나, 원격 저장소를 복제하는 단계

| 단계          | 명령어 | 설명 |
|--------------|----------------------|-------------------------------|
| **Git 초기화** | [`git init`](#init) | 새로운 Git 저장소를 생성 |
| **저장소 복제** | [`git clone <URL>`](#clone) | 원격 저장소를 로컬로 복제 |

### init
```sh
# Git 저장소 초기화: 현재 디렉토리에 .git 폴더 생성
git init 
```
- Git **저장소를 새롭게 초기화** 하는 명령어
- 이 명령어를 실행하면 **현재 디렉토리에 .git 폴더가 생성**되고, 해당 프로젝트가 Git으로 관리되는 로컬 Git 저장소가 됨.
- 최초로 한번만 실행하면 되지만, 설정이나 변경등의 이유로 git init 명령어 다시 사용해도 무방.

### clone
```sh
# 저장소 주소에 있는 소스코드를 내 로컬(PC)에 다운로드
git clone <원격저장소주소> 
```
- 해당 명령어는 소스코드를 다운받을 위치에서 실행. ex. cd \user\my-workplace
- GitHub의 저장소(repository)에서 원격 저장소 주소를 확인할 수 있으며, 이를 이용해 git clone을 수행할 수 있음.
<br/>

## 2. 코드 변경 사항 준비 (스테이징)  
코드 변경사항을 스테이징 영역에 추가

<details>
<summary> <sup>스테이징 영역</sup> </summary>

- 스테이징 영역은 Git이 커밋할 파일을 추적하는 공간 
- 스테이징 영역에 파일을 추가하면, 그 파일은 다음 커밋에 포함될 **준비** 상태
- 파일을 `git add`로 스테이징 하지 않으면, 해당 파일은 커밋에 포함되지 않음
- 즉, 내가 히스토리를 관리하고 싶은 파일만 선택하는 과정.
</details>


| 단계          | 명령어 | 설명 |
|--------------|----------------------------------|----------------------------------|
| **파일 추가** | [`git add <파일>`](#add) | 변경된 파일을 스테이징 영역에 추가 |
| **파일 삭제** | [`git rm <파일>`](#rm) | 파일을 Git에서 제거 |
| **변경 취소** | [`git restore <파일>`](#restore) | 스테이징 취소 |
| **상태 확인** | [`git status`](#status) | 현재 Git 상태(추가, 변경, 삭제)를 확인 |

### add
```sh
# 특정파일(app2.txt) 스테이징에 추가 
git add app2.txt         

# 2개 이상의 파일(app.txt, app2.txt) 스테이징에 추가
git add app.txt app2.txt 

# 수정된 파일(삭제, 추가는 포함안됨) 전부를 스테이징에 추가
git add .                

# 모든 변경(추가, 수정, 삭제된 파일)의 파일 전부를 스테이징에 추가
git add -A               
```
- 히스토리 관리를 하고싶은 특정 파일만 고르는 과정 (장바구니에 아이템 담기)
- add 된 파일은 스테이징 영역에 올라가고, 여기있는 파일만 커밋 가능.

### rm
```sh
# 파일이 작업 디렉토리(로컬 폴더)에서도 삭제
git rm <파일명>
```
- Git에서 파일을 제거하고 로컬에서도 삭제할 때 사용하는 명령어 (즉, 파일 자체가 사라짐)
- `파일 삭제 + Git 추적해제 /+ 추적해제` 까지 한번에 처리
- 로컬에서 삭제 -> git add <파일이름> -> git commit도 가능
- 즉, 작은 프로젝트라면 편한 방법을 쓰면 되고, 삭제 작업이 많거나 협업 중이라면 git rm을 쓰는 게 더 직관적

### restore
```sh
# 스테이징된 특정 파일(app.txt)을 스테이징 영역에서 삭제
git restore app.txt  

# 스테이징된 모든 파일을 스테이징 영역에서 삭제
git restore .       
```
- Restore: 스테이징된 파일을 스테이징 영역에서 삭제

### status
```sh
git status
```
- Git 저장소의 현재 상태를 확인하는 명령어. 예를 들어, 수정된 파일, 추가된 파일, 삭제된 파일 등을 보여줌.

  <details>
  <summary> 상태 종류 </summary>

  - `Untracked files`: 새로 생성된 파일이지만, Git이 그 파일을 아직 관리하지 않고 있다는 뜻
  - `Changes not staged for commit`: 변경되었지만 아직 스테이징되지 않은 파일
  - `Changes to be committed`: 스테이징 영역에 추가되어 커밋 대기 중인 파일
  - `Deleted files`: 삭제된 파일
  - `Branch`: 현재 체크아웃 된 브랜치 확인
  </details>
<br/>

## 3. Git(로컬 저장소)에 저장  
스테이징 된 파일을 Git (로컬 저장소)에 저장하는 단계.  

| 단계          | 명령어 | 설명 |
|--------------|----------------------------------|------------------------------|
| **커밋**      | [`git commit -m "메시지"`](#commit) | 스테이징 된 변경 사항을 로컬 저장소에 저장 |
| **이력 확인** | [`git log`](#log) | 커밋 히스토리를 확인 |
| **변경 비교** | [`git diff`](#diff) | 변경된 내용을 비교 |

### commit
```sh
git commit -m  '[message]'  # -m: message
git commit -a               # -a: amend, 수정된 모든 파일(추가된 파일은 추가안됨)을 스테이징 없이 자동으로 커밋에 포함.
git commit -am '[message]'  # 스테이징과 커밋을 동시에. but, 새롭게 추가된 파일은 스테이징 안됨.
```

- 변경 사항을 **로컬** 저장소에 저장하여 프로젝트의 변경 이력을 기록하는 역할
- 커밋 메세지 작성 규칙
  - 간결하고 명확한 메시지
  - 명령형 동사 사용 ex. Add feature, fix bug, Refactor code
  - 50자 이하

### log
```sh
# 커밋 해시(hash), 작성자, 날짜, 커밋 메시지가 최근것부터 표시
git log

# 각 커밋을 한 줄로 간략하게 표시
git log --all --oneline

# 그래프 형태로 보기. branch와 merge 히스토리를 시각적으로 확인
git log -all -oneline --graph  

# 커밋별 변경된 코드(diff) 확인
git log -p 

# 최근 3개(n개) 커밋만 표시
git log -n 3 

# 특정 파일(index.js)과 관련된 커밋만 표시
git log -- index.js 

# 특정 사용자가 작성한 커밋만 필터링
git log --author="ABC" 

# 특정 날짜 이후 커밋만 조회
git log --since="2025-02-01" 
```
- Git 저장소의 커밋 내역(History)를 조회
- 다만 입력 후엔 Vim 에디터가 켜져서 q 키로 종료

### diff
```sh
# 작업 디렉토리(수정된 파일)와 스테이징 영역(추가된 파일)의 차이점
git diff

# 스테이징 영역과 마지막 커밋의 차이점
git diff --staged

# 특정 두 커밋 간의 변경 사항을 비교
git diff 커밋해시1 커밋해시2

# 특정 파일의 변경 사항 확인
git diff index.js

# 브랜치 간 변경 사항 비교
git diff main feature-branch

# 변경된 부분을 빨간색(삭제)과 초록색(추가)으로 표시
git diff --color

# 변경된 파일 수와 수정된 줄 수만 간략히 표시
git diff --stat
```
- 파일의 변경 사항을 비교하는 명령어
<br/>

## 4. Git(로컬 저장소) 되돌리기  
Git의 로컬 저장소에서 변경 사항을 되돌리는 과정. 즉, GitHub(원격 저장소)에 반영되기 전에 로컬 작업을 수정. 

| 단계           | 명령어 | 설명 |
|--------------|--------------------------------------|----------------------------------|
| **커밋 되돌리기** | [`git revert <커밋해시>`](#revert) | 지정한 커밋을 되돌리는 새로운 커밋을 생성 (기존 커밋 삭제되지 않음) |
| **소프트 리셋** | [`git reset --soft <커밋해시>`](#reset---soft) | 지정한 커밋으로 되돌리되, 변경 사항은 유지 |
| **하드 리셋** | [`git reset --hard <커밋해시>`](#reset---hard) | 지정한 커밋으로 되돌리고, 변경 사항도 삭제 |

### revert
```sh
# 특정 커밋을 되돌리는 새로운 커밋을 생성
git revert <커밋해시>

# 최근 1개의 커밋을 되돌림
git revert HEAD

# 최근 3개의 커밋을 한 번에 되돌림: HEAD~2부터 HEAD까지 총 3개의 커밋을 되돌림
git revert HEAD~2..HEAD

# 여러 개의 커밋을 한 번에 취소하는 여러 개의 Revert 커밋 생성
git revert <커밋해시> <커밋해시>
```
- 기존 커밋을 되돌리는 **새로운 커밋을 생성**하는 명령어 <br/>
- 기존 커밋 히스토리는 유지 <br/>
- 원래 커밋 자체는 삭제되지 않음 <br/>

### reset --soft
```sh
# 최근 커밋을 되돌리기 (하지만 변경 사항은 유지)
git reset --soft HEAD~1

# 특정 커밋으로 이동 (커밋 ID 확인: git log)
git reset --soft <커밋해시>
```
- 특정 커밋 시점으로 이동하기 때문에 그 이후의 커밋이 삭제됨. <br/>
- 하지만 변경 사항들은 스테이징된 상태로 유지됨. <br/>
- 필요하면 `git commit -m "새 메시지"`로 다시 커밋 가능. <br/>
- 커밋 히스토리를 변경하므로 협업 시 주의 필요 <br/>

### reset --hard
```sh
# 최근 커밋을 완전히 삭제 (변경 사항도 함께 삭제)
git reset --hard HEAD~1

# 특정 커밋으로 되돌리기 (모든 변경 사항 삭제)
git reset --hard <커밋해시>
```
-  특정 커밋 시점으로 이동하기 때문에 그 이후의 커밋이 삭제됨. <br/>
-  파일 변경 사항까지 모두 삭제됨. <br/>
- 삭제된 커밋과 변경사항은 복구가 어려움. <br/>
- 협업 시 위험 → 원격 저장소에 푸시한 후 강제 푸시 필요 (`git push --force`) <br/>
<br/>

### 🎯 비교: `revert` VS `reset --soft` VS `reset --hard`

|  | `git revert` | `git reset --soft` | `git reset --hard` |
|-----------------|--------------------------|-------------------------|-------------------------|
| **작동 방식** | 새로운 "되돌림 커밋" 생성 | 특정 커밋 이후의 히스토리 삭제, 변경 사항 유지 | 특정 커밋 이후의 히스토리 및 변경 사항 삭제 |
| **히스토리 유지** | ✅ 유지됨 (안전) | ❌ 변경됨 (주의 필요) | ❌ 변경됨 (복구 어려움) |
| **로컬 영향** | 이전 커밋 내용이 되돌려짐 | 변경 사항이 **스테이징됨** | 변경 사항이 **모두 삭제됨** |
| **원격 저장소 푸시** | ✅ 가능 | ❌ 주의 필요 (히스토리 변경) | ❌ `git push --force` 필요 |
| **협업 시 적합?** | ✅ 적합 | ❌ 신중히 사용해야 함 | ❌ 위험 (주의 필요) |
<br/>

## 5. Git 브랜치 및 병합  
Git에서 브랜치를 생성하고 이동하거나, 병합하는 과정.  

| 단계           | 명령어 | 설명 |
|--------------|---------------------------------|----------------------------------|
| **브랜치 목록** | [`git branch`](#git-branch) | 브랜치 목록 확인 |
| **브랜치 생성** | [`git branch <브랜치명>`](#git-branch-브랜치명) | 새로운 브랜치를 생성 |
| **브랜치 올리기** | [`git branch --set-upstream origin <브랜치명>`](#git-branch---set-upstream-origin-브랜치명) | 생성된 브랜치를 origin(원격 저장소)에 올리기 |
| **브랜치 이동** | [`git switch <브랜치명>`](#git-switch-브랜치명) | 다른 브랜치로 이동 |
| **브랜치 생성+이동** | [`git checkout -b <브랜치명>`](#git-checkout--b-브랜치명) | *git branch <브랜치명>* + *git switch < 브랜치명>*  두 개 한방에 |
| **브랜치 삭제** | [`git branch -d <브랜치명>`](#branch--d-브랜치명) | 브랜치를 삭제 |
| **브랜치 병합** | [`git merge/rebase --squash <브랜치명>`](#mergerebasesquash) | 두 개의 브랜치를 병합 |

### git branch
```sh
# 로컬 브랜치 목록 확인
git branch

# 로컬 브랜치 + 원격 브랜치 목록 확인
git branch -a
```
- 브랜치 목록을 확인

### git branch <브랜치명>
1. 방법 1: powershell 이용
```sh
# feature-branch 라는 새로운 브랜치 생성. 새로운 브랜치로 자동 이동하지는 않음
git branch <브랜치명>
```

2. 방법 2: GitHub 에서 만듬
  - [GitHub](https://github.com/) 접속 > 새로운 브랜치 생성할 repository로 이동
  - <>code 버튼 > main ▼ > branch 이름 적기 > Create branch [브랜치이름] from main 선택

### git branch --set-upstream origin <브랜치명>
- 생성된 브랜치를 origin(원격 저장소)에 올림
```sh
# dev라는 브랜치를 생성하고 origin(원격 저장소)에 올릴 때 사용
git branch --set-upstram origin dev
```

### git switch <브랜치명>
```sh
# feature-branch(특정 브랜치)로 이동 (구 버전은 git checkout <브랜치명>)
git switch <브랜치명>

# 새로운 브랜치를 만들면서 바로 이동.
# -c: create
git switch -c <브랜치명>
```
- 특정 브랜치로 이동 

### git checkout -b <브랜치명>
- 브랜칭 생성과 이동을 한 방에: git branch <브랜치명> + git switch <브랜치명>
```sh
git checkout -b <브랜치명>
```

### branch -d <브랜치명>
```sh
# 로컬 브랜치를 삭제할 때 사용
git branch -d <브랜치명>
```
- 삭제하려는 브랜치가 다른 브랜치(예: main)에 병합되어 있어야 안전하게 삭제할 가능
- 병합되지 않은 브랜치를 삭제하려면 -D 옵션을 사용해 강제로 삭제할 수 있지만, 이 경우 중요한 변경 사항이 사라질 수 있음.
<br/>

### merge/rebase/squash
병합하는 방법은 [로컬](#방법-2-로컬에서-병합)에서도 가능하고 아님 브랜치(예. feature/login)에 push한 후 [GitHub](#방법-1-github에서-병합)에서도 가능.
  - **보통은 GitHub에서 병합**. 왜냐면, 팀에서 코드 리뷰를 하고 서로 상의 후에 한 후에 main으로 merge 하고 싶기 때문.

#### <h4>방법 1. GitHub에서 병합</h4>
1. 코드 수정 후에 특정 브랜치(예. feature/login, develop)에 push
2. [GitHub](https://github.com)에 접속
3. 원하는 Repository로 이동
4. Pull requests 탭 > New pull request 선택 > 상단에서 base 브랜치와 compare 브랜치 선택 > Create pull request 선택
5. 서로 코멘트 달면서 상의/협의
6. 협의가 끝 > Merge pull request 선택 
7. 만약, conflict가 있다면 해결 > 'Resolve conflicts' 선택 > 코드 창에서 코드 수정하고 'Mark as resolved' 선택 > 'Commit merge' 
8. Merge pull request > Confirm merge

#### 방법 2. 로컬에서 병합
##### <h4>Commit Strategy (커밋 방식)</h4>
1. `merge commit`
	```sh
	git merge <브랜치명>
	```
	- Three way merge 처럼 `두 길이 만나는 지점에 표지판(커밋)`을 세워서 두 길이 합쳐짐을 기록
	- 브랜치가 diverged(분기)한 경우, Git은 자동으로 새로운 병합 커밋(merge commit)을 생성하여 두 브랜치의 히스토리를 통합
	- 이 방식은 각 브랜치의 작업 이력을 그대로 남기고, 언제 어떤 브랜치가 합쳐졌는지 명확하게 기록

		<details>
		<summary> 📝 예시</summary>

		- 명령어 및 예시
		```sh
		# 1) main 브랜치에서 feature 브랜치 생성 후 작업
		git switch -c feature
		echo "Feature change" >> file.txt
		git add file.txt
		git commit -m "Add feature change"

		# 2) main 브랜치에도 별도 작업
		git switch main
		echo "Main update" >> file.txt
		git add file.txt
		git commit -m "Update main branch"

		# 3) feature 브랜치 병합 (Three-Way Merge 발생하여 merge commit 생성)
		git merge feature/login
		```

		- 히스토리 모습
		```sh
		git log --oneline --graph
		```
		```sql
		*   9a8b7c6 (HEAD -> main) Merge branch 'feature'
		|\  
		| * 3f4d2c0 (feature) Add feature change
		* | 1b2c3d4 Update main branch
		|/  
		* 1a2b3c4 Initial commit
		```
		</details>
<br/>

2. `rebase merge` 
	```sh
	git rebase <브랜치명>
	```
	- 길을 합치기 전에 `한쪽 길의 경로를 재정비`해서 마치 처음부터 하나의 연속된 길처럼 보이게 함.
	- Rebase Merge는 대상 브랜치(예: main)의 최신 커밋 위에 feature 브랜치의 커밋들을 재배치(rebase)하는 방식
	- feature 브랜치의 각 커밋을 그대로 순서대로 main 위에 재적용하여 선형(linear) 히스토리를 만들 수 있음.
	- 히스토리가 깔끔해지지만, 커밋들의 원래 분기 정보는 사라짐.
	- `개인 프로젝트`나 `feature 브랜치 작업 후 최신 main의 변경사항을 반영`하고자 할 때 사용.

		<details>
		<summary> 📝 예시</summary>

		- 명령어 및 예시
		```sh
		# 1) feature 브랜치에서 작업 진행 (main에서 분기)
		git switch -c feature/login
		echo "Login change" >> login.txt
		git commit -am "Add login change"

		# 2) main 브랜치에서 별도 작업이 발생 (예를 들어 업데이트 커밋)
		git switch main
		echo "Main update" >> file.txt
		git commit -am "Update main branch"

		# 3) feature/login 브랜치로 돌아가서 main 위로 rebase 실행
		# main 브랜치의 히스토리는 그대로 유지되고, feature/login 브랜치의 커밋들이 main 브랜치의 최신 커밋 위로 다시 쓰임.
		git switch feature/login
		git rebase main

		# 4) main 브랜치로 돌아와 fast-forward 병합 (merge commit 없이)
		git switch main
		git merge feature/login
		```

		- 히스토리 모습
		```sh
		git log --oneline --graph
		```
		```sql
		* 3f4d2c0 (HEAD -> main, feature/login) Add login change
		* 1b2c3d4 Update main branch
		* 1a2b3c4 Initial commit
		```
		</details>
<br/>

3. `squash merge`
	```sh
	git merge --squash <브랜치명>
	```
	- `브랜치의 모든 커밋을 하나로 합쳐서 병합`하는 방식(히스토리가 간결해짐).
	- feature 브랜치에서 이루어진 여러 커밋들을 **하나의 커밋으로 압축(squash)**하여 병합하는 방식
	- 결과적으로 feature 브랜치의 세부 커밋 이력은 사라지고, 한 개의 단일 커밋에 모든 변경사항이 담김

		<details>
		<summary> 📝 예시</summary>

		- 명령어 및 예시
		```sh
		# 1) feature/login 브랜치에서 여러 커밋 작업
		git switch -c feature/login
		echo "Part 1 of login" >> file.txt
		git commit -am "Login part 1"
		echo "Part 2 of login" >> file.txt
		git commit -am "Login part 2"

		# 2) main 브랜치로 돌아와서 squash merge 실행
		git switch main
		git merge --squash feature/login # feature 브랜치의 모든 변경사항을 하나로 압축하여 스테이징 영역에 반영

		# 3) 단일 커밋으로 기록
		git commit -m "Squash merge: Add feature/login changes"
		```

		- 히스토리 모습
		```sh
		git log --oneline --graph
		```
		```sql
		* abcdef1 (HEAD -> main) Squash merge: Add feature/login changes
		* 1a2b3c4 Initial commit
		```
		</details>
<br/>

##### <h4>Merge strategy (병합 방식)</h4>
1. `fast-foward merge`
	- 두 길이 사실 상 `하나의 길`처럼 이어짐
	- 브랜치가 분기된 이후 main에 추가 commit이 없을 때, 자동으로 fast-forward 병합이 발생
	- 깔끔한 선형 히스토리를 원할 때 사용하며, 불필요한 병합 커밋이 생기지 않아 히스토리가 간단
	- `개인 프로젝트`에서 주로 사용
		<details>
		<summary> 📝 예시 </summary>

		-	명령어 예시
		```sh
		# 1) main 브랜치에서 분기하기 전에 초기 커밋이 있다고 가정
		git init
		echo "Initial content" > file.txt
		git add file.txt
		git commit -m "Initial commit"

		# 2) feature/login 브랜치 생성 후 작업
		git switch -c feature/login
		echo "Login change" >> login.txt
		git commit -am "Add login change"

		# 3) main 브랜치로 돌아와서 feature 병합 (Fast-Forward Merge)
		git switch main
		git merge feature
		```
		<br/>

		- 히스토리 모습: 선형(직선)으로 커밋들이 나열됨
		```sh
		git log --oneline --graph
		```
		```sql
		* 3f4d2c0 (HEAD -> main, feature/login) Add login change
		* 1a2b3c4 Initial commit
		```
		</details>
<br/>

2. `3-way merge` 
	- 두 길이 서로 다른 방향에서 오다가 `어느 시점에 합쳐짐`
	- 두 브랜치 모두에 독립적으로 커밋이 추가되어 서로 내용이 달라졌을 때 발생
	- conflict 발생할 수도 있음. 두 브랜치의 변경 사항을 통합하기 위해 공통 조상을 기준으로 병합 커밋을 생성.
	- `협업` 중에 주로 사용.

		<details>
		<summary> 📝 예시 </summary>
		
		- 명령어 예시
		```sh
		# 1) 초기 상태 설정 (main 브랜치에 초기 커밋)
		git init
		echo "Initial content" > file.txt
		git commit -am "Initial commit"

		# 2) feature/login 브랜치 생성 및 작업
		git switch -c feature/login
		echo "Login change" >> file.txt
		git commit -am "Add feature/login change"

		# 3) main 브랜치로 돌아와서, main에서도 작업 추가
		git switch main
		echo "Main update" >> file.txt
		git commit -am "Update main branch"

		# 4) 이제 두 브랜치가 분기된 상태에서 병합
		git merge feature
		```
		<br/>

		- 히스토리 모습: 분기가 있었다가 `새로운 병합 커밋이 생성`된 것을 볼 수 있음
		```sh
		git log --oneline --graph
		```
		```sql
		*   9a8b7c6 (HEAD -> main) Merge branch 'feature/login'
		|\  
		| * 3f4d2c0 (feature/login) Add feature/login change
		* | 1b2c3d4 Update main branch
		|/  
		* 1a2b3c4 Initial commit

		```
		</details>
<br/>
<br/>

## 6. GitHub (원격 저장소) 작업  
Git(로컬 저장소) 내용을 GitHub (원격 저장소)에 반영  

| 단계          | 명령어 | 설명 |
|--------------|------------------------|----------------------------------|
| **현재 원격 저장소 확인** | [`git remote -v`](#git-remote--v) | 현재 연결된 원격 저장소 확인 |
| **원격 저장소 연결** | [`git remote add origin <URL>`](#git-remote-add-origin-원격저장소url) | 원격 저장소와 연결 |
| **업스트림 설정** | [`git push -u origin <브랜치이름>`](#git-push--u) | 업스트림 설정  |
| **푸시 (업로드)** | [`git push`](#push) | 로컬 변경 사항을 원격 저장소에 업로드 |
| **풀 (가져오기)** | [`git pull`](#pull) | 원격 저장소에서 변경 사항을 가져와 병합 |

### git remote -v
```
git remote -v
```

### git remote add origin <원격저장소URL>
```sh
# GitHub에 연결
git remote add origin https://github.com/username/my-project.git
```
- 로컬 Git 저장소를 원격 저장소(예: GitHub, GitLab, Bitbucket 등)와 연결
- origin은 기본 원격 저장소 이름으로 관례적으로 사용. origin에 원격 저장소 주소 저장

### git push -u
```sh
# 1) 최초 clone 할 때: 자동적으로 원격 저장소(origin)의 같은 이름의 브랜치를 추적하도록 설정
# git clone <원격주소>
git clone https://github.com/username/my-project.git

# 2) 최초 push 할 때: 새 로컬 브랜치를 처음 원격 푸시
# git push -u <원격저장소> <원격브랜치명>
git push -u origin main

# 3) 수동: 이미 존재하는 브랜치에 대해 업스트림을 수동으로 설정
# git branch --set-upstream-to=<원격저장소>/<원격브랜치명> <로컬브랜치명>
git branch --set-upstream-to=origin/main main
```
- 업스트림: 로컬 브랜치가 추적하는 원격 브랜치
- 업스트림을 설정하면 해당 로컬 브랜치가 원격 브랜치와 연결되어 앞으로 git push와 git pull을 쉽게 사용할 수 있음

### push
```sh
# 명시적: origin 이라는 원격 저장소의 main 브랜치로 업로드
# git push <원격저장소> <원격브랜치명>
git push origin main

# 암묵적: 원격 브랜치(업스트림)와 연결되어 있는 경우
git push
```
- 로컬 저장소에서 커밋된 변경 사항을 원격 저장소에 업로드(전송)

### pull
```sh
# 원격 저장소(origin)의 main 브랜치의 최신 변경 사항을 로컬의 main 브랜치로 가져와 병합
# git pull <원격저장소> <원격브랜치명>
git pull origin main

# 암묵적: 원격 브랜치(업스트림)와 연결되어 있는 경우
git pull
```
- 원격 저장소에서 최신 변경 사항을 가져와서 현재 로컬 브랜치에 병합
- `git pull`은 내부적으로 `git fetch`와 `git merge`를 순차적으로 실행하여 원격의 변경 사항을 로컬에 반영

<br/>
<br/>

# GitHub 계정 여러개 사용
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
    pwd        # 현재 작업 중인 디렉토리 경로 확인
    cd ~/.ssh  # .ssh 폴더로 이동
               # 만약, .ssh 폴더 없다면, mkdir .ssh 명령어로 폴더 생성. 위치는 C:\Users\<Username> 에.
    ls -al     # 현재 디렉토리에 있는 모든 파일 및 폴더 확인
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
#### 방법 1. 프로젝트 별로 `로컬` 유저 설정
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

#### 방법 2. 특정 폴더에서는 특정 유저로 로그인되도록 `전역파일(.gitconfig)` 설정
만약, 이 폴더 밑에 있는 저장소면 이 정보를 사용하겠다 라고 설정
1. 홈 디렉토리 확인
```sh
# 홈 디렉토리 확인
echo $Host
```

2. 홈 디렉토리로 이동해서 `.gitconfig` 파일 열기 또는 파일이 없다면 해당 위치에 파일 생성
    - 내용: 
      ```ini
      [includeIf "gitdir:~/projects/personal/"]
          path = ~/.gitconfig-personal
      ```

3. 위에서 path= 로 지정한 `.gitconfig-personal` 생성
    - 내용:
      ```ini
      [user]
      name = Your Name (Personal)
      email = your_personal@example.com
      ```

4. 테스트 및 확인
  - `~/projects/personal/` 아래에 새로운 저장소를 생성하거나 기존 저장소로 이동
  - 저장소 내에서 다음 명령어를 실행하여 설정이 제대로 반영되었는지 확인
    ```sh
    git config user.name
    git config user.email
    ```


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
    - [github.com에서 Repository를 생성하고 clone하는 경우](#githubcom에서-repository를-생성하고-clone하는-경우)

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

#### github.com에서 Repository를 생성하고 clone하는 경우
1. [github.com](github.com) 접속
2. Repository 생성
3. 로컬 탐색기에서 해당 Repository를 clone하고 싶은 위치로 이동
4. Shift + 우클릭 > Git Bash Here 선택
5. 아래 명령어 이용하여서 Repository Clone
  ```sh
  # @다음에 github.com이 아니라 my-github.com으로 수정하기
  git clone git@my-github.com:user01/test-repository.git
  ```
<br/>
<br/>

## 팀원1 에게 권한 부여
1. [GitHub](https://github.com/)로 이동
2. 권한 부여를 원하는 repository 로 이동
3. Settings 탭 > Collaborators and teams
4. Manage access 섹션 > Add people > 팀원 1 추가
<br/>
<br/>

# GitHub: 한 개의 repository에 여러 프로젝트 올리기
<br/>
<br/>

# Git Branch Strategy
프로젝트가 커지고, 사람이 많아지면 branch, merge 더러워짐. <br>
그래서 여러가지 방법론을 사용: 
  - [GitFlow](#1-gitflow-by-vincent-driessen)
  - [Trunk-based](#2-trunk-based-branch)
  - Github Flow
  - Gitlab Flow <br/>
> [!NOTE] *참고링크: [제주코딩베이스캠프](https://paullabworkspace.notion.site/GitHub-38a3fdea2b5f4980965b47e851e129a1#d62e25aa70a347fcadfd171069162b5d)*

## 1. Git Flow by Vincent Driessen
아래의 5가지 브랜치를 만들어서 관리. <br/>
근데, CI/CD에는 적합하지 않음. 그럴 경우 상황에 맞춰서 변형해서 쓰면 됨. ex. relase branch는 안만듬. 바로 develop branch 에서 main으로 merge.

### 브랜치 종류
1. main
    - 프로덕션(배포) 브랜치
2. develop
    - 신규 기능을 추가할 때, 여러 기능(feature 브랜치)들이 여기로 추가됨. 다음 릴리스를 위한 통합 브랜치
3. feature
    - 기능 별로 branch를 나누고 기능 개발이 끝난다면 develop에다가 merge. 
    - 여기서의 작명은 feature/[기능이름] ex) feature/guild, feature/friend
4. release (CI/CD 에서는 적합하지 않음)
    - develop branch가 다 기능 개발이 끝났다고 해서 바로 main에 합치기에 불안함. 
    - 그래서 그 전에 release라는 브랜치 만들어서 여기다가 커밋해서 여러가지 테스트 진행. 
    - 테스트가 완료되면 main branch에만 업데이트하는게 아닌 develop에도 업데이트 해줌. 왜냐면 신규 기능 계속 개발중일꺼니깐. 
5. hotfix
    - 급하게 수정되어야 할 버그를 위한 branch. 다 끝나고 나면 main, develop 에 업데이트

### 브랜치 흐름
<img src="https://github.com/Narae-H/github/blob/main/assets/gitflow.png?raw=true" width="500px" alt="Git Flow">

**장기 브랜치**
- main: 프로덕션(배포) 브랜치
- develop: 다음 릴리스를 위한 통합 브랜치

**기능 작업은 feature 브랜치로 진행**
- 새로운 기능이나 수정 작업을 시작할 때는 develop 브랜치에서 분기하여 feature/기능명 형태의 브랜치를 만듬.
- 작업이 완료되면 해당 feature 브랜치를 develop에 병합.
- develop 브랜치에 병합 완료한 feature 브랜치는 삭제.

**릴리스 및 배포**
- 일정 시점에서 develop의 안정된 상태를 main에 병합하여 프로덕션 서버에 배포.
- 병합 후에는 develop 브랜치로 다시 돌아와서 새로운 feature 브랜치를 만들어 개발을 계속 진행.
<br/>

`결론`: 보통은 develop 브랜치에서 새로운 기능을 위한 feature 브랜치를 만들어 작업하고, 완료 후 develop에 머지하는 방식. main 브랜치에서 직접 feature 브랜치를 생성하지 않음. 이렇게 하면 각 기능이 독립적으로 관리되고, develop 브랜치를 통해 통합 및 테스트한 후 main에 반영할 수 있음.


## 2. Trunk-based Branch
브랜치 하나만 잘 관리하자. main branch 만 잘 관리하고 새로운 기능이 필요할 때, feature/[기능이름] branch 만들어서 기능 개발하고 바로 다시 main으로 merge. <br/>
main 브랜치를 바로 user에게 배포하므로, 버그/오류가 많을 수 있음. <br/> 
그러므로 테스트 많이 자주 해야함 -> 테스트 자동화/배포 자동화 필요
<br/>

# Git Projects
## Git Project란?
- Projects는 GitHub에서 작업을 계획하고 추적하기 위한 유연하고 적응력 있는 도구
- GitHub 대한 issue 및 pull requests과 통합되어 작업을 효과적으로 계획하고 추적하는 데 도움이 되는 조정 가능한 스프레드시트, 작업 보드 및 로드맵

### 장점
- `최신 상태 유지`: 프로젝트 차트나 보드에서 변경이 일어나면 바로 이슈 탭 등에서 정보가 동기화 됨. 반대로 이슈 탭에서 어떤 변경이 일어나도 프로젝트 탭에서 바로 확인 가능.
- `항목에 메타데이터 추가`: 사용자 지정 필드로 메타데이터 추가 가능
  - A date field to track **target ship dates**.
  - A number field to track the **complexity of a task**.
  - A single select field to track whether a task is Low, Medium, or High **priority**.
  - A text field to add a **quick note**.
  - An iteration field to **plan work week-by-week**, including support for breaks.
- `프로젝트 자동화`: 기본 제공 워크플로를 사용하면 항목이 추가되거나 변경될 때 필드를 자동으로 설정
- `다양한 관점에서 프로젝트 보기`: 보는 방법을 테이블, 타임라인, 로그맵 등으로 볼 수 있음.

## Project 사용 방법

### 1. Project 생성
[조직 프로젝트 생성](https://docs.github.com/ko/issues/planning-and-tracking-with-projects/learning-about-projects/quickstart-for-projects#creating-an-organization-project), [사용자 프로젝트 생성](https://docs.github.com/ko/issues/planning-and-tracking-with-projects/learning-about-projects/quickstart-for-projects#creating-a-user-project) 둘 다 가능

### 2. 프로젝트 설명 및 추가 정보 설정
- 프로젝트의 목적을 공유하고, 프로젝트를 사용하는 방법에 대한 지침을 제공하고, 관련 링크를 포함하도록 프로젝트의 설명 및 추가 정보를 설정
- [설정법](https://docs.github.com/ko/issues/planning-and-tracking-with-projects/learning-about-projects/quickstart-for-projects#setting-your-project-description-and-readme)

### 3. 프로젝트 이슈 추가
- Project의 Table, Board, Roadmap 등에서 [Add Item](https://docs.github.com/ko/issues/planning-and-tracking-with-projects/learning-about-projects/quickstart-for-projects#adding-draft-issues-to-your-project)을 할 수 있음.
- 근데, 보통 여기서 등록하지 않고 `Issues` 탭에서 등록
- Issue는 issue가 생겨서 등록하는게 아니라, 모든 일과를 issue에다가 등록해 놓고 프로젝트 시작
<br/>

1. Repository로 이동
2. Issues 탭으로 이동
3. New Issue 선택
4. Title, Description 작성 및 Assignees, [Lables](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/managing-labels), Projects 선택
5. Create 선택
6. 생성된 이슈의 Projects에서 status도 선택 가능

    <details>
      <summary>GitHub Label 설명</summary>

    - 🎨 Github 기본제공 Label
      | 레이블 이름               | 설명                                                |
      | -------------------- | ------------------------------------------------- |
      | **bug**              | 소프트웨어의 결함이나 오류. 예: 기능이 예상대로 작동하지 않을 때.     |
      | **documentation**    | 문서화 관련 작업. README, 주석, 위키 등.                   |
      | **duplicate**        | 기존에 동일하거나 유사한 이슈/PR이 있는 경우 사용                |
      | **enhancement**      | 기능 개선이나 기존 기능의 확장을 의미                        |
      | **good first issue** | 입문자가 기여하기 좋은 쉬운 이슈에 표시(오픈소스 초보자 환영용)         |
      | **help wanted**      | 외부 기여자의 도움이 필요한 경우 사용                      |
      | **invalid**          | 이슈나 PR이 유효하지 않다고 판단될 때 사용 예: 잘못된 재현 방법 등      |
      | **question**         | 단순 질문성 이슈에 사용. 토론이 필요할 수 있음                    |
      | **wontfix**          | 해당 이슈는 수정하지 않기로 결정했을 때 사용. (우선순위 낮거나 정책상 제외 등) |

    - 🔧 SDLC 기반 GitHub 레이블 활용 예시

      | SDLC 단계                | 기본 레이블 예시                                | 커스텀 레이블 예시          | 색상 추천        | 설명                    |
      | ---------------------- | ---------------------------------------- | ------------------- | ------------ | --------------------- |
      | 🧠 계획 (Planning)       | `question`, `help wanted`                | `🧠 planning`       | 회색 `#cccccc` | 기획 단계 작업에 사용          |
      | 🔍 분석 (Analysis)       | `enhancement`, `discussion`              | `🔍 analysis`       | 파랑 `#1f77b4` | 요구사항 분석 및 검토          |
      | 🎨 설계 (Design)         | `enhancement`, `documentation`           | `🎨 design`         | 보라 `#9467bd` | UI/UX 및 시스템 구조 설계     |
      | 🚀 구현 (Implementation) | `bug`, `enhancement`, `good first issue` | `🚀 implementation` | 초록 `#2ca02c` | 기능 개발, 코드 작성          |
      | 🔧 테스트 (Testing)       | `bug`, `invalid`                         | `🔧 testing`        | 주황 `#ff7f0e` | 버그 확인 및 검증 테스트        |
      | 🛠 유지보수 (Maintenance)  | `wontfix`, `duplicate`, `help wanted`    | `🛠 maintenance`    | 빨강 `#d62728` | 기존 기능 수정, 리팩토링 등 유지보수 |
    </details>

### 4. 이슈 수정
#### 4-1. Github Issues에서 브랜치 생성
1) `Issues` 탭으로 이동
2) `New Issue` 선택
3) 필요한 정보 입력:
  - title 입력
  - Description 입력
  - Assignees 입력
  - Labels 선택
  - Projects 선택
4) `Submit new issue` 선택
5) `Issues` 탭 > 우측의 `Create a branch` 선택
6) 필요한 정보 입력 및 브랜치 생성(`Create branch`)
  - Branch name: 브랜치 이름 입력
  - Repository destination: 리포지토리 선택
  - What's next?: Checkout locally선택
7) 모달에 나와 있는 git script 복사
  ```sh
  git fetch origin
  git checkout <브랜치명>
  ```
8) VS code로 이동 > 7번에서 복사한 코드 복사 및 실행

9) branch 확인
```sh
git branch
```

10) branch로 이동
```sh
git checkout <브랜치명>
```

11) 코드 수정

12) 코드 원격 저장소에 반영
```sh
git add .
git commit -m 'one기능 완료'
git push
```

13) Github 웹사이트 `<> Code` 탭 에 보면 *feat/one had recent pusheds 15 seconds ago* 볼 수 있음 

14) `Compare & pull request` 비교하고 pull request(내가 만든 변경사항(브랜치)를 다른 브랜치에 합쳐달라고 요청하는 행위) 진행

15) 필요한 내용 작성
  - Title 작성
  - Description 작성: 
    - Description에 리뷰어가 코드 리뷰를 잘 할 수 있도록 매우 상세히 작성
    - 여기서는 issue에서 처음부터 생성되었으므로, closes #이슈 할 필요 없이 자동으로 닫힘. 
  - Reviewer 선택
  - Label 선택
  - Projects선택

16) `Create pull request` 선택

17) 리뷰어가 `pull request` > `Conversation`, `Files Changed` 를 보면서 리뷰를 `Write`에 리뷰 작성 > `Start a review`

18) 더 이상 리뷰할 게 없다면 reviewer가 `Files changed` > `Finish your reivew` > `Approve` 함.

19) `Conversation` 탭에서 최종적으로 코드 짠 사람이 `Merge pull request` > `Confirm merge` 버튼 눌러서 merge 진행 

20) `Pull request successfully merged and closed` 문구 옆에 `Delete Branch` 버튼 있음.

21) 이 브랜치는 기능 개발 끝났으므로 `Delete Branch` 눌러서 브랜치 삭제

22) `<>Code` 탭에서 들어가보면 feat/one 브랜치 삭제된 것 확인 가능. (but, 아직 내 vs code에는 feat/one 브랜치 남아있음)

23) 내 VS code에서 feat/one 브랜치 삭제:
  - `git checkout dev` # dev branch로 이동
  - `git pull` # 변경된 코드 가져 옴
  - `git branch -D feat/one` # 기능 개발 끝난 one branch 삭제

#### 4-2. VS Code 에서 브랜치 생성
- 여기서는 [Github branch 전략](#git-branch-strategy)을 쓴다고 가정
- main 브랜치와, dev 브랜치, 그리고 기능개발 브랜치가 있음. -> <br/>
  기능 개발할 때마다 새로운 브랜치를 생성하고 기능개발이 끝나면 dev 브랜치에 merge -> <br/> 
  dev 브랜치의 내용을 main 브랜치에 push
<br/>

1) 기능 브랜치 생성
```sh
# one이라는 기능을 하는 브랜치 생성
git branch feat/one
``` 

2) 브랜치 확인
```sh
git branch
```

3) 브랜치 이동
```sh
git checkout feat/one
```

4) 코드 수정

5) 코드 원격 저장소에 반영
```sh
git add .
git commit -m 'one기능 완료'
git push --set-upstram origin one # one 브랜치 원격 저장소에 올림 + 코드도 올림
```

6) Github 웹사이트 <>code에 보면 "feat/one had recent pusheds 15 seconds ago" 볼 수 있음 

7) `Compare & pull request` 비교하고 pull request(내가 만든 변경사항(브랜치)를 다른 브랜치에 합쳐달라고 요청하는 행위) 진행

8) 필요한 내용 작성
  - Title 작성
  - Description 작성: 
    - Description에 리뷰어가 코드 리뷰를 잘 할 수 있도록 매우 상세히 작성
    - closes #이슈: 마지막에 이 부분 꼭 작성해줘야 함. 여기서 이슈를 선택하여 닫아야만 내가 기존에 만들었던 이슈가 Done 섹션으로 이동하게 됨. 그렇지 않으면 해당 프로젝트에 새로운 이슈 생성 
  - Reviewer 선택
  - Label 선택
  - Projects선택

9) `Create pull request` 선택

10) 리뷰어가 `pull request` > `Conversation`, `Files Changed` 를 보면서 리뷰를 `Write`에 리뷰 작성 > `Start a review`

11) 더 이상 리뷰할 게 없다면 reviewer가 `Files changed` > `Finish your reivew` > `Approve` 함.

12) `Conversation` 탭에서 최종적으로 코드 짠 사람이 `Merge pull request` > `Confirm merge` 버튼 눌러서 merge 진행 

13) `Pull request successfully merged and closed` 문구 옆에 `Delete Branch` 버튼 있음.

14) 이 브랜치는 기능 개발 끝났으므로 `Delete Branch` 눌러서 브랜치 삭제

15) `<>Code` 탭에서 들어가보면 feat/one 브랜치 삭제된 것 확인 가능. (but, 아직 내 vs code에는 feat/one 브랜치 남아있음)

16) 내 VS code에서 feat/one 브랜치 삭제:
  - `git checkout dev` # dev branch로 이동
  - `git pull` # 변경된 코드 가져 옴
  - `git branch -D feat/one` # 기능 개발 끝난 one branch 삭제

<br/>

# 유용한 툴1 - Gitmoji - Commit Emoji by seatonjiang
- 커밋 창에 자동으로 Gitmoji 선택 메뉴 제공
- 원하는 이모지를 클릭하면 메시지에 자동 삽입
- 커밋 스타일 유지에 유용 