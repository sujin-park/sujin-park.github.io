---
title: '[Git] Git tag, Release Drafter로 release note 작성하기'
date: 2021-09-04 16:32:00
category: 'web'
draft: false
---

## Git tag

Git에서 `tag`란 특정 커밋을 태깅하는 것으로 특정 커밋을 가리키는 링크라고 할 수 있다. 커밋은 checkout 하여 작업 내용에 대해 수정할 수 있지만, tag는 수정이 불가능하다. **tag는 보통 소프트웨어의 버전을 명시하거나 릴리즈 할 때 사용한다.**

### tag 조회

```bash
# 이미 존재하는 모든 tag 조회
$ git tag
v1.1.0
v1.2.0
v1.3.0
v1.4.0
v1.5.0

# 이미 존재하는 tag 중 검색패턴을 사용하여 조회
$ git tag -i "v1.1*"
v1.1.0
v1.1.1
v1.1.2
v1.1.3
v1.1.4-rc

# tag가 붙은 커밋에 대한 정보 조회
$ git show <tag-name>
commit ab392deeb30da181745819b168f5d07ec8beb759 (tag: v1.1.0)
Author: sujin-park <psujin831@gmail.com>
Date:   Wed Jun 9 09:03:47 2021 +0900

    feature: add banner component
...
```

### tag 생성

tag 이름을 설정하여 로컬 저장소에 tag 를 생성한다. 특정 commit hash 를 뒤에 붙이지 않으면 tag 는 현재 HEAD 에 생성이 된다.

```bash
# HEAD 에 tag 생성
$ git tag <tag-name>

# 특정 commit 에 tag 생성
$ git tag <tag-name> <commit-hash>
```

### 원격 저장소에 tag push

원격 저장소에 특정 tag만 push 하거나 모든 tag 를 push 할 수 있다.

```bash
# 원격 저장소로 특정 tag push
$ git push origin <tag-name>

# 원격 저장소로 모든 tag push
$ git push origin --tags 
```

### tag 삭제

로컬 저장소 또는 원격 저장소에 있는 tag를 삭제할 수 있다.

```bash
# 로컬 저장소 특정 tag 삭제
$ git tag -d <tag-name>

# 원격 저장소 특정 tag 삭제
$ git tag origin :tags/<tag-name>
```

## Git Release

위에서 소프트웨어 릴리즈 시 버전을 명시하기 위해 사용하는 Git tag에 대해서 보았다. Git tag 는 릴리즈(Release) 기능을 사용할 때 사용하는 tag 로 tag 를 사용하는 예시 중 릴리즈(Release) 에 대해 보고자 한다.


<img width="1527" alt="스크린샷 2021-09-04 오후 4 06 26" src="https://user-images.githubusercontent.com/29244798/132086854-f54fd0c3-954c-490c-a475-54587d19db06.png">


Github 페이지의 오른쪽 탭을 보면 Releases 항목에서 `Create a new release` 버튼을 눌러 새로운 릴리즈를 작성할 수 있다.

<img width="1274" alt="스크린샷 2021-09-04 오후 4 07 40" src="https://user-images.githubusercontent.com/29244798/132086850-f7a5c549-cc62-4132-853f-93ea35e6e5e2.png">

위 스크린샷을 기준으로 본다면 아래와 같은 내용을 첨부할 수 있다.

- tag
    - 원격 저장소에 있는 모든 tag 선택 가능
    - Choose a tag 를 클릭하면 아래 항목에 바로 tag 를 생성할 수 있는 기능도 있다.
- target 브랜치: master
    - target은 원격 저장소에 있는 모든 브랜치 선택 가능
- 릴리즈 제목
- 릴리즈 내용
- 소스코드를 빌드한 바이너리 파일

위처럼 릴리즈 제목과 릴리즈 내용 등을 직접 작성하는데, 어떤 내용이 이번 릴리즈에 포함되는지 매번 PR 제목과 author 등을 복사 + 붙여넣기를 통해 릴리즈 내용을 채워나가는건 번거로운 일이다. 번거로운 것 뿐만 아니라 수동으로 릴리즈 내용을 채우다보면 누락하는 작업 내용도 있을 것이다.

Release 를 자동화 해줄 수 있는 [Release Drafter](https://github.com/marketplace/actions/release-drafter) 를 함께 알아보고자 한다.


## Github Release Drafter

GitHub Action workflows에서 Release Drafter GitHub 액션을 아래와 같이 YAML 기반 워크플로우 파일을 구성하여 사용할 수 있다.

<img width="380" alt="스크린샷 2021-09-04 오후 4 25 15" src="https://user-images.githubusercontent.com/29244798/132086856-6f96031e-bebd-40bc-b80c-b0dc0d87edd7.png">


- `.github/workflows/release-drafter.yml` : workflow 파일
- `.github/release-drafter.yml` : 설정 파일

### release-drafter.yml

```yaml
name: Release Drafter

on:
  push:
    # branches to consider in the event; optional, defaults to all
    branches:
      - develop
  # pull_request event is required only for autolabeler
  pull_request:
    # Only following types are handled by the action, but one can default to all as well
    types: [opened, reopened, synchronize]

jobs:
  update_release_draft:
    runs-on: ubuntu-latest
    steps:
      # (Optional) GitHub Enterprise requires GHE_HOST variable set
      #- name: Set GHE_HOST
      #  run: |
      #    echo "GHE_HOST=${GITHUB_SERVER_URL##https:\/\/}" >> $GITHUB_ENV

      # Drafts your next Release notes as Pull Requests are merged into "develop"
      - uses: release-drafter/release-drafter@v5
        # (Optional) specify config name to use, relative to .github/. Default: release-drafter.yml
        # with:
        #   config-name: my-config.yml
        #   disable-autolabeler: true
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```


## 참고

- [https://medium.com/pozalabs/github-repository-release-반쯤-자동화하기-30e4738e5283](https://medium.com/pozalabs/github-repository-release-%EB%B0%98%EC%AF%A4-%EC%9E%90%EB%8F%99%ED%99%94%ED%95%98%EA%B8%B0-30e4738e5283)
- [https://git-scm.com/book/en/v2/Git-Basics-Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
- [https://github.com/marketplace/actions/release-drafter](https://github.com/marketplace/actions/release-drafter)