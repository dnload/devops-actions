### PR Auto Labeler

PR 제목을 기반으로 자동으로 라벨을 추가하는 액션입니다.

**기능:**

- PR 제목에서 타입 추출
- 패턴 매칭을 통한 라벨 자동 지정
- GitHub 라벨 자동 추가

**사용 예시:**

```yaml
- uses: dnload/devops-actions/.github/actions/pr-auto-labeler@main
  with:
    pr_title: ${{ github.event.pull_request.title }}
    commit_message_pattern: "^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .+"
    token: ${{ secrets.GITHUB_TOKEN }}
```

**패턴 매칭:**

- PR 제목이 지정된 패턴과 일치하면 해당 타입을 라벨로 추가
- 기본 패턴은 Conventional Commits 형식을 따름
- 예: "feat: 새로운 기능 추가" → "feat" 라벨 추가
