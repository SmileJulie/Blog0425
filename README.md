# SmileJulie Blog

이 저장소는 GitHub Pages와 Just the Docs 테마로 운영하는 블로그입니다.

## 로컬 실행

1. Ruby와 Bundler를 설치합니다.
2. 저장소 루트에서 `bundle install`을 실행합니다.
3. `bundle exec jekyll serve`로 미리보기를 실행합니다.
4. 브라우저에서 `http://localhost:4000/Blog0425/`를 엽니다.

## 배포

- `main` 브랜치에 푸시하면 `.github/workflows/pages.yml`이 GitHub Pages용 사이트를 빌드하고 배포합니다.
- GitHub 저장소의 Settings > Pages 에서 Source 를 GitHub Actions 로 설정해야 합니다.

## 주요 파일

- `_config.yml`: 사이트 기본 설정
- `index.md`: 홈
- `blog.md`: 글 목록 페이지
- `_posts/`: 블로그 글

## 이미지 관리

- 포스트 이미지는 `assets/images/posts/` 아래에 저장합니다.
- 여러 글에서 공통으로 쓰는 이미지는 `assets/images/common/`에 둡니다.
- 예시 경로: `assets/images/posts/2026-04-24-jekyll-guide/cover.png`
- Markdown 예시: `![설명]({{ "/assets/images/posts/2026-04-24-jekyll-guide/cover.png" | relative_url }})`
