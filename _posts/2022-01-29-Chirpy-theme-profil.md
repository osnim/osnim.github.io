---
title: Chirpy jekyll 테마에서 프로필 변경하기
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-01-29 00:10:00 +0900
categories: [jekyll]
tags: [blog, jekyll theme, Chirpy, profil]
---

# 프로필 변경 및 사진 업로드

먼저 로컬에 이미지만을 저장할 새로운 폴더를 만들고 그 폴더를 자신의 GitHub에 푸시합니다. 여기서 주의할 점은 `<자신의 github이름>.github.io`안이 아닌 밖에 리포지토리를 생성합니다.

git push 하는 법은 이전에도 많이 사용하였으니 생략하겠습니다.

**\_config.yml** 파일에서 `img_cdn` 은 images 파일의 끝을 담당하고 있습니다.

`avatar` 부분 프로필을 담당하고 있습니다.

### img_cdn

1. https://cdn.jsdelivr.net/gh/:

   CDN 서버를 활용해서 js파일들을 전송하는 것인데 github 리포지토리에 있는 사진을 서비스 합니다.
   gh는 github의 약자입니다. 그대로 냅둡니다.
   자세한 사항은 [jsdelivr](https://www.jsdelivr.com/?docs=gh)에서 확인하면 됩니다.

2. cotes2020/
   여기는 자신의 GitHub 이름을 입력합니다.

3. chirpy-images
   사진들이 있는 리포지토리 이름입니다. `<자신의 github이름>.github.io`안이 아닌 밖에 리포지토리를 생성합니다.

4. @f4e0354b674f65a53b8917f0f786ed2956898cc1'
   리포지토리의 버전을 의미합니다.

   버전을 찾는 방법은 다음과 같습니다. 저는 Chripy GitHub를 예로 들겠습니다.

   Chripy jekyll 테마의 제작자의 GitHub의 [chirpy-images](https://github.com/cotes2020/chirpy-images)로 이동합니다.

   ![/20220129/profil_version1.png](/20220129/profil_version1.png){: width="972" height="589" style="max-width: 70%"}

   위 사진 순서대로 commons 커밋 메세지를 클릭합니다 그러면 위의 commit 옆에 버전이 나타나는데 이를 복사합니다.

   이 버전을 @ 옆에 붙여넣기 합니다.

### avatar

프로필로 저장하고 싶은 사진이 있는 경로의 주소를 작성합니다. 위에서 생성한 이미지 전용 폴더 안에 있어야 합니다.

### 사진 업로드가 실시간으로 입력이 안되는 경우

Chripy 테마의 프로필과 사진 업로드는 조금 특이한 방식으로 되어있습니다. 사진이 로컬에만 있으면 안되고
사진을 GitHub에 업로드하여 아바타나 이미지를 블로그에 반영 할 수 있습니다. 이전의 커밋버전과 현재의 버전이 안맞으면 이미지 로드가 잘 안되는 경우가 있습니다.

이 경우 이미지를 모두 GitHub에 업로드 한 후, 바뀐 버전을 **\_config.yml** 파일에서 `img_cdn` 부분의 버전을 수정하여 로컬에서 작업해보시기 바랍니다. 그리고 이미지 폴더가 2개 이상인 경우 동시에 커밋을 해서 커밋 버전을 되도록 일치시키는 것이 좋습니다. 불가피 할 경우 최신의 버전을 `img_cdn` 버전에 입력하시면 될 것 같습니다.

버전은 콘솔 창에서도 `git log` 통해 확인이 가능합니다.

---

## 참조한 사이트

여기는 제가 참조한 사이트입니다.

<https://chirpy.cotes.page/>

<https://kkminseok.github.io/posts/startblog/>

<https://blog.kimzinu.com/posts/jekyll-4/>

<https://zeddios.tistory.com/1223?category=682196>
