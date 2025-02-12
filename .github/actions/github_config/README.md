### Github Config

GitHub 작업을 위한 기본 설정을 수행하는 액션입니다.

**기능:**

- Git 사용자 이메일 설정
- Git 사용자 이름 설정
- Git 자격 증명 설정

**사용 예시:**

```yaml
- uses: dnload/devops-actions/.github/actions/github_config@main
  with:
    email: "devops@example.com"
    name: "DevOps Bot"
    token: ${{ secrets.GITHUB_TOKEN }}
```
