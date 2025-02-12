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
