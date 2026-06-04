# 질문 형식 가이드

## 필수: 모든 질문은 이 형식을 사용해야 합니다.

### 규칙: 채팅에서는 절대 질문하지 마세요
**중요**: 절대 채팅에서 직접 질문을 해서는 안 됩니다. 모든 질문은 전용 질문 파일에 배치되어야 합니다.

### 질문 파일 형식

#### 파일 명명 규칙
- 설명이 포함된 이름을 사용하세요: `{phase-name}-questions.md`
- 예:
  - `classification-questions.md`
  - `requirements-questions.md`
  - `story-planning-questions.md`
  - `design-questions.md`

#### 질문 구조
모든 질문에는 의미 있는 옵션과 마지막 옵션으로 "기타"가 포함되어야 합니다.

```markdown
## Question [Number]
[Clear, specific question text]

A) [First meaningful option]
B) [Second meaningful option]
[...additional options as needed...]
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

**비판적인**:
- "기타"는 모든 질문의 마지막 옵션으로 필수입니다.
- 의미 있는 옵션만 포함하세요. 슬롯을 채우기 위한 옵션을 만들지 마세요.
- 가능한 한 많거나 적은 옵션을 사용하십시오(최소 2개 + 기타).

### 완전한 예

```markdown
# Requirements Clarification Questions

Please answer the following questions to help clarify the requirements.

## Question 1
What is the primary user authentication method?

A) Username and password
B) Social media login (Google, Facebook)
C) Single Sign-On (SSO)
D) Multi-factor authentication
E) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 2
Will this be a web or mobile application?

A) Web application
B) Mobile application
C) Both web and mobile
D) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 3
Is this a new project or existing codebase?

A) New project (greenfield)
B) Existing codebase (brownfield)
C) Other (please describe after [Answer]: tag below)

[Answer]: 
```

### 사용자 응답 형식
사용자는 [답변]: 태그: 뒤에 문자 선택을 입력하여 답변합니다.

```markdown
## Question 1
What is the primary user authentication method?

A) Username and password
B) Social media login (Google, Facebook)
C) Single Sign-On (SSO)
D) Multi-factor authentication

[Answer]: C
```

### 사용자 응답 읽기
사용자가 완료를 확인한 후:
1. 문제 파일 읽기
2. [답변] 뒤의 답변 추출: 태그
3. 모든 질문에 답변되었는지 확인하세요.
4. 답변을 바탕으로 분석 진행

### 객관식 지침

#### 옵션 개수
- 최소: 의미 있는 옵션 2개 + '기타'(A, B, C)
- 일반적: 의미 있는 옵션 3~4개 + '기타'(A, B, C, D, E)
- 최대: 의미 있는 옵션 5개 + '기타'(A, B, C, D, E, F)
- **중요**: 단지 슬롯을 채우기 위해 옵션을 만들지 마세요. 의미 있는 선택만 포함하세요.

#### 옵션 품질
- 옵션을 상호 배타적으로 설정
- 가장 일반적인 시나리오를 다룹니다.
- 의미 있고 현실적인 옵션만 포함하세요.
- **항상 마지막 옵션으로 "기타"를 포함하세요**(필수)
- 구체적이고 명확하게 작성하세요.
- **A, B, C, D 슬롯을 채우기 위한 옵션을 구성하지 마세요**

#### 좋은 예:
```markdown
## Question 5
What database technology will be used?

A) Relational (PostgreSQL, MySQL)
B) NoSQL Document (MongoDB, DynamoDB)
C) NoSQL Key-Value (Redis, Memcached)
D) Graph Database (Neo4j, Neptune)
E) Other (please describe after [Answer]: tag below)

[Answer]: 
```

#### 나쁜 예(피하세요):
```markdown
## Question 5
What database will you use?

A) Yes
B) No
C) Maybe

[Answer]: 
```

### 워크플로 통합

#### 1단계: 질문 파일 만들기
```markdown
Create aidlc-docs/{phase-name}-questions.md with all questions
```

#### 2단계: 사용자에게 알리기
```
"I've created {phase-name}-questions.md with [X] questions. 
Please answer each question by filling in the letter choice after the [Answer]: tag. 
If none of the options match your needs, choose the last option (Other) and describe your preference. Let me know when you're done."
```

#### 3단계: 확인 대기
사용자가 "완료", "완료", "완료" 등의 말을 기다립니다.

#### 4단계: 읽기 및 분석
```
Read aidlc-docs/{phase-name}-questions.md
Extract all answers
Validate completeness
Proceed with analysis
```

### 오류 처리

#### 누락된 답변
[답변]: 태그가 비어 있는 경우:
```
"I noticed Question [X] is not answered. Please provide an answer using one of the letter choices 
for all questions before proceeding."
```

#### 잘못된 답변
답변이 유효한 문자 선택이 아닌 경우:
```
"Question [X] has an invalid answer '[answer]'. 
Please use only the letter choices provided in the question."
```

#### 모호한 답변
사용자가 문자 대신 설명을 제공하는 경우:
```
"For Question [X], please provide the letter choice that best matches your answer. 
If none match, choose 'Other' and add your description after the [Answer]: tag."
```

### 모순 및 모호성 감지

**필수**: 사용자 응답을 읽은 후 모순과 모호성을 확인해야 합니다.

#### 모순 감지
논리적으로 일관되지 않은 답변을 찾으십시오.
- 범위 불일치: "버그 수정"이지만 "전체 코드베이스가 영향을 받음"
- 위험 불일치: '낮은 위험'이지만 '급격한 변경'
- 타임라인 불일치: "빠른 수정"이지만 "다중 하위 시스템"
- 영향 불일치: "단일 구성 요소"이지만 "중요한 아키텍처 변경"

#### 모호함 감지
불분명하거나 경계선에 있는 응답을 찾으십시오.
- 여러 분류에 적합한 답변
- 특이성이 부족한 반응
- 여러 질문에 걸쳐 지표가 충돌함

#### 명확한 질문 만들기
모순이나 모호함이 발견된 경우:

1. **설명 파일 만들기**: `{phase-name}-clarification-questions.md`
2. **문제 설명**: 어떤 모순/모호함이 발견되었는지 명확하게 설명합니다.
3. **타겟팅 질문**: 객관식 형식을 사용하여 문제 해결
4. **원래 질문 참조**: 답변이 상충되는 질문을 표시합니다.

**예**:
```markdown
# [Phase Name] Clarification Questions

