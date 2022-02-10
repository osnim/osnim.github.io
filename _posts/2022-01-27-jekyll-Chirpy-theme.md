---
title: GitHub 블로그에 Chirpy jekyll 테마 (theme) 적용하기
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-01-27 15:22:00 +0900
categories: [jekyll]
tags: [blog, jekyll, theme, Chirpy]
---

# jekyll 테마 찾기

<https://jekyll-themes.com/free/>

저는 이 많은 무료 테마 중에서 다음과 같은 이유로 [**chirpy**](https://jekyll-themes.com/chirpy/) 테마를 선택하게 되었습니다.

1. 왼쪽에 카테고리나 네비게이터가 있어 사용자 입장에서 이동이 자유로우며 현재 위치가 어디인지 쉽게 파악이 가능함
2. 검색 기능으로 쉽게 글이나 자료를 찾을 수 있음
3. 프로필 사진 첨부와 브라우저 탭에 icon을 넣을 수 있어 차별화 된 블로그 제작 가능
4. 블로그 첫 페이지에서 포스트의 제목과 글이 간략하게 나와서 복잡해 보이지 않음
5. jekyll 테마 중에선 비교적 유명하여 문제가 발생하면 다른 사람들이 사용했던 방법들을 찾아 볼 수 있음
6. 꾸준한 관리로 최근까지 업데이트 되어있음

**이제 부터는 `chirpy` 테마 위주로 글을 작성하겠습니다. 다른 테마는 적용 되지 않을 수 있으니 참고 바랍니다!**

# Prerequisites

Jekyll, ruby, bundler 등을 사전에 설치해야 chirpy를 수월하게 설치할 수 있습니다.
설치 방법은 제 이전 포스트에 나와있습니다. 주소는 아래와 같습니다.

[jekyll(지킬)로 GitHub 블로그 만들기](https://osnim.github.io/posts/jekyll-GitHub/)

만약 여기서 이해가 잘 안된다면 제가 참조한 블로그들에서 확인 부탁드립니다.
블로그들은 이 포스트 제일 하단에 있습니다.

## 참고사항

저는 GitHub Desktop 앱을 사용해서 제가 작성한 커맨드를 모두 사용하지는 않습니다. GitHub Desktop 이 매우 편리하지만 모든 사람들이 GitHub Desktop 앱을 사용하지 않아서 커맨드 형식으로 포스트를 작성했습니다. 문제가 생기는 경우 메일 주세요!

# chirpy-starter로 레포지터리 생성

저는 제가 참조했던 블로그 들과 다르게 했습니다. 먼저 아래 사이트로 접속해서 레포지터리를 새로 만듭니다.

[Chirpy-starter](https://github.com/cotes2020/chirpy-starter/generate)

**중요**한건 리포지토리 이름을 `<자신의 github이름>.github.io`로 설정해야 합니다.

만약 이전에 `<자신의 github이름>.github.io`로 레포지터리를 만드셨다면, 이 리포지토리를 삭제하고 다시 생성하시기 바랍니다.

삭제하는 방법은 **settings**에 들어가셔서 맨 아래 가시면 **Delete this repository**를 선택하시면 됩니다.

자신의 컴퓨터에서 원하는 위치에 이동해서

**cmd** 나 **Start Command Prompt With Ruby**를 실행하여

위에서 생성한 자신의 리포지토리 주소를 입력후 git clone을 실행합니다.

또는 자신의 리포지토리를 깃허브에서 ZIP파일로 다운 받아 압축을 해제합니다.

그 후 다음 명령어를 실행합니다.

```console
 $ bash tools/init.sh
```

만약 bash가 실행이 안되거나 bash가 설치가 안되어있는 경우 아래 사이트를 참조해서 bash를 설치하시기 바랍니다.

[Windows10 에서 bash 설치](https://jhnyang.tistory.com/441)

저는 bash 때문에 시간이 많이 뺏겼고 아무도 이 문제가 발생하지 않아 저 혼자 해결하느라 시간이 많이 걸렸습니다. 제가 이 글을 남기는 이유도 여기에 있습니다.

**init.sh** 명령어는 `<자신의 github이름>.github.io` 폴더에서

.travis.yml 파일과,

docs 폴더,

\_posts폴더 안에있는 파일들을 삭제해주는 기능이 있습니다.

(A) Gemfile.lock을 .gitignore 텍스트 파일 맨 아래에 추가하고 저장합니다. 그리고 Gemfile.lock을 삭제합니다.

**(A) 부분은 chirpy 블로그의 [getting-started](https://chirpy.cotes.page/posts/getting-started/) 포스트의 내용과 다르지만 저는 이렇게 해서 오히려 잘 되어 이 방법을 사용하고 있습니다. 자신에게 맞는 방법을 하면 될 것 같습니다.**

**그리고 Gemfile.lock을 삭제해도 다시 생기는 경우가 있는데 계속 삭제 안해도 저는 잘 되어서 .gitignore 파일에만 Gemfile.lock을 추가하면 되는 것 같습니다.**

아까 git clone이나 ZIP파일을 압축 해제한 경로에서 다음 명령어를 실행합니다.

```console
 $ bundle exec jekyll s
```

# 첫 블로그 페이지 설정 및 변경

\_config.yml 파일을 열어서 수정 합니다. 저는 `VScode` 편집기로 다음과 같이 수정했습니다.

timezone: Asia/Seoul

title: osnim

tagline: Python, Android(java), React, Node.js, C++, C, Linux.

url: "https://osnim.github.io"

username: osnim

name: osnim

email: alsth4@naver.com

links: 는 다른 부분은 두고 url 부분만 수정했습니다.

theme_mode: [light]

아래 부터는 프로필 사진을 변경하는 부분입니다. 여기 자세한 부분은 따로 포스트 할 예정이니 일단 넘어가 주세요

img_cdn: "https://cdn.jsdelivr.net/gh/osnim/
Images@edc68e382d111f90dae130584bed36d2c5be1015"

vatar: "/commons/hi_noglasses.jpg"

# Running Local Server

git clone을 실행한 위치 또는 chirpy 테마가 설치된 폴더 경로로 이동하셔서 다음 명령어를 실행합니다.

```console
 $ bundle exec jekyll s
```

로컬 서비스가 실행되고 다음 주소를 인터넷 브라우저 주소창에 입력해주시기 바랍니다.

<http://127.0.0.1:4000>

만약 로컬 서비스를 종료하고 싶은 경우cmd 나 Start Command Prompt With Ruby에서 ctrl + c 를 입력하면 됩니다.

이 페이지는 다른사람은 보지 못하고 오로지 자신의 컴퓨터에서만 확인이 가능합니다.

# 실제 GitHub 블로그에 적용하기

위에서 변경한 사항이 모두 로컬 서비스에서 적용이 잘 되었으면 이제 자신의 github.io  
블로그에 chirpy 테마를 진짜로 적용시킬 차례입니다. 저는 블로그 글까지 적용 잘 되는지 <https://github.com/cotes2020/jekyll-theme-chirpy/tree/master/_posts>의 포스트들을 제 \_post 폴더에 넣어 글이 잘 게시되는지 확인하는 방법으로 진행했습니다.

**cmd** 나 **Start Command Prompt With Ruby**에서 **git push**를 진행합니다.

```console
   git add --all
   git commit -m "커밋 메시지"
   git push -u origin main
```

여기서부터가 가장 중요합니다. chirpy는 다른 jekyll 테마와 달리 브랜치가 main이 아닌 chirpy 제작자가 만든 gh-pages라는 branch를 새로 생성해서 `bot`을 이용하여 자동으로 `pages build and deployment`을 실행해서 GitHub에 반영합니다.

이와 관련해서 자세한 사항은 [**chirpy getting-started**](https://chirpy.cotes.page/posts/getting-started/#usage) 페이지를 확인하면 될 것 같습니다.

만약 `pages build and deployment`에서 빨간 불이 들어온다면 글 제목이나 Gemfile에서 문제가 생긴 것이니 다시 한번 확인 부탁드립니다.

**글 제목의 경우 되도록 영어로 작성하기를 권장드리며, 띄어쓰기 대신 - 를 사용해주세요**

**gh-pages**라는 branch를 적용하는 방법은 자신의 GitHub 페이지로 이동해서 `<자신의 github이름>.github.io` 리포지토리로 이동합니다.

그 후 Settings의 Pages 메뉴로 이동해서 Branch를 main에서 gh-pages로 선택하고 **/(root)**폴더는 그대로 놓고 **save** 버튼을 클릭합니다.

만약 **gh-pages** Branch가 생성되지 않은 경우

bash tools/init.sh 에서 문제가 생겼거나 Gemfile.lock에서 문제가 생긴 것일 수 있으니 .gitignore 파일에서 Gemfile.lock을 넣어보시고 안되면 리포지토리를 삭제하고 처음부터 다시 하기를 권장합니다. 저 또한 그랬습니다.

save까지 잘 되었다면 이제 리포지토리의 **Actions**로 이동하셔서 블로그에 적용되는 진행사항을 볼 수 있습니다.

Action에서 노란 불이 들어오며 진행상황을 알려줍니다. 평균 30초~ 2분 정도 소요되며 이보다 더 오래 걸린적도 있지만 5분을 넘기진 않는 것 같습니다.

그리고 초록불이 들어와 GitHub 블로그에 적용이 되었다고 하더라도 실제 저희가 보는 데에는 시간이 더 걸리니 10초~30초 정도 블로그를 확인해보시기 바랍니다.

저도 처음에는 변경할 때 마다 블로그에 적용했는데 이러면 시간이 오래 걸리고 바로 확인을 못하는 문제가 발생했습니다.

그래서 지금은 로컬에서 먼저 수정을 하고 최종만 블로그에 포스팅하는 방식으로 진행하고 있습니다.

혹시라도 잘 안되는 부분이 있으면 왼쪽 메일로 문의 주세요!

제 블로그를 방문해주셔서 정말 감사합니다.

---

## 참조한 사이트

여기는 제가 참조한 사이트입니다.

<https://chirpy.cotes.page/>

<https://kkminseok.github.io/posts/startblog/>

<https://blog.kimzinu.com/posts/jekyll-4/>

<https://zeddios.tistory.com/1223?category=682196>
