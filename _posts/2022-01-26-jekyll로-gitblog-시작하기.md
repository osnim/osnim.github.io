---
title: jekyll(지킬)로 GitHub 블로그 만들기 (Windows10, 64bit)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-01-26 21:00:00 +0900
categories: [블로그, jekyll]
tags: [Github, 블로그, jekyll, 지킬]
pin: true
---

## 서론

### 내가 GitHub 블로그로 기록을 남기는 이유

나도 이제 대학교를 졸업하고 제대로 취업 준비를 하려고 한다. 내 동기들은 모두 4학년에 취업준비를 시작하지만 나는 1학년 때 학점을 잘 챙겨놓지 못해서 4학년 1,2학기 21. 20 학점을 들었다. 그래서 취업 준비를 본격적으로 하지 못했다.  
4학년 때 자격증도 따고 서류도 넣어보고필기시험도 보러 가며 나 나름대로 취업시장을 조금이나가 경험했지만 악착같이 열심히 하지 않았다. 전공 수업과 학점이 그 당시에는 더 중요했기 때문이다.

학점을 많이 복구해서 내 기준에서의 좋은 학점까지 만들 수 있었다. 또한 내가 배우고 싶은 강의를 들으며 원하던 공부도 다 해봤다고 생각해서 이제는 온전히 취업에 신경 쓸 수 있을 것 같다. 그래서 이번 주 부터 포트폴리오, OPIC, 코딩테스트, CS에만 전념할 것이다.

그래서 이렇게 처음 Github 블로그 작성에 보다 신경을 많이 쓰고 있으며 글도 많이 남길 것이다.
예전의 나 였으면 Github 블로그만 만들고 그 과정에서 생긴 Error나 당황했던 내용을 모두 남기지 않고 글도 안 썼을테지만 지금은 최대한 기록들을 남기고 싶다.
이렇게 복습하면서 공부도 하고 문제를 구글링하여 다시 찾아 해결하는 번거로움을 덜어 낼 수 있을 것이다. 하지만 가장 큰 이유는 내가 아무런 비용 없이 받았던 도움들을 또 다른 누군가에게 베풀수 있다고 생각하기 때문에 이렇게 기록을 남길 것 이다.

## Github 블로그를 만들기

1. 자신의 Github에서 Repository를 새로 만듭니다. 중요한건 Repository 이름을 `<자신의 github이름>.github.io`로 설정해야 합니다. 저의 경우는 [**내 GitHub 블로그**](https://osnim.github.io) 입니다.

2. README file을 체크합니다. (안해도 상관 없지만 저는 하는 것을 추천합니다.)

3. 그 Repository를 로컬로 clone 합니다. ZIP파일로 다운받아서 압축을 풀어도 되고 HTTPS 주소를 복사하여 프롬프트에서 아래 명령어를 실행해도 상관없습니다.

```console
git clone 복사한 주소
```

압축을 푼 Explorer나 git clone을 실행한 위치에 Repository의 파일이 생성됩니다. 이 [**경로**](#경로)를 잘 기억해두셔야 합니다.
[**Using the Chirpy Starter**](#option-1-using-the-chirpy-starter)

4. git push

```console
git add --all
git commit -m "커밋 메시지"
git push -u origin main
```

5. 로컬에서 push한 github 블로그를 주소창에 넣어 확인합니다

   > `https://<자신의 github이름>.github.io`

## jekyll로 Github 블로그를 만들기

### 1. Ruby Install (Ruby 다운 및 설치)

저는 이번 GitHub 블로그를 만들면서 jekyll(지킬)에서 제공하는 Theme(테마)를 사용했습니다.
jekyll은 ruby라는 언어로 제작되었습니다. [**ruby installer**][ruby-installer] 사이트 가셔서 Ruby를 다운받고 설치를 해야합니다. (참고로 저는 Windows10 64bit 운영체제를 사용하고 있습니다.)

사이트에 들어가시면 `Ruby, we recommend that you use the Ruby+Devkit 2.7.X (x64) installer.` 라는 문구를 오른쪽에서 확인 할 수 있습니다. 저도 안정된 `Ruby+Devkit 2.7.5-1(x64)` 버전을 사용했고 잘 되었습니다.
설치시 옵션을 선택할 때 `Use UTF-8 as default external encoding.`을 꼭 체크해야합니다. 저는 3개 모두 체크했습니다.

### 2. jekyll을 로컬에 설치

Ruby 설치가 완료되면 `Start Command Prompt with Ruby` 프롬프트를 실행합니다.
처음 실행하면 경로가 `C:\Users\사용자이름` 인데 여기에 다음과 같은 명령을 넣고 실행하여 로컬에 jekyll을 설치합니다.

```console
gem install jekyll bundler
```

# 경로

프롬프트 콘솔 창에서 이전에 clone한 로컬 경로인 github.io 폴더로 이동하여 다음 명령어를 순차적으로 입력합니다.

### 3. Jekyll 생성 및 jekyll을 로컬 서버에서 실행

```console
jekyll new ./
bundle install
bundle exec jekyll serve
```

이렇게 입력하고 `http://127.0.0.1:4000/` 또는 `http://localhost:4000/`를 인터넷 주소창에 입력하여 로컬 서버에서 jekyll이 적용된 Github 블로그를 확인할 수 있습니다. 하지만 이는 자신의 컴퓨터에서만 확인 가능하므로 다른 사람도 접근 가능할 수 있게 Github repository에 push를 해줘야 합니다.

`<자신의 github이름>.github.io` repository에 다음과 같은 명령어를 입력하여 push합니다.

```consol
git add .
git commit -m " 커밋 메세지"
git push
```

모두 입력 후 브라우저 주소창에 `<자신의 github이름>.github.io`를 입력하여 자신의 github 블로그를 확인합니다.

[ruby-installer]: https://rubyinstaller.org/downloads
