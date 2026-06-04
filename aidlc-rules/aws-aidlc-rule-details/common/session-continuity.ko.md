# 세션 연속성 템플릿

## 돌아오신 것을 환영합니다 프롬프트 템플릿
사용자가 기존 AI-DLC 프로젝트 작업을 계속하기 위해 돌아올 때 다음 메시지를 표시합니다.

```markdown
**Welcome back! I can see you have an existing AI-DLC project in progress.**

Based on your aidlc-state.md, here's your current status:
- **Project**: [project-name]
- **Current Phase**: [INCEPTION/CONSTRUCTION/OPERATIONS]
- **Current Stage**: [Stage Name]
- **Last Completed**: [Last completed step]
- **Next Step**: [Next step to work on]

**What would you like to work on today?**

A) Continue where you left off ([Next step description])
B) Review a previous stage ([Show available stages])

[Answer]: 
```

## 필수: 세션 연속성 지침
1. **기존 프로젝트를 감지할 때 항상aidlc-state.md를 먼저 읽으세요**
2. 워크플로 파일에서 **현재 상태를 구문 분석**하여 프롬프트를 채웁니다.
3. **필수: 이전 단계 아티팩트 로드** - 단계를 재개하기 전에 이전 단계의 모든 관련 아티팩트를 자동으로 읽습니다.
   - **리버스 엔지니어링**: Architecture.md, code-structure.md, api-documentation.md 읽기
   - **요구사항 분석**: 요구사항.md, 요구사항-검증-질문.md를 읽어보세요.
   - **사용자 스토리**: story.md, personas.md, story-generation-plan.md를 읽어보세요.
   - **애플리케이션 디자인**: 애플리케이션 디자인 아티팩트 읽기(comComponents.md, component-methods.md, services.md)
   - **디자인(단위)**: unit-of-work.md,unit-of-work-dependent.md,unit-of-work-story-map.md를 읽어보세요.
   - **유닛별 디자인**: 기능적 디자인.md, nfr-requirements.md, nfr-design.md, 인프라 디자인.md를 읽어보세요.
   - **코드 단계**: 모든 코드 파일, 계획 및 모든 이전 아티팩트를 읽습니다.
4. **단계별 스마트 컨텍스트 로딩**:
   - **초기 단계(작업 공간 감지, 리버스 엔지니어링)**: 로드 작업 공간 분석
   - **요구사항/스토리**: 리버스 엔지니어링 + 요구사항 아티팩트 로드
   - **디자인 단계**: 로드 요구 사항 + 스토리 + 아키텍처 + 디자인 아티팩트
   - **코드 단계**: 모든 아티팩트 + 기존 코드 파일 로드
5. **아키텍처 선택 및 현재 단계에 따라 옵션 조정**
6. **일반적인 설명보다는 구체적인 다음 단계 표시**
7. 타임스탬프와 함께 audit.md에 **연속성 프롬프트 기록**
8. **컨텍스트 요약**: 아티팩트를 로드한 후 사용자 인식을 위해 로드된 내용에 대한 간략한 요약을 제공합니다.
9. **질문하기**: 항상 설명이나 사용자 피드백 질문을 .md 파일에 배치하여 질문하세요. 채팅 세션에서 객관식 질문을 인라인으로 배치하지 마세요.

## 오류 처리
세션 재개 중에 아티팩트가 누락되거나 손상된 경우 복구 절차에 대한 지침은 [error-handling.md](error-handling.md)를 참조하세요.