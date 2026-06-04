# 워크플로우 계획

**목적**: 실행할 단계를 결정하고 종합적인 실행 계획을 수립합니다.

**항상 실행**: 이 단계는 항상 요구 사항과 범위를 이해한 후 실행됩니다.

## 1단계: 모든 이전 컨텍스트 로드

### 1.1 리버스 엔지니어링 아티팩트 로드(브라운필드인 경우)
- 건축.md
- 구성요소-inventory.md
- 기술 스택.md
- 의존성.md

### 1.2 부하 요구 사항 분석
- 요구사항.md(의도 분석 포함)
- 요구사항-검증-질문.md(답변 포함)

### 1.3 사용자 스토리 로드(실행한 경우)
- Stories.md
- 페르소나.md

## 2단계: 세부 범위 및 영향 분석

**이제 완전한 컨텍스트(요구 사항 + 스토리)가 있으므로 자세한 분석을 수행합니다.**

### 2.1 변환 범위 감지(브라운필드에만 해당)

**브라운필드 프로젝트**인 경우 변환 범위를 분석합니다.

#### 건축적 변화
- **단일 구성요소 변경** 및 **아키텍처 변환**
- **인프라 변경** 및 **애플리케이션 변경**
- **배포 모델 변경** (Lambda→Container, EC2→Serverless 등)

#### 관련 구성 요소 식별
변환의 경우 다음을 식별하십시오.
- 업데이트가 필요한 **인프라 코드**
- 변경이 필요한 **CDK 스택**
- **API 게이트웨이** 구성
- **로드 밸런서** 요구 사항
- **네트워킹** 변경 필요
- **모니터링/로깅** 적응

#### 패키지 간 영향
- 업데이트가 필요한 **CDK 인프라** 패키지
- **공유 모델** 버전 업데이트가 필요함
- **클라이언트 라이브러리** 엔드포인트 변경이 필요함
- 새로운 테스트 시나리오가 필요한 **테스트 패키지**

### 2.2 변경 영향 평가

#### 영향 영역
1. **사용자 대상 변경**: 이것이 사용자 경험에 영향을 미치나요?
2. **구조적 변경**: 시스템 아키텍처가 변경되나요?
3. **데이터 모델 변경**: 데이터베이스 스키마나 데이터 구조에 영향을 미치나요?
4. **API 변경**: 인터페이스나 계약에 영향을 미치나요?
5. **NFR 영향**: 성능, 보안 또는 확장성에 영향을 줍니까?

#### 애플리케이션 계층 영향(해당하는 경우)
- **코드 변경**: 새로운 진입점, 어댑터, 구성
- **종속성**: 새로운 라이브러리, 프레임워크 변경
- **구성**: 환경 변수, 구성 파일
- **테스팅**: 단위 테스트, 통합 테스트

#### 인프라 계층 영향(해당하는 경우)
- **배포 모델**: Lambda→ECS, EC2→Fargate 등
- **네트워킹**: VPC, 보안 그룹, 로드 밸런서
- **스토리지**: 영구 볼륨, 공유 스토리지
- **확장**: 자동 확장 정책, 용량 계획

#### 운영 계층 영향(해당되는 경우)
- **모니터링**: CloudWatch, 사용자 지정 지표, 대시보드
- **로깅**: 로그 집계, 구조화된 로깅
- **경고**: 알람 구성, 알림 채널
- **배포**: CI/CD 파이프라인 변경, 롤백 전략

### 2.3 구성요소 관계 매핑(브라운필드에만 해당)

**브라운필드 프로젝트인 경우** 구성요소 종속성 그래프를 만듭니다.

```markdown
## Component Relationships
- **Primary Component**: [Package being changed]
- **Infrastructure Components**: [CDK/Terraform packages]
- **Shared Components**: [Models, utilities, clients]
- **Dependent Components**: [Services that call this component]
- **Supporting Components**: [Monitoring, logging, deployment]
```