I detected contradictions in your responses that need clarification:

## Contradiction 1: [Brief Description]
You indicated "[Answer A]" (Q[X]:[Letter]) but also "[Answer B]" (Q[Y]:[Letter]).
These responses are contradictory because [explanation].

### Clarification Question 1
[Specific question to resolve contradiction]

A) [Option that resolves toward first answer]
B) [Option that resolves toward second answer]
C) [Option that provides middle ground]
D) [Option that reframes the question]

[Answer]: 

## Ambiguity 1: [Brief Description]
Your response to Q[X] ("[Answer]") is ambiguous because [explanation].

### Clarification Question 2
[Specific question to clarify ambiguity]

A) [Clear option 1]
B) [Clear option 2]
C) [Clear option 3]
D) [Clear option 4]

[Answer]: 
```

#### 설명을 위한 작업흐름

1. **탐지**: 모순/모호성에 대한 모든 응답을 분석합니다.
2. **만들기**: 문제가 발견되면 설명 질문 파일을 생성합니다.
3. **알림**: 사용자에게 문제 및 설명 파일에 대해 알립니다.
4. **대기**: 사용자가 설명을 제공할 때까지 진행하지 마세요.
5. **재검증**: 명확히 한 후 일관성을 다시 확인합니다.
6. **진행**: 모든 모순이 해결된 후에만 앞으로 나아갑니다.

#### 사용자 메시지 예
```
"I detected 2 contradictions in your responses:

1. Bug fix scope vs. codebase impact (Q1 vs Q2)
2. Low risk vs. breaking changes (Q7 vs Q4)

I've created classification-clarification-questions.md with 2 questions to resolve these.
Please answer these clarifying questions before I can proceed with classification."
```

### 모범 사례

1. **구체적으로 작성하세요**: 질문은 명확하고 모호하지 않아야 합니다.
2. **포괄적이어야 함**: 필요한 모든 정보를 포함하십시오.
3. **간결하게 작성하세요**: 한 가지 주제에 집중하여 질문을 유지하세요.
4. **실용적이어야 합니다**: 옵션은 현실적이고 실행 가능해야 합니다.
5. **일관성을 유지하세요**: 모든 질문 파일에서 동일한 형식을 사용하세요.

### 단계별 예

#### 2개의 의미 있는 옵션이 있는 예:
```markdown
## Question 1
Is this a new project or existing codebase?

A) New project (greenfield)
B) Existing codebase (brownfield)
C) Other (please describe after [Answer]: tag below)

[Answer]: 
```

#### 3가지 의미 있는 옵션의 예:
```markdown
## Question 2
What is the deployment target?

A) Cloud (AWS, Azure, GCP)
B) On-premises servers
C) Hybrid (both cloud and on-premises)
D) Other (please describe after [Answer]: tag below)

[Answer]: 
```

#### 4가지 의미 있는 옵션의 예:
```markdown
## Question 3
What architectural pattern should be used?

A) Monolithic architecture
B) Microservices architecture
C) Serverless architecture
D) Event-driven architecture
E) Other (please describe after [Answer]: tag below)

[Answer]: 
```

## 요약

**기억하다**:
- ✅ 항상 질문 파일을 생성하세요
- ✅ 항상 객관식 형식을 사용하세요
- ✅ **항상 마지막 옵션으로 "기타"를 포함하세요(필수)**
- ✅ 의미 있는 옵션만 포함하세요. 슬롯을 채우기 위해 옵션을 만들지 마세요.
- ✅ 항상 [답변]을 사용하세요: 태그
- ✅ 항상 사용자 완료를 기다립니다
- ✅ 모순에 대한 응답을 항상 검증하세요
- ✅ 필요한 경우 항상 설명 파일을 만드세요.
- ✅ 진행하기 전에 항상 모순을 해결하세요
- ❌ 채팅에서는 절대 질문하지 마세요
- ❌ A, B, C, D만 갖기 위해 옵션을 만들지 마세요.
- ❌ 답변 없이 진행하지 마세요.
- ❌ 해결되지 않은 모순으로 진행하지 마십시오
- ❌ 모호한 응답에 대해 절대 가정하지 마세요.
