---
title: "GitHub Pages 블로그 만들기 4: 배포 오류 해결과 운영 시작"
date: 2026-04-29 09:00:00 +0900
categories:
  - blog
  - github-pages
tags:
  - troubleshooting
  - github-actions
  - bundler
---

# GitHub Pages 블로그 만들기 4: 배포 오류 해결과 운영 시작

이전 글에서 GitHub Actions를 이용해 Jekyll 블로그를 자동 배포하는 설정까지 정리했습니다.

그런데 실제로는 설정을 맞췄다고 바로 끝나지 않았습니다.
이번 글에서는 배포 과정에서 만난 문제와 해결 과정을 정리합니다.

개인적으로는 이 과정이 가장 기록할 가치가 있다고 느꼈습니다.
성공한 설정만 남기는 것보다, 어디서 막혔고 왜 그랬는지를 남겨두는 편이 나중에 훨씬 도움이 되기 때문입니다.

## 첫 번째 문제: Pages가 활성화되지 않아서 실패

처음 워크플로를 실행했을 때 `build` 단계에서 바로 실패했습니다.

원인을 확인해 보니, 워크플로가 Pages 사이트 정보를 읽는 단계에서 저장소의 Pages 설정을 찾지 못하고 있었습니다.

즉, 워크플로 파일은 있었지만 저장소의 Pages 설정이 아직 `GitHub Actions`로 활성화되지 않은 상태였습니다.

해결 방법은 단순했습니다.

1. 저장소 `Settings`로 이동
2. `Pages` 메뉴 선택
3. `Source`를 `GitHub Actions`로 설정

이 설정을 맞춘 뒤에 다시 워크플로를 실행하니, 이전과는 다른 단계까지 진행됐습니다.

## 두 번째 문제: Bundler 플랫폼 오류

Pages 설정을 맞춘 뒤에는 배포가 다시 실패했습니다.

이번에는 `bundle install` 단계에서 에러가 났고, 로그를 보면 핵심 메시지는 아래와 비슷했습니다.

```text
Your bundle only supports platforms ["x64-mingw-ucrt"] but your local platform is x86_64-linux.
Add the current platform to the lockfile with `bundle lock --add-platform x86_64-linux`
```

이 문제는 로컬에서 Windows 환경으로 `bundle install`을 실행하면서 만들어진 `Gemfile.lock`이 Windows 플랫폼만 포함하고 있었기 때문에 생긴 것입니다.

그런데 GitHub Actions 러너는 Linux 환경에서 실행됩니다.
즉, 로컬 개발 환경과 CI 환경의 플랫폼이 달랐던 겁니다.

## 해결 방법: Linux 플랫폼 추가

이 문제는 안내 메시지 그대로 해결할 수 있었습니다.

```bash
bundle lock --add-platform x86_64-linux
```

이 명령을 실행한 뒤 `Gemfile.lock`을 다시 커밋하고 푸시하니, GitHub Actions에서 `bundle install`이 정상적으로 진행됐습니다.

이 경험을 통해 알게 된 건, Jekyll이나 Bundler 문제가 아니라 `lockfile`의 플랫폼 정보 문제였다는 점입니다.

## 로그를 볼 때 중요했던 포인트

문제가 생겼을 때 무조건 처음부터 모든 로그를 읽기보다, 아래 순서로 보는 게 효율적이었습니다.

1. 실패한 job이 무엇인지 확인
2. 실패한 step 이름 확인
3. 에러 메시지 마지막 몇 줄 읽기
4. 실제 실패 원인이 설정 문제인지 환경 문제인지 구분

이번 경우도 `build`가 실패했다는 것보다, 그 안의 `bundle install`에서 Linux 플랫폼 경고가 나왔다는 점이 핵심이었습니다.

## 성공 이후 확인한 것

문제를 해결한 뒤에는 아래를 순서대로 확인했습니다.

- Actions run 상태가 `Success`인지
- `build`, `deploy`가 모두 완료됐는지
- GitHub Pages 사이트 주소가 정상적으로 열리는지
- 홈, Blog, About 페이지 이동이 되는지

이 과정을 끝내고 나서야 비로소 “블로그가 배포됐다”고 볼 수 있었습니다.

## 이번 경험에서 얻은 체크리스트

앞으로 비슷한 설정을 할 때는 아래 순서로 점검하면 됩니다.

1. Pages 설정이 `GitHub Actions`인지 확인
2. `_config.yml`의 `url`, `baseurl` 확인
3. `Gemfile.lock`에 Linux 플랫폼이 포함되어 있는지 확인
4. Actions 로그에서 실패 step과 에러 메시지 확인
5. 배포 후 실제 URL 접속 확인

이 다섯 가지를 체크하면 대부분의 초기 배포 문제는 빠르게 정리할 수 있습니다.

## 운영 시작 전에 정리한 것

배포가 성공한 뒤에는 설정 작업보다 운영 구조가 더 중요해집니다.

앞으로는 아래 기준으로 글을 계속 쌓아가면 됩니다.

- `_posts`에 날짜 형식에 맞는 새 글 추가
- 카테고리와 태그를 일관되게 사용
- 홈과 About 페이지는 필요할 때만 수정
- 글을 쓰고 `main`에 푸시하면 자동 배포

즉, 이제부터는 구축 단계보다 운영 단계에 들어간 셈입니다.

## 마무리

이번 시리즈를 진행하면서 느낀 건, GitHub Pages 블로그는 생각보다 단순하지만 환경 차이에서 의외의 문제가 생길 수 있다는 점이었습니다.

그래도 한 번 구조를 잡아두면 이후에는 Markdown으로 글을 쓰고 push하는 것만으로 블로그를 계속 운영할 수 있습니다.

이제 기본 골격은 갖춰졌으니, 다음부터는 실제 글을 쌓아가면서 블로그를 점점 다듬어 가면 됩니다.

---

이전 글: [GitHub Pages 블로그 만들기 3: GitHub Actions로 자동 배포하기]({{ "/2026/04/28/github-pages-blog-03-actions-deploy/" | relative_url }})

시리즈 끝.