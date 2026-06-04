# 리버스 엔지니어링

**목적**: 기존 코드베이스를 분석하고 포괄적인 디자인 아티팩트 생성

**실행 시기**: Brownfield 프로젝트 감지(작업공간에서 기존 코드 발견)

**건너뛰기**: Greenfield 프로젝트(기존 코드 없음)

**재실행 동작**: 재실행은 작업 공간 감지.md에 의해 제어됩니다. 기존 리버스 엔지니어링 아티팩트가 발견되고 아직 최신 상태인 경우 해당 아티팩트가 로드되고 리버스 엔지니어링을 건너뜁니다. 아티팩트가 오래되었거나(코드베이스의 마지막 중요한 수정보다 오래됨) 사용자가 명시적으로 재실행을 요청하는 경우 리버스 엔지니어링이 다시 실행되어 아티팩트가 현재 코드 상태를 반영하는지 확인합니다.

## 1단계: 다중 패키지 검색

### 1.1 스캔 작업 공간
- 모든 패키지(언급된 패키지 제외)
- 구성 파일을 통한 패키지 관계
- 패키지 유형: 애플리케이션, CDK/인프라, 모델, 클라이언트, 테스트

### 1.2 비즈니스 컨텍스트 이해
- 시스템이 전반적으로 수행하고 있는 핵심사업
- 모든 패키지의 사업 개요
- 시스템에서 구현되는 비즈니스 트랜잭션 목록

### 1.3 인프라 검색
- CDK 패키지(CDK 종속성이 있는 package.json)
- Terraform(.tf 파일)
- CloudFormation(.yaml/.json 템플릿)
- 배포 스크립트

### 1.4 빌드 시스템 검색
- 빌드 시스템: 브라질, Maven, Gradle, npm
- 빌드 시스템 선언을 위한 구성 파일
- 패키지 간 종속성 구축

### 1.5 서비스 아키텍처 발견
- Lambda 함수(핸들러, 트리거)
- 컨테이너 서비스(Docker/ECS 구성)
- API 정의(Smithy 모델, OpenAPI 사양)
- 데이터 저장소(DynamoDB, S3 등)

### 1.6 코드 품질 분석
- 프로그래밍 언어 및 프레임워크
- 테스트 커버리지 지표
- 린팅 구성
- CI/CD 파이프라인

## 2단계: 비즈니스 개요 문서 생성

`aidlc-docs/inception/reverse-engineering/business-overview.md` 만들기:

```markdown
# Business Overview

## Business Context Diagram
[Mermaid diagram showing the Business Context]

## Business Description
- **Business Description**: [Overall Business description of what the system does]
- **Business Transactions**: [List of Business Transactions that the system implements and their descriptions]
- **Business Dictionary**: [Business dictionary terms that the system follows and their meaning]

## Component Level Business Descriptions
### [Package/Component Name]
- **Purpose**: [What it does from the business perspective]
- **Responsibilities**: [Key responsibilities]
```

## 3단계: 아키텍처 문서 생성

`aidlc-docs/inception/reverse-engineering/architecture.md` 만들기:

```markdown
# System Architecture

## System Overview
[High-level description of the system]

## Architecture Diagram
[Mermaid diagram showing all packages, services, data stores, relationships]

## Component Descriptions
### [Package/Component Name]
- **Purpose**: [What it does]
- **Responsibilities**: [Key responsibilities]
- **Dependencies**: [What it depends on]
- **Type**: [Application/Infrastructure/Model/Client/Test]

## Data Flow
[Mermaid sequence diagram of key workflows]

## Integration Points
- **External APIs**: [List with purposes]
- **Databases**: [List with purposes]
- **Third-party Services**: [List with purposes]

## Infrastructure Components
- **CDK Stacks**: [List with purposes]
- **Deployment Model**: [Description]
- **Networking**: [VPC, subnets, security groups]
```

## 4단계: 코드 구조 문서 생성

`aidlc-docs/inception/reverse-engineering/code-structure.md` 만들기:

```markdown
# Code Structure

## Build System
- **Type**: [Maven/Gradle/npm/Brazil]
- **Configuration**: [Key build files and settings]

## Key Classes/Modules
[Mermaid class diagram or module hierarchy]

### Existing Files Inventory
[List all source files with their purposes - these are candidates for modification in brownfield projects]

**Example format**:
- `[path/to/file]` - [Purpose/responsibility]

## Design Patterns
### [Pattern Name]
- **Location**: [Where used]
- **Purpose**: [Why used]
- **Implementation**: [How implemented]

## Critical Dependencies
### [Dependency Name]
- **Version**: [Version number]
- **Usage**: [How and where used]
- **Purpose**: [Why needed]
```

