# Vibe 코딩 V1.1 궁극 가이드
**저자:** [Nicolas Zullo, https://x.com/NicolasZu](https://x.com/NicolasZu)
**작성일:** 2025년 3월 12일
**최종 업데이트:** 2025년 4월 24일

---

## 시작하기

Vibe 코딩을 시작하려면 아래 두 가지 도구만 있으면 됩니다:

* **Gemini 2.5 Pro Thinking**
* **Cursor with Claude Sonnet 3.7 Thinking**

*(참고: 이전 버전 가이드에서는 Grok 3를 사용했으나, 현재는 **Gemini 2.5 Pro**로 전환했습니다. 주요 이유는 1M 토큰 컨텍스트 윈도우를 지원하여 프로젝트 이해 범위를 넓힐 수 있고, 복잡한 소프트웨어 아키텍처 처리 능력이 뛰어나기 때문입니다.)*

모든 설정을 올바르게 하는 것이 핵심입니다. 제대로 설정하지 않으면 AI가 자동으로 계획을 세워 코드베이스가 관리 불가능한 난장판이 될 수 있으니 주의하세요.

**핵심 원칙:** *계획이 전부입니다.* AI가 자율적으로 계획을 세우지 못하도록 관리하세요.

---

## 환경 설정

### 1. 게임 디자인 문서 작성

* 먼저 게임 아이디어를 Gemini 2.5 Pro Thinking에 전달하여 간단한 \*\*게임 디자인 문서(Game Design Document)\*\*를 Markdown(`.md`) 형식으로 생성하세요.
* 생성된 문서를 검토 및 보완하여 비전과 일치하도록 다듬습니다. 기본적인 수준이어도 괜찮습니다. 목표는 AI에게 프로젝트 구조와 목적에 대한 맥락을 제공하는 것입니다.

### 2. 기술 스택 및 `.cursor/rules`

* Gemini 2.5 Pro Thinking에 가장 적합한 기술 스택(예: 멀티플레이 3D 게임에는 ThreeJS와 WebSocket)을 추천하도록 요청하고, `tech-stack.md`로 저장하세요.

  * *가장 간단하면서도 견고한 스택*을 제안하도록 도전하세요.
* Cursor에서 명령 팔레트(`Cmd + Shift + P`)를 열어 "Configure Rules for '.cursor'"를 선택합니다.
* `/Generate Cursor Rules` 명령을 실행하면 프로젝트의 모든 `.md` 파일을 기준으로 우수한 규칙이 생성됩니다.
* **생성된 규칙을 반드시 검토하세요.**

  * 규칙은 \*\*모듈화(Multiple files)\*\*를 강조하고 \*\*모놀리식(Monolith)\*\*을 피하도록 설정해야 합니다.
  * 중요한 규칙은 "Always"로 표시하여 Cursor가 항상 참조하도록 만드세요. 예시:

    ```
    # IMPORTANT:
    # Always read memory-bank/@architecture.md before writing any code. Include entire database schema.
    # Always read memory-bank/@game-design-document.md before writing any code.
    # After adding a major feature or completing a milestone, update memory-bank/@architecture.md.
    ```
  * 비(非)Always 규칙은 네트워킹, 상태 관리 등 스택별 모범 사례를 유도하도록 설정하세요.
* *이 규칙 설정은 최적화된 코드와 깔끔한 구조 유지를 위해 필수입니다.*

### 3. 구현 계획 수립

* 아래 자료를 Gemini 2.5 Pro Thinking에 제공하고, 단계별 \*\*구현 계획(Implementation Plan)\*\*을 Markdown(`.md`) 형식으로 작성하도록 요청합니다:

  * 게임 디자인 문서 (`game-design-document.md`)
  * 기술 스택 문서 (`tech-stack.md`)
  * 설정한 Cursor 규칙 (`.cursor/rules`)
* 구현 계획에는 다음이 포함되어야 합니다:

  * 구체적이고 작은 단위의 단계별 지침
  * 각 단계별로 올바른 구현 확인을 위한 테스트 항목
  * 코드 없이 명확하고 구체적인 설명
  * *초기에는 기본 게임(Base Game)에 집중합니다.*

### 4. 메모리 뱅크 구성

* 새 프로젝트 폴더를 생성하고 Cursor에서 엽니다.
* 프로젝트 폴더 내에 `memory-bank` 하위 폴더를 만듭니다.
* `memory-bank` 폴더에 다음 파일을 추가하세요:

  * `game-design-document.md`
  * `tech-stack.md`
  * `implementation-plan.md`
  * `progress.md` (완료된 단계 기록용으로 빈 파일 생성)
  * `architecture.md` (파일 용도 문서화용으로 빈 파일 생성)
* *Cursor 규칙 저장 시, 루트 디렉터리에 `.cursor/rules` 파일이 자동 생성됩니다.*

---

## 기본 게임 개발

### 모든 사항이 명확한지 확인

* Cursor에서 **Claude Sonnet 3.7 Thinking** 선택
* 프롬프트: `/memory-bank` 폴더 내 문서를 모두 읽고 `implementation-plan.md`가 명확한지 알려주세요.
* 보완해야 할 질문 9\~10개를 받고, 답변 후 `implementation-plan.md`를 개선하도록 요청합니다.

### 첫 구현 프롬프트

* Cursor에서 **Claude Sonnet 3.7 Thinking** 선택

* 프롬프트: `/memory-bank` 폴더 내 문서를 모두 읽고 구현 계획 1단계를 실행하세요. 테스트를 실행할 것이고, 검증 완료 전에는 2단계를 시작하지 마세요.

* 테스트 검증 후, `progress.md`에 수행 내용을 기록하고 `architecture.md`에 파일별 아키텍처 설명을 추가하도록 합니다.

* **Extreme vibe:** Superwhisper([https://superwhisper.com)를](https://superwhisper.com%29를) 설치하여 타이핑 대신 음성으로 Claude와 소통할 수도 있습니다.

### 워크플로우

* 1단계 완료 후:

  * Git에 커밋(필요 시 Gemini 2.5에 도움 요청)
  * 새 Composer 열기(`Cmd + N`, `Cmd + I`)
  * 프롬프트: 메모리뱅크 파일과 `progress.md`를 읽고 2단계를 진행하세요. 테스트 검증 전까지 3단계로 넘어가지 않습니다.
* 위 과정을 전체 구현 계획이 완료될 때까지 반복합니다.

---

## 세부 기능 추가

기본 게임 개발이 완료되면 다양한 기능을 실험하고 다듬어 보세요:

* 안개, 후처리, 효과, 사운드 등
* 더 나은 배경 혹은 오브젝트 디자인
* 각 주요 기능마다 `feature-implementation.md` 파일을 만들어 짧은 단계와 테스트를 기록하세요.

---

## 버그 수정 및 문제 해결

* 프롬프트 실패 시:

  * Cursor에서 "restore" 클릭 후 프롬프트 개선
* 오류 발생 시:

  * **자바스크립트 콘솔:** `F12` 열어 오류 복사 후 Cursor에 붙여넣기
  * **시각적 오류:** 스크린샷 제공
  * **간편 옵션:** BrowserTools([https://browsertools.agentdesk.ai/installation](https://browsertools.agentdesk.ai/installation)) 설치
* 정말 막혔을 때:

  * 마지막 Git 커밋으로 리셋(`git reset`) 후 재시도
  * RepoPrompt([https://repoprompt.com/)나](https://repoprompt.com/%29나) uithub([https://uithub.com/)로](https://uithub.com/%29로) 전체 코드베이스를 하나 파일로 받아 Gemini 2.5에 도움 요청

---

## 기타 팁

* **소규모 편집:** Claude Sonnet 3.5 또는 GPT-4.1
* **마케팅 카피라이팅:** GPT-4.5
* **스프라이트 생성(2D 이미지):** ChatGPT-4o
* **음악 생성:** Suno
* **사운드 효과 생성:** ElevenLabs
* **영상 생성:** Sora
* **더 나은 프롬프트:** "think as long as needed to get this right..." 추가

---

## 자주 묻는 질문

**Q: 앱을 만드는 경우에도 같은 워크플로우를 써야 하나요?**
**A:** 대부분 동일합니다. GDD 대신 PRD(Product Requirements Document)를 작성하세요. v0, Lovable, Bolt.new 같은 툴로 프로토타입 후 Cursor에서 이어가도 좋습니다.

**Q: 당신의 도그파이트 게임 비행기가 멋진데, 하나의 프롬프트로 복제할 수 없어요!**
**A:** 한 프롬프트가 아니라 약 30개의 프롬프트와 `plane-implementation.md` 파일로 단계별 구현합니다.

**Q: 멀티플레이 게임 서버 설정 방법을 모르겠어요**
**A:** Gemini 2.5 Pro나 ChatGPT-4o에 물어보세요.

