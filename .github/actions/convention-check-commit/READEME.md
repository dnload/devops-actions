### Convention Check

커밋 메시지와 PR 제목의 컨벤션을 검사하는 액션입니다.

**기능:**

- 커밋 메시지 컨벤션 검사
- PR 제목 컨벤션 검사
- 커스텀 패턴 지원
- 예외 패턴 지원

**커밋 메시지 검사 사용 예시:**

```yaml
- uses: dnload/devops-actions/.github/actions/convention-check-commit@main
  with:
    commit_message_pattern: "^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .+"
    etc_pattern: "^(Merge|Release|Revert) .+"
    pull_request_url: ${{ github.event.pull_request.commits_url }}
    token: ${{ secrets.GITHUB_TOKEN }}
```

**PR 제목 검사 사용 예시:**

```yaml
- uses: dnload/devops-actions/.github/actions/convention-check-pr-title@main
  with:
    pr_title: ${{ github.event.pull_request.title }}
    pr_title_pattern: "^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .+"
    etc_pattern: "^(Merge|Release|Revert) .+"
```

**주요 입력 변수:**

- `commit_message_pattern`: 커밋 메시지 검사 패턴
- `pr_title_pattern`: PR 제목 검사 패턴
- `etc_pattern`: 예외 허용 패턴
- `pull_request_url`: PR의 커밋 목록 URL
- `token`: GitHub 토큰
- `pr_title`: PR 제목

**패턴 예시:**

- `feat: 새로운 기능 추가`
- `fix(auth): 로그인 버그 수정`
- `docs: README 업데이트`
- `Merge pull request #123`
