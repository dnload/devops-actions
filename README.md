# devops-actions

이 저장소는 DevOps 워크플로우에서 자주 사용되는 GitHub Actions의 모음입니다.

## 사용 가능한 Actions

### Slack Notification

Slack 채널에 메시지를 보내는 액션입니다.

**기능:**

- 즉시 메시지 전송
- 예약 메시지 전송 지원
- 커스텀 봇 이름 설정

**사용 예시:**

```yaml
- uses: dnload/devops-actions/.github/actions/slack_notification@main
  with:
    slack_from: "Bot Name"
    slack_channel: "Channel Name"
    message: "배포가 완료되었습니다!"
    schedule_time: "08:00" # 선택사항
```

devops-message 서비스 자세한 사용 방법: https://message.rt.dnotitia.ai/docs

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

### Github API

GitHub API를 호출하기 위한 범용 액션입니다.

**기능:**

- REST API 엔드포인트 호출
- GET, POST, PATCH, DELETE 메소드 지원
- 응답 데이터 처리

**사용 예시:**

```yaml
- uses: dnload/devops-actions/.github/actions/github_api@main
  id: api_call
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    endpoint: "/issues/123"
    method: "PATCH"
    body: '{"state": "closed"}'

# API 응답 사용하기
- name: API 응답 처리
  run: |
    # 전체 응답 출력
    echo "${{ steps.api_call.outputs.body }}"

    # jq를 사용한 응답 파싱 예시
    echo "${{ steps.api_call.outputs.body }}" | jq -r '.state'
```

**응답 처리:**

- 모든 API 호출의 응답은 `steps.<step_id>.outputs.body`를 통해 접근 가능
- 응답은 JSON 형식으로 반환되며, jq를 사용하여 필요한 데이터 추출 가능
- `silent: "false"` 옵션을 사용하면 curl의 상세 출력 확인 가능

**응답 활용 예시:**

```yaml
# Issue 상태 확인
- name: Check issue state
  run: |
    ISSUE_STATE=$(echo "${{ steps.api_call.outputs.body }}" | jq -r '.state')
    echo "이슈 상태: $ISSUE_STATE"

# PR 리뷰어 목록 추출
- name: Get PR reviewers
  run: |
    REVIEWERS=$(echo "${{ steps.api_call.outputs.body }}" | jq -r '.requested_reviewers[].login')
    echo "리뷰어 목록: $REVIEWERS"

# 커밋 정보 파싱
- name: Parse commit info
  run: |
    COMMIT_SHA=$(echo "${{ steps.api_call.outputs.body }}" | jq -r '.sha')
    COMMIT_MESSAGE=$(echo "${{ steps.api_call.outputs.body }}" | jq -r '.commit.message')
    echo "커밋 SHA: $COMMIT_SHA"
    echo "커밋 메시지: $COMMIT_MESSAGE"
```

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