각 관련 구성 요소에 대해 다음을 수행합니다.
- **변경 유형**: 주요, 사소한, 구성 전용
- **변경 이유**: 직접 종속성, 배포 모델, 네트워킹
- **우선순위 변경**: 중요, 중요, 선택 사항

### 2.4 위험 평가

위험 수준 평가:
1. **낮음**: 격리된 변경, 쉬운 롤백, 잘 이해됨
2. **중간**: 여러 구성 요소, 중간 정도의 롤백, 일부 알 수 없는 사항
3. **높음**: 시스템 전체에 미치는 영향, 복잡한 롤백, 상당한 미지의 사항
4. **중요**: 생산에 매우 중요하고 롤백이 어렵고 불확실성이 높음

## 3단계: 위상 결정

### 3.1 사용자 스토리 - 이미 실행되었거나 건너뛰었나요?
**이미 실행됨**: 다음 결정으로 이동
**실행되지 않음 - IF 실행**:
- 여러 사용자 페르소나
- 사용자 경험에 미치는 영향
- 허용 기준이 필요함
- 팀 협업이 필요함

**다음인 경우 건너뛰기**:
- 내부 리팩토링
- 선명한 재현으로 버그 수정
- 기술 부채 감소
- 인프라 변화

### 3.2 애플리케이션 설계 - IF 실행:
- 새로운 구성 요소 또는 서비스가 필요함
- 구성 요소 방법 및 비즈니스 규칙에 대한 정의가 필요합니다.
- 서비스 계층 설계 필요
- 구성 요소 종속성에 대한 설명이 필요합니다.

**다음인 경우 건너뛰기**:
- 기존 구성 요소 경계 내의 변경 사항
- 새로운 구성 요소나 방법이 없습니다.
- 순수한 구현 변경

### 3.3 단위 생성 - IF 실행:
- 새로운 데이터 모델 또는 스키마
- API 변경 또는 새로운 엔드포인트
- 복잡한 알고리즘 또는 비즈니스 로직
- 상태 관리 변경
- 여러 패키지를 변경해야 함
- 코드형 인프라 업데이트 필요

**다음인 경우 건너뛰기**:
- 간단한 논리 변경
- UI 전용 변경 사항
- 구성 업데이트
- 간단한 구현

### 3.4 NFR 구현 - IF 실행:
- 성능 요구 사항
- 보안 고려 사항
- 확장성 문제
- 모니터링/관찰 가능성 필요

**다음인 경우 건너뛰기**:
- 기존 NFR 설정으로 충분함
- 새로운 NFR 요구사항 없음
- NFR에 영향을 주지 않는 간단한 변경

## 4단계: 적응형 세부정보 기록

**적응형 깊이 설명은 [깊이 수준.md](../common/length-levels.md)를 참조하세요**

실행될 각 단계에 대해 다음을 수행합니다.
- 정의된 모든 아티팩트가 생성됩니다.
- 아티팩트 내의 세부 수준은 문제의 복잡성에 맞춰 조정됩니다.
- 모델은 문제 특성을 기반으로 적절한 세부 사항을 결정합니다.

## 5단계: 다중 모듈 조정 분석(브라운필드에만 해당)

**여러 모듈/패키지가 있는 브라운필드인 경우** 종속성을 분석하고 최적의 업데이트 전략을 결정합니다.

### 5.1 모듈 종속성 분석
- 빌드 시스템 종속성 및 종속성 매니페스트 검사
- 빌드 시간과 런타임 종속성 식별
- 모듈 간 API 계약 및 공유 인터페이스 매핑

