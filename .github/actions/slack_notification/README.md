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
