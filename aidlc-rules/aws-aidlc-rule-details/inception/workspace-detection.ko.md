# 작업 공간 감지

**목적**: 작업공간 상태 확인 및 기존 AI-DLC 프로젝트 확인

## 1단계: 기존 AI-DLC 프로젝트 확인

`aidlc-docs/aidlc-state.md`이 있는지 확인하세요.
- **존재하는 경우**: 마지막 단계에서 재개(이전 단계의 컨텍스트 로드)
- **존재하지 않는 경우**: 새 프로젝트 평가를 계속합니다.

## 2단계: 작업공간에서 기존 코드 검색

**작업공간에 기존 코드가 있는지 확인:**
- 소스 코드 파일(.java, .py, .js, .ts, .jsx, .tsx, .kt, .kts, .scala, .groovy, .go, .rs, .rb, .php, .c, .h, .cpp, .hpp, .cc, .cs, .fs 등)에 대한 작업 공간 스캔
- 빌드 파일(pom.xml, package.json, build.gradle 등)을 확인하세요.
- 프로젝트 구조 지표 찾기
- 작업공간 루트 디렉토리 식별(aidlc-docs/ 아님)

**기록 결과:**
```markdown
## Workspace State
- **Existing Code**: [Yes/No]
- **Programming Languages**: [List if found]
- **Build System**: [Maven/Gradle/npm/etc. if found]
- **Project Structure**: [Monolith/Microservices/Library/Empty]
- **Workspace Root**: [Absolute path]
```

## 3단계: 다음 단계 결정

**작업공간이 비어 있는 경우(기존 코드 없음)**:
- 플래그 설정: `brownfield = false`
- 다음 단계: 요구사항 분석

**작업공간에 기존 코드가 있는 경우**:
- 플래그 설정: `brownfield = true`
- `aidlc-docs/inception/reverse-engineering/`에서 기존 리버스 엔지니어링 아티팩트를 확인하세요.
- **리버스 엔지니어링 아티팩트가 존재하는 경우**:
    - 아티팩트가 오래되었는지 확인합니다(아티팩트 타임스탬프를 코드베이스의 마지막 중요한 수정 사항과 비교).
    - **아티팩트가 최신인 경우**: 로드하고 요구 사항 분석으로 건너뜁니다.
    - **아티팩트가 오래된 경우**: 다음 단계는 리버스 엔지니어링입니다(아티팩트를 새로 고치려면 다시 실행).
    - **사용자가 명시적으로 재실행을 요청하는 경우**: 다음 단계는 부실 여부에 관계없이 리버스 엔지니어링입니다.
- **리버스 엔지니어링 아티팩트가 없는 경우**: 다음 단계는 리버스 엔지니어링입니다.

## 4단계: 초기 상태 파일 생성

`aidlc-docs/aidlc-state.md` 만들기:

```markdown
# AI-DLC State Tracking

## Project Information
- **Project Type**: [Greenfield/Brownfield]
- **Start Date**: [ISO timestamp]
- **Current Stage**: INCEPTION - Workspace Detection

## Workspace State
- **Existing Code**: [Yes/No]
- **Reverse Engineering Needed**: [Yes/No]
- **Workspace Root**: [Absolute path]

## Code Location Rules
- **Application Code**: Workspace root (NEVER in aidlc-docs/)
- **Documentation**: aidlc-docs/ only
- **Structure patterns**: See code-generation.md Critical Rules

## Stage Progress
[Will be populated as workflow progresses]
```

## 5단계: 완료 메시지 표시

**브라운필드 프로젝트의 경우:**
```markdown
# 🔍 Workspace Detection Complete

Workspace analysis findings:
• **Project Type**: Brownfield project
• [AI-generated summary of workspace findings in bullet points]
• **Next Step**: Proceeding to **Reverse Engineering** to analyze existing codebase...
```

**그린필드 프로젝트의 경우:**
```markdown
# 🔍 Workspace Detection Complete

Workspace analysis findings:
• **Project Type**: Greenfield project
• **Next Step**: Proceeding to **Requirements Analysis**...
```

## 6단계: 자동으로 진행

- **사용자 승인이 필요하지 않습니다** - 이는 정보 제공용일 뿐입니다.
- 자동으로 다음 단계로 진행합니다.
  - **브라운필드**: 리버스 엔지니어링(기존 아티팩트가 없는 경우) 또는 요구 사항 분석(아티팩트가 있는 경우)
  - **그린필드**: 요구사항 분석
