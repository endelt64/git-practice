# Git-Practice  
Git 연습을 위한 저장소 ([참고 문서](https://kimseunghyun76.tistory.com/116))

## 목차
* [주요 설정](#주요-설정)
  * [시작](#시작)
    * [SSH 키 생성](#ssh-키-생성)  
    * [공개 키를 authorized_keys 파일에 등록](#공개-키를-authorized_keys-파일에-등록)  
    * [SSH 폴더 및 키 퍼미션 조정](#ssh-폴더-및-키-퍼미션-조정)  
    * [원격지 서버에 공개 키 전송](#원격지-서버에-공개-키-전송)  
    * [SSHD 설정 파일 편집](#sshd-설정-파일-편집)  
    * [공개 키를 GitHub에 등록](#공개-키를-github에-등록)  
    * [GitHub와의 SSH 통신이 활성화되었는지 확인](#github와의-ssh-통신이-활성화되었는지-확인)
    * [GitHub에서 원격 저장소 생성](#github에서-원격-저장소-생성)
    * [Git 저장소 활성화하기](#git-저장소-활성화하기)
    * [개행 문자를 LF로 유지](#개행-문자를-lf로-유지)
  * [저장소와 커밋 관리](#저장소와-커밋-관리)
    * [원격 저장소를 로컬에 추가](#원격-저장소를-로컬에-추가)
    * [기존의 원격 저장소를 로컬에 복제](#기존의-원격-저장소를-로컬에-복제)
    * [원격 저장소에 Push하기](#원격-저장소에-push하기)
    * [이전 커밋으로 돌아갔다가 복귀하기](#이전-커밋으로-돌아갔다가-복귀하기)
    * [원격 저장소 주소 변경하기](#원격-저장소-주소-변경하기)
    * [브랜치 생성 및 이동](#브랜치-생성-및-이동)
    * [전체 브랜치의 목록과 현재 참조하고 있는 브랜치 확인](#전체-브랜치의-목록과-현재-참조하고-있는-브랜치-확인)
    * [브랜치 삭제](#브랜치-삭제)
  * [서브모듈](#서브모듈)
    * [Git 서브모듈 Push하기](#git-서브모듈-push하기)
    * [Git 서브모듈을 포함하여 원격 저장소를 로컬로 가져오기](#git-서브모듈을-포함하여-원격-저장소를-로컬로-가져오기)
* [Fork 저장소 동기화](#fork-저장소-동기화)
    * [원격 저장소 목록에 업스트림 링크 추가](#원격-저장소-목록에-업스트림-링크-추가)
    * [업스트림의 최신 이력 가져오기](#업스트림의-최신-이력-가져오기)
    * [로컬 저장소의 마스터 브랜치로 이동한 후 업스트림의 마스터 브랜치를 로컬 저장소로 병합](#로컬-저장소의-마스터-브랜치로-이동한-후-업스트림의-마스터-브랜치를-로컬-저장소로-병합)
    * [새롭게 병합한 로컬 저장소를 원격 저장소의 마스터 브랜치로 Push](#새롭게-병합한-로컬-저장소를-원격-저장소의-마스터-브랜치로-push)
* [기타](#기타)
    * [gitignore 파일 작성 예시](#gitignore-파일-작성-예시)
    * [로컬 저장소는 그대로 둔 채 원격 저장소에 있는 파일 또는 폴더만 삭제하기](#로컬-저장소는-그대로-둔-채-원격-저장소에-있는-파일-또는-폴더만-삭제하기)
    * [에러 대응](#에러-대응)

## 주요 설정  
### 시작  
#### SSH 키 생성
```
ssh-keygen -t rsa -b 4096 -C "(이메일 주소)"
[-t: 키의 타입 결정(rsa/dsa), -b: 키의 비트 수 결정(4096-bit), -C: 키 끝에 남길 코멘트]
```

'ssh-keygen -t rsa'까지만 입력해도 기본적인 키는 생성이 가능하다.

#### 공개 키를 authorized_keys 파일에 등록
```
cat ~/.ssh/(공개 키 파일) >> ~/.ssh/authorized_keys
```

#### SSH 폴더 및 키 퍼미션 조정
```
chmod 600 ~/.ssh/authorized_keys
chmod 400 ~/.ssh/(공개 키 파일)
chmod 400 ~/.ssh/(개인 키 파일)
```

#### 원격지 서버에 공개 키 전송
```
ssh-copy-id -i (공개 키 파일) (사용자명)@(외부 IP)
```

#### SSHD 설정 파일 편집
sudo vi /etc/ssh/sshd_config
```
Port 2222 [WSL 전용: 윈도우 기본 SSH 포트 22와의 충돌 방지]
PasswordAuthentication yes [선택]
```

#### 공개 키를 GitHub에 등록
윈도우의 경우 MINGW64/WSL 기반 공개 키를 각각 등록한다.

#### GitHub와의 SSH 통신이 활성화되었는지 확인
```
ssh -T git@github.com
```

#### GitHub에서 원격 저장소 생성
Private 또는 Public 저장소를 선택한다.

---

#### Git 저장소 활성화하기
```
git init
git config --global user.name "(이름)"
git config --global user.email "(이메일 주소)"
```

--global로 지정된 명령은 최초 1회만 입력한다.

#### 개행 문자를 LF로 유지
```
git config --global core.eol lf
git config --global core.autocrlf input
```

### 저장소와 커밋 관리
#### 원격 저장소를 로컬에 추가
```
git remote add origin git@github.com:hwooo/git-practice.git
git remote -v
git pull origin master
```

#### 기존의 원격 저장소를 로컬에 복제
```
git clone git@github.com:hwooo/git-practice.git
```

#### 원격 저장소에 Push하기
```
git add -p [스테이징]
git commit -m "(커밋 메시지)" [커밋]
git push origin master [푸시]
(git push -f origin master [현재 원격 저장소의 상태를 무시하고 강제 푸시])
git status
```

#### 이전 커밋으로 돌아갔다가 복귀하기
```
git checkout HEAD~(숫자) [입력한 숫자만큼 이전 단계의 커밋으로 이동 ex) git checkout HEAD~1: 1단계 이전 커밋]
git checkout (복귀할 브랜치 이름)
```

#### 원격 저장소 주소 변경하기
```
git remote set-url origin (SSH 형식의 저장소 링크)
```

---

### 브랜치
#### 브랜치 생성 및 이동
```
git checkout -b (브랜치 이름)
git checkout (브랜치 이름)
```

특정 브랜치에서 파일을 수정한 다음 커밋하지 않은 채 다른 브랜치로 변경하려고 할 경우, 변경 내역이 다른 브랜치로 옮겨지는 문제가 발생하는 것을 막기 위해 오류 메시지가 뜬다.

#### 전체 브랜치의 목록과 현재 참조하고 있는 브랜치 확인
```
git branch
```

#### 브랜치 삭제
```
git branch -d (브랜치 이름)
```

---

### 서브모듈
#### Git 서브모듈 Push하기
```
git submodule add (SSH 형식의 서브모듈 저장소 링크) [.gitmodules 파일 자동 생성]
cd (서브모듈 폴더)
git add --all
git commit -m "서브모듈 커밋 메시지"
git push origin master
cd ..
git commit -am "마스터 커밋 메시지"
git push origin master
```

#### Git 서브모듈을 포함하여 원격 저장소를 로컬로 가져오기
```
git clone --recursive (SSH 형식의 저장소 링크)
```

---

## Fork 저장소 동기화
* 다운스트림(Downstream): Fork된 저장소, 또는 Fork하는 행위
* 업스트림(Upstream): 원본 저장소, 또는 원본 저장소에 Push하는 행위

#### 원격 저장소 목록에 업스트림 링크 추가
```
git remote add upstream (SSH 형식의 원본 저장소 링크)
git remote -v
```

#### 업스트림의 최신 이력 가져오기
```
git fetch upstream
```

#### 로컬 저장소의 마스터 브랜치로 이동한 후 업스트림의 마스터 브랜치를 로컬 저장소로 병합
```
git checkout master
git merge upstream/master
```

#### 새롭게 병합한 로컬 저장소를 원격 저장소의 마스터 브랜치로 Push
```
git push origin master
```

여기서 원격 저장소는 Fork한 저장소를 의미한다.

---

## 기타  
#### gitignore 파일 작성 예시
```
# IntelliJ IDEA configurations
.idea/
META-INF/
out/
*.iml

# Visual Studio Code configurations
.vscode/
*.code-workspace

# Gradle configurations
.gradle/
build/
local.properties

# Packages
node_modules/

# macOS metadata
.DS_Store
```

#### 로컬 저장소는 그대로 둔 채 원격 저장소에 있는 파일 또는 폴더만 삭제하기
```
git rm -r --cached ./(삭제할 파일 또는 폴더)
git commit -m "Remove files/folders"
git push origin master
```

#### 에러 대응
원격 저장소에 Push할 때 "fatal: refusing to merge unrelated histories"

```
git pull origin master --allow-unrelated-histories
git push origin master
```

원격 저장소로부터 Pull할 때 "Pull is not possible because you have unmerged files"
```
git commit -am "(커밋 메시지)"
git pull origin master
```

원격 저장소로부터 Pull할 때 "Please commit your changes or stash them before you merge"
* `stash` 명령어 참고: https://gmlwjd9405.github.io/2018/05/18/git-stash.html
```
git stash [수정한 내역을 임시 저장하고 마지막 커밋 이후, 마지막 수정 이전(스테이징 이전)의 시점으로 되돌림]
git pull origin master

git stash pop [임시 저장한 내역을 꺼내서 현재 소스 코드와 병합]
(git stash pop --index [스테이징 기록을 유지한 채 임시 저장한 내역을 꺼내옴])
(git stash drop [임시 저장한 내역을 꺼내오지 않고 삭제])

git commit -m "(커밋 메시지)"
git push origin master
```
