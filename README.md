# umc-product-android-config

[umc-product-android](https://github.com/UMC-PRODUCT/umc-product-android) 앱이 **원격으로 읽어가는 설정 저장소**입니다.

앱을 새로 배포하지 않고도 특정 화면에 안내 다이얼로그를 켜거나 끄고, 문구를 바꿀 수 있습니다.
Play 스토어 심사와 사용자 업데이트를 기다릴 필요가 없습니다.

> **현재 상태:** 앱 연동 작업은 아직 진행 중입니다. 이 저장소의 값을 바꿔도 지금은 앱 동작이 바뀌지 않습니다.

## 앱이 읽는 주소

```
https://umc-product.github.io/umc-product-android-config/app-config.json
```

GitHub Pages 로 서빙되며 캐시가 10분입니다. **머지 후 최대 10분 뒤에 앱에 반영**됩니다.

## 파일

| 파일 | 설명 |
|---|---|
| `app-config.json` | 실제 설정. 이것만 고치면 됩니다 |
| `schema.json` | 값의 규칙. 오타·잘못된 값을 걸러냅니다 |
| `.github/workflows/validate.yml` | PR 마다 위 규칙으로 검사 |

## 고치는 방법

1. `app-config.json` 을 열고 오른쪽 위 **연필 아이콘** 클릭
2. 값 수정
3. 아래에서 **Create a new branch for this commit and start a pull request** 선택 → **Propose changes**
4. `validate` 검사가 통과하면 머지
5. 10분 안에 앱에 반영

되돌리려면 그 PR 페이지의 **Revert** 버튼을 누르면 됩니다.

> 저장소 화면에서 키보드 `.` 을 누르면 브라우저에 편집기가 열립니다. 자동완성과 오류 표시가 있어 실수를 줄일 수 있습니다.

## 설정 값

```json
{
  "version": 1,
  "notices": [
    {
      "screen": "EmailSignUp",
      "enabled": false,
      "template": "INFO",
      "title": "인증 메일이 안 올 수 있어요",
      "body": "지금 이메일 발송량이 많아 인증 메일이 늦거나 도착하지 않을 수 있어요. 메일이 오지 않으면 다음 날 다시 시도해주세요.",
      "until": "2026-12-31"
    }
  ]
}
```

| 필드 | 필수 | 설명 |
|---|---|---|
| `screen` | O | 어느 화면에 띄울지 |
| `enabled` | O | `true` 면 노출, `false` 면 숨김. **평소에는 `false` 로 두고 필요할 때만 켭니다** |
| `template` | O | 다이얼로그 모양 |
| `title` | O | 제목 (40자 이내) |
| `body` | O | 본문 (200자 이내) |
| `until` | X | 이 날짜가 지나면 자동으로 안 뜹니다 (`YYYY-MM-DD`) |

### `screen` 에 쓸 수 있는 값

앱의 화면 경로 이름(`MainDestination`)을 그대로 씁니다. 대소문자까지 똑같이 적어야 합니다.

| 구분 | 값 | 화면 |
|---|---|---|
| 시작·인증 | `Login` | 로그인 (카카오·구글·UMC 계정 선택) |
| | `EmailLogin` | UMC 계정 로그인 |
| | `FindPassword` | 비밀번호 찾기 |
| | `EmailSignUp` | 이메일 회원가입 (이메일 인증) |
| | `SocialSignUp` | 소셜 회원가입 (이메일 인증) |
| | `SignUp` | 개인정보 입력 (가입 마지막 단계) |
| | `Permission` | 권한 안내 |
| | `SignUpFail` | 챌린저 인증 실패 |
| | `SignUpFailCode` | 챌린저 코드 입력 |
| 홈 | `Home` | 홈 |
| | `Notification` | 알림 |
| | `ScheduleAdd` | 일정 생성 |
| | `ScheduleEdit` | 일정 수정 |
| | `ScheduleDetail` | 일정 상세 |
| 공지 | `Notice` | 공지 목록 |
| | `NoticeSearch` | 공지 검색 |
| | `NoticeDetail` | 공지 상세 |
| | `NoticeWrite` | 공지 작성·수정 |
| | `AdminNotice` | 운영진 공지 |
| 활동 | `Act` | 활동 |
| | `AdminChallengerDetail` | 챌린저 상세 (운영진) |
| | `AdminStudyGroupCreate` | 스터디 그룹 생성 (운영진) |
| | `AdminStudyGroupSchedule` | 스터디 그룹 일정 (운영진) |
| 커뮤니티 | `Community` | 커뮤니티 |
| | `CommunitySearch` | 커뮤니티 검색 |
| | `CommunityCreate` | 글 작성 |
| | `CommunityEdit` | 글 수정 |
| | `CommunityChatting` | 스레드 상세 |
| MY | `Mycard` | 내 명함 |
| | `Mypage` | 설정 |
| | `MyProfile` | 프로필 |
| | `MyContent` | 내가 쓴 글·댓글·스크랩 |
| | `Qrcode` | 내 QR 코드 |
| | `ReceivedCard` | 받은 명함 |

- `Splash`(앱 시작 화면)는 금방 지나가는 화면이라 쓸 수 없습니다
- 공지 상세·스레드 상세처럼 대상이 여러 개인 화면은 **모든 공지·모든 스레드에** 똑같이 뜹니다

### `template` 에 쓸 수 있는 값

| 값 | 모양 |
|---|---|
| `INFO` | 제목 + 본문 + 확인 버튼 |
| `CONFIRM` | 버튼 2개 (확인 / 취소) |
| `BLOCKING` | 전체 화면. 점검 안내처럼 진행을 막아야 할 때 |

## 주의

- **비밀값을 넣지 마세요.** 공개 저장소라 누구나 읽을 수 있습니다. 토큰·비밀번호·내부 주소는 금지입니다
- 위 표에 없는 값을 쓰면 `validate` 검사에서 막힙니다
- `main` 은 보호돼 있어 직접 커밋할 수 없습니다. 항상 PR 로 올라갑니다