### 5.2 업데이트 전략 결정
종속성 분석을 기반으로 다음을 결정합니다.
- **업데이트 순서**: 종속성으로 인해 먼저 업데이트해야 하는 모듈
- **병렬화 기회**: 동시에 업데이트할 수 있는 모듈
- **조정 요구사항**: 버전 호환성, API 계약, 배포 순서
- **테스트 전략**: 모듈별 테스트 접근 방식과 통합 테스트 접근 방식
- **롤백 전략**: 시퀀스 중간에 오류가 발생할 경우 복구 계획

### 5.3 문서 조정 계획
```markdown
## Module Update Strategy
- **Update Approach**: [Sequential/Parallel/Hybrid]
- **Critical Path**: [Modules that block other updates]
- **Coordination Points**: [Shared APIs, infrastructure, data contracts]
- **Testing Checkpoints**: [When to validate integration]
```

영향을 받는 각 모듈을 식별합니다.
- **업데이트 우선순위**: 먼저 업데이트해야 함과 나중에 업데이트할 수 있음
- **종속성 제약**: 무엇에 의존하는지, 무엇에 의존하는지
- **변경 범위**: 주요(중단), 사소한(호환), 패치(수정)

## 6단계: 워크플로 시각화 생성

다음을 보여주는 Mermaid 순서도를 만듭니다.
- 모든 단계가 순차적으로 진행됨
- 각 조건부 단계에 대한 EXECUTE 또는 SKIP 결정
- 각 단계 상태에 대한 적절한 스타일링

**스타일링 규칙**(플로우차트 뒤에 추가):
```
style WD fill:#4CAF50,stroke:#1B5E20,stroke-width:3px,color:#fff
style CG fill:#4CAF50,stroke:#1B5E20,stroke-width:3px,color:#fff
style BT fill:#4CAF50,stroke:#1B5E20,stroke-width:3px,color:#fff
style US fill:#BDBDBD,stroke:#424242,stroke-width:2px,stroke-dasharray: 5 5,color:#000
style Start fill:#CE93D8,stroke:#6A1B9A,stroke-width:3px,color:#000
style End fill:#CE93D8,stroke:#6A1B9A,stroke-width:3px,color:#000

linkStyle default stroke:#333,stroke-width:2px
```

