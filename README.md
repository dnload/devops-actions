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

### SonarQube Analysis

SonarQube 정적 코드 분석을 실행하는 액션입니다.

**기능:**

- SonarQube 스캐너를 사용한 코드 분석
- C/C++ 프로젝트를 위한 빌드 래퍼 지원
- Rust 프로젝트를 위한 Clippy 분석 지원
- LLVM 커버리지 리포트 통합
- 소스 코드 및 커버리지 제외 패턴 설정

**사용 예시:**

```yaml
- uses: dnload/devops-actions/.github/actions/sonarqube-analysis@main
  with:
    sonar_token: ${{ secrets.SONAR_TOKEN }}
    sonar_host_url: "https://sonarqube.example.com"
    sonar_project_key: "my-project"
    sonar_project_name: "My Project"
    analysis_sources: "src"
    # C/C++ 프로젝트 옵션
    is_build_wrapper: "true"
    build_command: "make"
    # Rust 프로젝트 옵션
    is_rust_project: "true"
    clippy_report_path: "clippy-report.json"
    # 제외 패턴
    sonar_exclusions: "**/*.test.ts"
    sonar_coverage_exclusions: "**/*.spec.ts"
```

**주요 입력 변수:**

- `sonar_token`: SonarQube 인증 토큰
- `sonar_host_url`: SonarQube 서버 URL
- `sonar_project_key`: 프로젝트 키
- `sonar_project_name`: 프로젝트 이름
- `analysis_sources`: 분석할 소스 코드 경로
- `is_build_wrapper`: C/C++ 빌드 래퍼 사용 여부
- `is_rust_project`: Rust 프로젝트 여부
- `sonar_exclusions`: 분석에서 제외할 파일 패턴
- `sonar_coverage_exclusions`: 커버리지 분석에서 제외할 파일 패턴

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