## 5단계: API 문서 생성

`aidlc-docs/inception/reverse-engineering/api-documentation.md` 만들기:

```markdown
# API Documentation

## REST APIs
### [Endpoint Name]
- **Method**: [GET/POST/PUT/DELETE]
- **Path**: [/api/path]
- **Purpose**: [What it does]
- **Request**: [Request format]
- **Response**: [Response format]

## Internal APIs
### [Interface/Class Name]
- **Methods**: [List with signatures]
- **Parameters**: [Parameter descriptions]
- **Return Types**: [Return type descriptions]

## Data Models
### [Model Name]
- **Fields**: [Field descriptions]
- **Relationships**: [Related models]
- **Validation**: [Validation rules]
```

## 6단계: 구성요소 인벤토리 생성

`aidlc-docs/inception/reverse-engineering/component-inventory.md` 만들기:

```markdown
# Component Inventory

## Application Packages
- [Package name] - [Purpose]

## Infrastructure Packages
- [Package name] - [CDK/Terraform] - [Purpose]

## Shared Packages
- [Package name] - [Models/Utilities/Clients] - [Purpose]

## Test Packages
- [Package name] - [Integration/Load/Unit] - [Purpose]

## Total Count
- **Total Packages**: [Number]
- **Application**: [Number]
- **Infrastructure**: [Number]
- **Shared**: [Number]
- **Test**: [Number]
```

## 7단계: 기술 스택 문서 생성

`aidlc-docs/inception/reverse-engineering/technology-stack.md` 만들기:

```markdown
# Technology Stack

## Programming Languages
- [Language] - [Version] - [Usage]

## Frameworks
- [Framework] - [Version] - [Purpose]

## Infrastructure
- [Service] - [Purpose]

## Build Tools
- [Tool] - [Version] - [Purpose]

## Testing Tools
- [Tool] - [Version] - [Purpose]
```

## 8단계: 종속성 문서 생성

`aidlc-docs/inception/reverse-engineering/dependencies.md` 만들기:

```markdown
# Dependencies

## Internal Dependencies
[Mermaid diagram showing package dependencies]

### [Package A] depends on [Package B]
- **Type**: [Compile/Runtime/Test]
- **Reason**: [Why dependency exists]

## External Dependencies
### [Dependency Name]
- **Version**: [Version]
- **Purpose**: [Why used]
- **License**: [License type]
```

## 9단계: 코드 품질 평가 생성

`aidlc-docs/inception/reverse-engineering/code-quality-assessment.md` 만들기:

```markdown
# Code Quality Assessment

## Test Coverage
- **Overall**: [Percentage or Good/Fair/Poor/None]
- **Unit Tests**: [Status]
- **Integration Tests**: [Status]

## Code Quality Indicators
- **Linting**: [Configured/Not configured]
- **Code Style**: [Consistent/Inconsistent]
- **Documentation**: [Good/Fair/Poor]

## Technical Debt
- [Issue description and location]

## Patterns and Anti-patterns
- **Good Patterns**: [List]
- **Anti-patterns**: [List with locations]
```

## 10단계: 타임스탬프 파일 생성

`aidlc-docs/inception/reverse-engineering/reverse-engineering-timestamp.md` 만들기:

```markdown
# Reverse Engineering Metadata

**Analysis Date**: [ISO timestamp]
**Analyzer**: AI-DLC
**Workspace**: [Workspace path]
**Total Files Analyzed**: [Number]

## Artifacts Generated
- [x] architecture.md
- [x] code-structure.md
- [x] api-documentation.md
- [x] component-inventory.md
- [x] technology-stack.md
- [x] dependencies.md
- [x] code-quality-assessment.md
```

## 11단계: 상태 추적 업데이트

`aidlc-docs/aidlc-state.md` 업데이트:

```markdown
## Reverse Engineering Status
- [x] Reverse Engineering - Completed on [timestamp]
- **Artifacts Location**: aidlc-docs/inception/reverse-engineering/
```

## 12단계: 사용자에게 완료 메시지 표시

```markdown
# 🔍 Reverse Engineering Complete

[AI-generated summary of key findings from analysis in the form of bullet points]

> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the reverse engineering artifacts at: `aidlc-docs/inception/reverse-engineering/`

> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the reverse engineering analysis if required
> ✅ **Approve & Continue** - Approve analysis and proceed to **Requirements Analysis**
```

## 13단계: 사용자 승인을 기다립니다.

- **필수**: 사용자가 명시적으로 승인할 때까지 진행하지 마세요.
- **필수**: 전체 원시 입력을 사용하여 audit.md에 사용자 응답을 기록합니다.
