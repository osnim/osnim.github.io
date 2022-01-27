---
title: jekyll(지킬)로 GitHub 블로그 만들기 (Windows10, 64bit)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-01-26 21:00:00 +0900
categories: [blog, jekyll]
tags: [Github, blog, jekyll]
---

# Github 블로그 만들기

1. 자신의 Github에서 Repository를 새로 만듭니다. 중요한건 Repository 이름을 `<자신의 github이름>.github.io`로 설정해야 합니다. 제 블로그는 [osnim GitHub 블로그](https://osnim.github.io) 입니다.

2. README file을 체크합니다. (안해도 상관 없지만 저는 하는 것을 추천합니다.)

3. 그 Repository를 로컬로 clone 합니다. ZIP파일로 다운받아서 압축을 풀어도 되고 HTTPS 주소를 복사하여 프롬프트에서 아래 명령어를 실행해도 상관없습니다.

- ```console
  git clone 복사한 주소
  ```

압축을 푼 Explorer나 git clone을 실행한 위치에 Repository의 파일이 생성됩니다. 이 [**경로**]를 잘 기억해두셔야 합니다.

4. git push

: ```console"
: git add --all
: git commit -m "커밋 메시지"
: git push -u origin main
: ````

5. 로컬에서 push한 github 블로그를 주소창에 넣어 확인합니다

> `https://<자신의 github이름>.github.io`

# jekyll를 Github 블로그에 적용

## 1. Ruby Install (Ruby 다운 및 설치)

저는 이번 GitHub 블로그를 만들면서 jekyll(지킬)에서 제공하는 Theme(테마)를 사용했습니다.
jekyll은 ruby라는 언어로 제작되었습니다. [**ruby installer**][ruby-installer] 사이트 가셔서 Ruby를 다운받고 설치를 해야합니다. (참고로 저는 Windows10 64bit 운영체제를 사용하고 있습니다.)

사이트에 들어가시면 `Ruby, we recommend that you use the Ruby+Devkit 2.7.X (x64) installer.` 라는 문구를 오른쪽에서 확인 할 수 있습니다. 저도 안정된 `Ruby+Devkit 2.7.5-1(x64)` 버전을 사용했고 잘 되었습니다.
설치시 옵션을 선택할 때 `Use UTF-8 as default external encoding.`을 꼭 체크해야합니다. 저는 3개 모두 체크했습니다.

## 2. jekyll을 로컬에 설치

Ruby 설치가 완료되면 `Start Command Prompt with Ruby` 프롬프트를 실행합니다.
처음 실행하면 경로가 `C:\Users\사용자이름` 인데 여기에 다음과 같은 명령을 넣고 실행하여 로컬에 jekyll을 설치합니다.

```console
gem install jekyll bundler
```

프롬프트 콘솔 창에서 이전에 clone한 로컬 [**경로**]인 github.io 폴더로 이동하여 다음 명령어를 순차적으로 입력합니다.

## 3. Jekyll 생성 및 jekyll을 로컬 서버에서 실행

```console
jekyll new ./
bundle install
bundle exec jekyll serve
```

이렇게 입력하고 [**http://127.0.0.1:4000/**](http://localhost:4000/) 또는 [**http://localhost:4000/**](http://localhost:4000/)를 인터넷 주소창에 입력하여 로컬 서버에서 jekyll이 적용된 Github 블로그를 확인할 수 있습니다. 하지만 이는 자신의 컴퓨터에서만 확인 가능하므로 다른 사람도 접근 가능할 수 있게 Github repository에 push를 해줘야 합니다.

`<자신의 github이름>.github.io` repository에 다음과 같은 명령어를 입력하여 push합니다.

```consol
git add .
git commit -m " 커밋 메세지"
git push
```

모두 입력 후 브라우저 주소창에 `<자신의 github이름>.github.io`를 입력하여 자신의 github 블로그를 확인합니다.

[ruby-installer]: https://rubyinstaller.org/downloads
