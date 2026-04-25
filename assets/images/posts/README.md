# Post Images

블로그 글에 들어가는 이미지는 이 폴더 아래에 저장합니다.

권장 방식:

1. 글 하나당 하위 폴더 하나를 만듭니다.
2. 폴더 이름은 글 날짜와 주제를 같이 적습니다.
3. Markdown에서는 `relative_url` 필터를 사용해 경로를 연결합니다.

예시 폴더:

- `assets/images/posts/2026-04-24-jekyll-guide/`
- `assets/images/posts/2026-04-24-actions-deploy/`

예시 Markdown:

```md
![GitHub Actions 배포 흐름]({{ "/assets/images/posts/2026-04-24-actions-deploy/workflow.png" | relative_url }})
```
