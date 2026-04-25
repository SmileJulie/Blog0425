---
title: "GitHub Pages 블로그 만들기 3: GitHub Actions로 자동 배포하기"
date: 2026-04-28 09:00:00 +0900
categories:
  - blog
  - github-pages
tags:
  - github-actions
  - deployment
  - jekyll
---

# GitHub Pages 블로그 만들기 3: GitHub Actions로 자동 배포하기

이전 글에서 Jekyll과 `just-the-docs` 테마를 연결해 블로그의 기본 구조를 만들었습니다.

이번 글에서는 이 블로그를 GitHub Pages에 자동으로 배포하는 과정을 정리합니다.

## 왜 GitHub Actions로 배포하나

예전에는 GitHub Pages에서 브랜치나 폴더를 직접 선택해서 배포하는 방식이 더 익숙했습니다.

하지만 Jekyll 테마를 쓰고, 빌드 과정을 명확하게 관리하려면 GitHub Actions 방식이 훨씬 낫습니다.

이 방식의 장점은 아래와 같습니다.

- push만 하면 자동으로 빌드와 배포가 진행된다.
- 실패 원인을 로그로 확인할 수 있다.
- Ruby, Bundler, Jekyll 버전을 워크플로에서 관리할 수 있다.
- 나중에 CI 성격의 검증을 추가하기 쉽다.

## 배포 워크플로 파일 만들기

핵심은 저장소 루트의 `.github/workflows/pages.yml` 파일입니다.

이번 블로그에서는 아래 흐름으로 구성했습니다.

1. 저장소 checkout
2. Pages 설정 정보 읽기
3. Ruby 설치
4. Jekyll 빌드
5. 결과물을 Pages artifact로 업로드
6. GitHub Pages에 배포

워크플로 파일은 대략 아래 형태입니다.

```yml
name: Deploy Jekyll site to Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:
```

이 설정 덕분에 `main` 브랜치에 푸시할 때마다 자동 배포가 실행됩니다.

## GitHub Pages에서 꼭 맞춰야 하는 설정

워크플로 파일만 만든다고 배포가 끝나는 건 아닙니다.

GitHub 저장소의 아래 항목도 맞춰야 합니다.

- `Settings`
- `Pages`
- `Build and deployment`
- `Source: GitHub Actions`

이 설정이 빠져 있으면 워크플로가 돌아도 Pages 사이트를 찾지 못해서 실패할 수 있습니다.

## `_config.yml`에서 중요한 값

프로젝트 사이트 방식으로 배포할 때는 `_config.yml`의 `url`과 `baseurl`이 특히 중요합니다.

예를 들어 이번 설정은 아래와 같습니다.

```yml
url: https://username.github.io
baseurl: /tech-notes
```

이 두 값이 맞아야 정적 사이트 내부 링크가 올바르게 생성됩니다.

특히 프로젝트 사이트는 루트(`/`)가 아니라 저장소 경로(`/tech-notes`) 아래에 열리기 때문에 `baseurl`이 틀리면 링크가 깨질 가능성이 큽니다.

## 로컬 확인도 가능하다

배포 전에 로컬에서 미리 확인하는 것도 가능합니다.

대표적인 흐름은 아래와 같습니다.

```bash
bundle install
bundle exec jekyll serve
```

이렇게 하면 브라우저에서 변경 내용을 먼저 보고, 이상 없을 때 GitHub에 푸시할 수 있습니다.

## 실제 배포 흐름

정리하면 배포 과정은 꽤 단순합니다.

1. Markdown이나 설정 파일 수정
2. Git commit
3. `main` 브랜치로 push
4. GitHub Actions 실행
5. Pages 사이트 갱신

작성자는 글만 관리하고, 배포는 자동화된 흐름에 맡기는 구조입니다.

## 배포가 성공하면 확인할 것

배포가 끝난 뒤에는 아래를 확인해 보면 좋습니다.

- 홈 화면이 정상적으로 열리는지
- 메뉴 링크가 올바른지
- 글 목록에서 포스트가 보이는지
- 개별 포스트 URL이 정상인지
- 검색 기능이 동작하는지

겉으로 사이트는 열리더라도 내부 링크가 잘못된 경우가 있기 때문에 한 번은 직접 눌러 보는 편이 좋습니다.

## 이번 단계의 핵심

이번 단계에서 가장 중요한 포인트는 두 가지였습니다.

1. GitHub Pages는 `GitHub Actions` 소스로 설정해야 한다.
2. Jekyll 프로젝트 사이트에서는 `baseurl` 설정이 중요하다.

이 두 가지가 맞아야 자동 배포 흐름이 안정적으로 돌아갑니다.

## 마무리

이제 블로그는 단순한 정적 파일 묶음이 아니라, `push`만으로 자동 배포되는 형태가 되었습니다.

이후부터는 글을 쓰고 커밋하는 것만으로 사이트가 업데이트되기 때문에 운영 부담이 많이 줄어듭니다.

다음 글에서는 실제로 배포하면서 겪었던 실패 사례와, 어떤 식으로 원인을 찾아 해결했는지 정리해 보겠습니다.

---

이전 글: [GitHub Pages 블로그 만들기 2: Jekyll과 just-the-docs 테마 연결하기]({{ "/2026/04/27/github-pages-blog-02-jekyll-theme-setup/" | relative_url }})

다음 글: [GitHub Pages 블로그 만들기 4: 배포 오류 해결과 운영 시작]({{ "/2026/04/29/github-pages-blog-04-troubleshooting/" | relative_url }})