**스타일 가이드라인**:
- 완료/항상 실행: `fill:#4CAF50,stroke:#1B5E20,stroke-width:3px,color:#fff` (흰색 텍스트가 있는 녹색 소재)
- 조건부 실행: `fill:#FFA726,stroke:#E65100,stroke-width:3px,stroke-dasharray: 5 5,color:#000`(검은색 텍스트가 있는 주황색 재질)
- 조건부 건너뛰기: `fill:#BDBDBD,stroke:#424242,stroke-width:2px,stroke-dasharray: 5 5,color:#000`(검은색 텍스트가 있는 회색 소재)
- 시작/종료: `fill:#CE93D8,stroke:#6A1B9A,stroke-width:3px,color:#000`(검은색 텍스트가 있는 보라색 소재)
- 단계 컨테이너: 더 밝은 재질 색상 사용(INCEPTION: #BBDEFB, CONSTRUCTION: #C8E6C9, OPERATIONS: #FFF59D)

## 7단계: 실행 계획 문서 작성

`aidlc-docs/inception/plans/execution-plan.md` 만들기:

```markdown
# Execution Plan

## Detailed Analysis Summary

### Transformation Scope (Brownfield Only)
- **Transformation Type**: [Single component/Architectural/Infrastructure]
- **Primary Changes**: [Description]
- **Related Components**: [List]

### Change Impact Assessment
- **User-facing changes**: [Yes/No - Description]
- **Structural changes**: [Yes/No - Description]
- **Data model changes**: [Yes/No - Description]
- **API changes**: [Yes/No - Description]
- **NFR impact**: [Yes/No - Description]

### Component Relationships (Brownfield Only)
[Component dependency graph]

### Risk Assessment
- **Risk Level**: [Low/Medium/High/Critical]
- **Rollback Complexity**: [Easy/Moderate/Difficult]
- **Testing Complexity**: [Simple/Moderate/Complex]

## Workflow Visualization

```mermaid
흐름도 TD
    Start(["사용자 요청"])
    
    하위 그래프 INCEPTION["🔵 INCEPTION PHASE"]
        WD["작업 공간 감지<br/><b>상태</b>"]
        RE["리버스 엔지니어링<br/><b>상태</b>"]
        RA["요구사항 분석<br/><b>상태</b>"]
        미국["사용자 스토리<br/><b>상태</b>"]
        WP["워크플로 계획<br/><b>상태</b>"]
        AD["애플리케이션 디자인<br/><b>상태</b>"]
        UG["단위 생성<br/>(계획 + 생성)<br/><b>상태</b>"]
    끝
    
    하위 그래프 건설["🟢 건설 단계"]
        FD["기능적 디자인<br/><b>상태</b>"]
        NFRA["NFR 요구사항<br/><b>상태</b>"]
        NFRD["NFR 디자인<br/><b>상태</b>"]
        ID["인프라 설계<br/><b>상태</b>"]
        CG["코드 생성<br/>(계획 + 생성)<br/><b>실행</b>"]
        BT["빌드 및 테스트<br/><b>실행</b>"]
    끝
    
    하위 그래프 작업["🟡 작업 단계"]
        OPS["작업<br/><b>자리표시자</b>"]
    끝
    
    시작 --> WD
    WD --> RA
    RA --> WP
    WP --> CG
    CG --> BT
    BT --> End(["완료"])
    
    %% STATUS를 COMPLETED, SKIP, EXECUTE로 적절하게 바꾸십시오.
    %% 상태에 따라 스타일 적용
```

**Note**: Replace STATUS placeholders with actual phase status (COMPLETED/SKIP/EXECUTE) and apply appropriate styling

## Phases to Execute

### 🔵 INCEPTION PHASE
- [x] Workspace Detection (COMPLETED)
- [x] Reverse Engineering (COMPLETED/SKIPPED)
- [x] Requirements Analysis (COMPLETED)
- [x] User Stories (COMPLETED/SKIPPED)
- [x] Execution Plan (IN PROGRESS)
- [ ] Application Design - [EXECUTE/SKIP]
  - **Rationale**: [Why executing or skipping]
- [ ] Units Generation - [EXECUTE/SKIP]
  - **Rationale**: [Why executing or skipping]

### 🟢 CONSTRUCTION PHASE
- [ ] Functional Design - [EXECUTE/SKIP]
  - **Rationale**: [Why executing or skipping]
- [ ] NFR Requirements - [EXECUTE/SKIP]
  - **Rationale**: [Why executing or skipping]
- [ ] NFR Design - [EXECUTE/SKIP]
  - **Rationale**: [Why executing or skipping]
- [ ] Infrastructure Design - [EXECUTE/SKIP]
  - **Rationale**: [Why executing or skipping]
- [ ] Code Generation - EXECUTE (ALWAYS)
  - **Rationale**: Implementation planning and code generation needed
- [ ] Build and Test - EXECUTE (ALWAYS)
  - **Rationale**: Build, test, and verification needed

### 🟡 OPERATIONS PHASE
- [ ] Operations - PLACEHOLDER
  - **Rationale**: Future deployment and monitoring workflows

## Package Change Sequence (Brownfield Only)
[If applicable, list package update sequence with dependencies]

## Estimated Timeline
- **Total Phases**: [Number]
- **Estimated Duration**: [Time estimate]

## Success Criteria
- **Primary Goal**: [Main objective]
- **Key Deliverables**: [List]
- **Quality Gates**: [List]

[IF brownfield]
- **Integration Testing**: All components working together
- **Operational Readiness**: Monitoring, logging, alerting working
```

## 8단계: 상태 추적 초기화

`aidlc-docs/aidlc-state.md` 업데이트:

```markdown
# AI-DLC State Tracking

## Project Information
- **Project Type**: [Greenfield/Brownfield]
- **Start Date**: [ISO timestamp]
- **Current Stage**: INCEPTION - Workflow Planning

## Execution Plan Summary
- **Total Stages**: [Number]
- **Stages to Execute**: [List]
- **Stages to Skip**: [List with reasons]

## Stage Progress

### 🔵 INCEPTION PHASE
- [x] Workspace Detection
- [x] Reverse Engineering (if applicable)
- [x] Requirements Analysis
- [x] User Stories (if applicable)
- [x] Workflow Planning
- [ ] Application Design - [EXECUTE/SKIP]
- [ ] Units Generation - [EXECUTE/SKIP]

### 🟢 CONSTRUCTION PHASE
- [ ] Functional Design - [EXECUTE/SKIP]
- [ ] NFR Requirements - [EXECUTE/SKIP]
- [ ] NFR Design - [EXECUTE/SKIP]
- [ ] Infrastructure Design - [EXECUTE/SKIP]
- [ ] Code Generation - EXECUTE
- [ ] Build and Test - EXECUTE

### 🟡 OPERATIONS PHASE
- [ ] Operations - PLACEHOLDER

## Current Status
- **Lifecycle Phase**: INCEPTION
- **Current Stage**: Workflow Planning Complete
- **Next Stage**: [Next stage to execute]
- **Status**: Ready to proceed
```

## 9단계: 사용자에게 계획 제시

```markdown
# 📋 Workflow Planning Complete

I've created a comprehensive execution plan based on:
- Your request: [Summary]
- Existing system: [Summary if brownfield]
- Requirements: [Summary if executed]
- User stories: [Summary if executed]

**Detailed Analysis**:
- Risk level: [Level]
- Impact: [Summary of key impacts]
- Components affected: [List]

**Recommended Execution Plan**:

I recommend executing [X] stages:

🔵 **INCEPTION PHASE:**
1. [Stage name] - *Rationale:* [Why executing]
2. [Stage name] - *Rationale:* [Why executing]
...

🟢 **CONSTRUCTION PHASE:**
3. [Stage name] - *Rationale:* [Why executing]
4. [Stage name] - *Rationale:* [Why executing]
...

I recommend skipping [Y] stages:

🔵 **INCEPTION PHASE:**
1. [Stage name] - *Rationale:* [Why skipping]
2. [Stage name] - *Rationale:* [Why skipping]
...

🟢 **CONSTRUCTION PHASE:**
3. [Stage name] - *Rationale:* [Why skipping]
4. [Stage name] - *Rationale:* [Why skipping]
...

[IF brownfield with multiple packages]
**Recommended Package Update Sequence**:
1. [Package] - [Reason]
2. [Package] - [Reason]
...

**Estimated Timeline**: [Duration]

> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the execution plan at: `aidlc-docs/inception/plans/execution-plan.md`

> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the execution plan if required
> [IF any stages are skipped:]
> 📝 **Add Skipped Stages** - Choose to include stages currently marked as SKIP
> ✅ **Approve & Continue** - Approve plan and proceed to **[Next Stage Name]**
```

## 10단계: 사용자 응답 처리

- **승인된 경우**: 실행 계획의 다음 단계로 진행합니다.
- **변경이 요청된 경우**: 실행 계획을 업데이트하고 다시 확인하세요.
- **사용자가 단계를 강제로 포함/제외하려는 경우**: 그에 따라 계획을 업데이트하세요.

## 11단계: 상호작용 기록

`aidlc-docs/audit.md`에 로그인하세요:

```markdown
## Workflow Planning - Approval
**Timestamp**: [ISO timestamp]
**AI Prompt**: "Ready to proceed with this plan?"
**User Response**: "[User's COMPLETE RAW response]"
**Status**: [Approved/Changes Requested]
**Context**: Workflow plan created with [X] stages to execute

---
```
