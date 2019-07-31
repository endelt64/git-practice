# Git-Practice  
Git 연습을 위한 저장소

## 주요 설정  
#### SSH 키 생성 및 퍼미션 조정:
```
ssh-keygen -t rsa -b 4096 -C "(이메일 주소)"
[-t: 키의 타입 결정(rsa/dsa), -b: 키의 비트 수 결정(4096-bit), -C: 키 끝에 남길 코멘트]
```

#### 공개 키(*.pub)를 authorized_keys 파일에 등록:
```
cat ~/.ssh/(공개 키 파일) >> ~/.ssh/authorized_keys
```

#### SSH 폴더 및 키 퍼미션 조정
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chmod 400 ~/.ssh/(공개 키 파일)
chmod 400 ~/.ssh/(개인 키 파일)
```

#### SSH 에이전트에 개인 키 등록
```
eval "$(ssh-agent -s)" [아래의 ssh-add 명령어가 동작하지 않을 경우]
ssh-add ~/.ssh/(개인 키 파일)
```

#### 원격지 서버에 공개 키 전송
```
ssh-copy-id -i (공개 키 파일) (사용자명)@(외부 IP)
```

#### /etc/ssh/sshd_config 파일 설정
```
Port 2222 [WSL 전용: 윈도우 기본 SSH 포트 22와의 충돌 방지]
PermitRootLogin without-password
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2
PasswordAuthentication yes [선택]
```

#### [공개 키를 GitHub에 등록(윈도우의 경우 MINGW64/WSL 기반 공개 키를 각각 등록)]

#### GitHub와의 SSH 통신이 활성화되었는지 확인:
```
ssh -T git@github.com
```

#### [GitHub에서 원격 저장소 생성]

#### Git 시작하기(--global로 지정된 명령은 최초 1회만 입력):
```
git init
git config --global user.name "(이름)"
git config --global user.email "(이메일 주소)"
```

#### 개행 문자를 LF로 유지:
```
git config --global core.eol lf
git config --global core.autocrlf input
```

#### 원격 저장소를 로컬에 추가:
```
git remote add origin git@github.com:NDelt/Git-Practice.git
git remote -v
git pull origin master
```

#### 기존의 원격 저장소를 로컬에 복제:
```
git clone git@github.com:NDelt/Git-Practice.git
```

#### 원격 저장소에 Push하기:
```
git status
git add -p
git commit -m "(커밋 메시지)"
git push origin master
```

## Fork한 저장소를 원본 저장소의 최신 상태로 동기화하기
* 다운스트림(Downstream): 어떤 브랜치로부터 Pulling한 자식 브랜치
* 업스트림(Upstream): 자식 브랜치가 pushing하는 부모 브랜치

#### 원격 저장소에 업스트림 브랜치 추가:
```
git remote add upstream (SSH 형식의 원본 저장소 링크)
git remote -v
```

#### 업스트림 브랜치의 최신 이력 가져오기:

## 기타  
#### .gitignore 파일 작성하기(예시):
```
# IntelliJ IDEA configurations
.idea/
META-INF/
out/

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

#### 원격 저장소 주소 변경하기:
```
git remote set-url origin (SSH 형식의 저장소 링크)
```

#### 원격 저장소에 Push할 때 "fatal: refusing to merge unrelated histories" 에러 발생:
```
git pull origin master --allow-unrelated-histories
git push origin master
```

#### 원격 저장소로부터 Pull할 때 "Pull is not possible because you have unmerged files" 에러 발생:
```
git commit -am "(커밋 메시지)"
git pull origin master
```

#### 로컬 저장소는 그대로 둔 채 원격 저장소에 있는 파일/폴더만 삭제하기:
```
git rm -r --cached (삭제할 파일/폴더)
git commit -m "Remove files/folders"
git push origin master
```
