---
title: '[Git] Git hooks를 이용하여 commit message 에 특정 문자 넣기'
date: 2021-12-19 11:50:00
category: 'web'
draft: false
---

## Git Hooks 란

Git은 특정 이벤트가 발생했을 때 특정 스크립트를 실행할 수 있도록 Hook 이라는 기능을 지원한다. 프로젝트에 커밋을 하거나 푸시하기 전에 린트 또는 테스트를 실행할 수 있고, commit message 작성에 도움을 주는 hook 또한 있다. 

<br>

Hooks 는 기본적으로 프로젝트의 `.git/hooks` 에 위치한다. 

<img width="658" alt="스크린샷 2021-12-05 오후 8 55 39" src="https://user-images.githubusercontent.com/29244798/146679364-308e4534-9dfd-4850-b9f0-59740e8dd0ec.png">

<br>

위 예제는 shell script 와 Perl script 로 작성되어 있지만, 실행할 수 있다면 Ruby 또는 Python 과 같이 익숙한 언어로 작성할 수도 있다. 위 예제에서 `.sample` 확장자만 제거하여도 바로 hook을 사용할 수 있다.

<br>

sample 파일은 그대로 유지하고 새로 생성하고 싶다면 아래와 같이 touch 명령어를 이용하여 파일을 생성할 수 있다. 생성만 하면 되는 것은 아니고 `실행할 수 있는 권한`을 줘야 한다. 

```bash
$ vi GIT_REPOSITORY/.git/hooks/pre-commit
$ chmod +x GIT_REPOSITORY/.git/hooks/prepare-commit-msg
```

<br>

## Hooks 종류

### pre-commit

커밋할 때 가장 먼저 호출되는 Hook 으로 커밋 메시지를 작성하기 전에 호출된다. pre-commit hook 에서는 린트를 실행하여 코드 스타일을 검사하거나, 라인 끝의 공백 문자를 검사하거나 테스트를 한다.

아래 명령어를 실행하면 commit 시 pre-commit hook 을 `생략`할 수 있다.

```bash
$ git commit --no-verify
```

### prepare-commit-msg

커밋 메시지를 수정하기 전에 먼저 prefix 또는 suffix 를 붙이는 것과 같이 hook 을 통해 커밋 메시지를 손보고 싶을 때 사용한다.

### commit-msg

이 hook 은 커밋 메시지가 들어 있는 임시 파일의 경로를 argument 로 받고 스크립트가 0이 아닌 값을 반환하면 커밋이 되지 않는다. 이 hook에서는 최종적으로 커밋이 완료되기 전에 프로젝트 상태나 커밋 메시지를 검증한다.

### post-commit

커밋이 완료되면 post-commit hook이 실행되므로 post-commit 은 커밋한 것을 동료나 다른 프로그램에 노티를 줄 때 사용할 수 있다.

### pre-rebase

rebase 하기 전에 실행되는 훅으로 이미 push 한 커밋을 rebase 하지 못하게 할 수 있는 hook 으로 사용한다. Git 이 기본적으로 제공해주는 pre-rebase.sample 코드에 예제 또한 있다.

### post-rewrite

커밋을 변경하는 명령을 실행했을 때 실행되는 훅으로 용량이 크거나 Git 이 관리하지 않는 파일을 옮길 때, 문서를 자동으로 생성할 때 사용한다.

### post-merge

Merge 가 끝나고 나서 실행되는 hook이다.

<br>

## Hooks 사용 예제

동료와 협업할 때 기본적으로 Git commit message 에 대해 포맷을 논의하고 동일하게 작성하려고 노력을 한다. 이 노력을 각자 할 수도 있겠지만, 이번에는 Git hooks 중 prepare-commit-msg 를 사용하여 Git 이 어느정도 자동으로 해줄 수 있는 방법을 소개해보려고 한다.

<br>

**가이드**

- 하나의 저장소 내에 여러 개의 프로젝트가 존재하여 프로젝트의 이름을 prefix 로 작성
- feature, refactoring, fix 등 해당 커밋의 목적이 무엇인지 작성
- 마지막으로 작성된 커밋 메시지
    
    

위 요구사항에 부합하는 prepare-commit-msg 를 아래와 같이 작성해보았다.

```tsx
COMMIT_MSG_FILE=$1

BRANCH_NAME=`git rev-parse --abbrev-ref HEAD`
PROJECT_NAME=''
PREFIX=''

if [[ "${BRANCH_NAME}" == *"/"* ]];then
    PROJECT_NAME=`echo ${BRANCH_NAME} | cut -d '/' -f1`
    PREFIX=`echo ${BRANCH_NAME} | cut -d '/' -f2 | cut -d '-' -f1`
fi

FIRST_LINE=`head -n1 ${COMMIT_MSG_FILE}`

if [[ ${PROJECT_NAME} == '' ]]; then
    exit
fi

if [ -z "$FIRST_LINE" ]; then
    sed -i ".bak" "1s/^/[$PROJECT_NAME] $PREFIX /" ${COMMIT_MSG_FILE}
fi
```

가이드에 부합하지 않는 브랜치를 복잡한 정규식을 활용하여 예외케이스를 작성해주면 좋을 것 같았지만 우선 1차적으로 구현을 위해 위와 같이 작성하였다.

그럼 이제 실제로 커밋을 생성한다고 가정해보려고 한다.

아래는 위 가이드를 만족하는 테스트 브랜치명이다. 브랜치명을 `project-name/feature-XXX` 라고 생성하고  

`git commit` 명령어를 실행하면 아래와 같이 [project] feature 라는 접두사가 붙는다.

<img width="582" alt="스크린샷 2021-12-19 오후 11 39 39" src="https://user-images.githubusercontent.com/29244798/146679378-38f64534-f0a0-4794-9244-a67eb8615551.png">

<br>
가이드를 만족하지 않는 브랜치명을 작성하게 되면 아래와 같이 공백인 것을 볼 수 있다.

<img width="579" alt="스크린샷 2021-12-19 오후 11 40 01" src="https://user-images.githubusercontent.com/29244798/146679398-b61a3b5a-404d-4f7d-800a-8207815354d1.png">


<br>
<br>
<br>

**참고**

[https://mincong.io/2019/07/23/prepare-commit-message-using-git-hook/](https://mincong.io/2019/07/23/prepare-commit-message-using-git-hook/)

[https://ohgyun.com/639](https://ohgyun.com/639)

[https://techblog.woowahan.com/2530/](https://techblog.woowahan.com/2530/)

[https://git-scm.com/docs/githooks](https://git-scm.com/docs/githooks)