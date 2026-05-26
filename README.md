# 실습 03 - 회의록 자동 생성기 (강의 결과물)

회의 녹음 파일(.wav, .m4a, .mp3 등)을 업로드하면 Groq Whisper로 받아쓰기하고, Gemini가 회의록을 정리해 주는 단일 HTML 도구예요.

## 사전 준비

- 브라우저: Chrome / Edge / Safari 최신판
- API 키 2종 (강의 11강에서 발급):
  - **Groq API 키** (https://console.groq.com - 음성 받아쓰기용)
  - **Google Gemini API 키** (https://aistudio.google.com/app/apikey - 회의록 정리용)
- Node.js 설치 불필요 (HTML 파일만 더블클릭)

## 사용 모델

- 음성 받아쓰기: `whisper-large-v3-turbo` (Groq)
- 회의록 정리: `gemini-3.1-flash-lite` (기본), 503 오류 시 `gemini-3-flash-preview`로 자동 폴백 가능

## 실행 방법

### 1순위: 더블클릭

1. `meeting_minutes.html` 더블클릭
2. 브라우저가 자동으로 열려요

### API 키 입력 (최초 1회)

1. 상단 STEP 1 영역에서 Groq API KEY 입력 후 저장
2. 같은 영역에서 GEMINI API KEY 입력 후 저장
3. localStorage에 안전 저장되어 다음부터는 자동 로드

`.env` 파일 만들 필요 없어요. 브라우저 안에서만 보관해요.

## 샘플 파일

폴더 안에 테스트용 샘플이 들어있어요.

- `샘플미팅.wav`, `샘플미팅2.m4a`: 회의 녹음 샘플 (각자 다른 길이)
- `미팅 요약1.pdf`: 결과 예시

먼저 샘플로 한 번 돌려보고 본인 회의 파일을 올려보세요.

## 트러블슈팅

### "Groq API 응답 없음" 또는 401 오류

1. Groq Console에서 API 키 다시 확인
2. 키가 만료됐는지 점검 (Groq는 무료 한도 내에서 동작)

### "Gemini 503" 오류

모델이 일시적으로 과부하인 경우예요. 화면 상단 GEMINI 모델 드롭다운에서 `gemini-3-flash-preview`로 변경 후 재시도하세요.

### "파일 크기가 너무 큽니다"

Groq Whisper API는 파일 크기 제한이 있어요 (약 25MB). 도구가 자동으로 분할 처리를 시도하지만, 1시간 넘는 회의는 미리 잘라서 올리는 게 안정적이에요.

### HTML이 더블클릭으로 안 열려요

파일 우클릭 > 연결 프로그램 > Chrome/Edge/Safari 선택

## 보안 주의

API 키는 비밀번호와 같아요.

- 노출되면 즉시 Groq Console / Google AI Studio에서 비활성화 후 재발급
- 이 도구는 키를 본인 브라우저 localStorage에만 저장합니다 (서버 전송 X)
- 회의 파일도 본인 브라우저에서 API로만 직접 전송되며, 라핀 서버를 거치지 않아요
