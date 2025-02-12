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
    # LLVM 커버리지 리포트 경로
    sonar_llvm_cov_report_path: "coverage.json"
    # SonarQube 퀄리티 게이트 옵션
    sonar_quality_gate: "true"
```

**필수 입력 변수:**

| 변수명               | 설명                  |
| -------------------- | --------------------- |
| `sonar_token`        | SonarQube 인증 토큰   |
| `sonar_host_url`     | SonarQube 서버 URL    |
| `sonar_project_key`  | 프로젝트 키           |
| `sonar_project_name` | 프로젝트 이름         |
| `analysis_sources`   | 분석할 소스 코드 경로 |

**선택적 입력 변수:**

| 변수명                       | 기본값               | 설명                                                                       |
| ---------------------------- | -------------------- | -------------------------------------------------------------------------- |
| `is_build_wrapper`           | "false"              | C/C++ 빌드 래퍼 사용 여부                                                  |
| `build_command`              | ""                   | 빌드 명령어 (ex: /usr/bin/python3 ./run_test.py --build --test --coverage) |
| `bw_output_path`             | "./bw-output"        | 빌드 래퍼 출력 경로                                                        |
| `is_rust_project`            | "false"              | Rust 프로젝트 여부                                                         |
| `clippy_report_path`         | "clippy-report.json" | Clippy 리포트 경로                                                         |
| `sonar_exclusions`           | ""                   | 분석에서 제외할 파일 패턴                                                  |
| `sonar_coverage_exclusions`  | ""                   | 커버리지 분석에서 제외할 파일 패턴                                         |
| `sonar_llvm_cov_report_path` | ""                   | LLVM 커버리지 리포트 경로                                                  |
| `sonar_quality_gate`         | "false"              | SonarQube 퀄리티 게이트 사용 여부                                          |
