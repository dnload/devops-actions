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
