# 빌드 및 테스트

**목적**: 모든 유닛을 구축하고 포괄적인 테스트 전략을 실행합니다.

## 전제조건
- 모든 장치에 대해 코드 생성이 완료되어야 합니다.
- 모든 코드 아티팩트를 생성해야 합니다.
- 프로젝트를 빌드하고 테스트할 준비가 되었습니다.

---

## 1단계: 테스트 요구 사항 분석

프로젝트를 분석하여 적절한 테스트 전략을 결정합니다.
- **단위 테스트**: 코드 생성 중에 단위별로 이미 생성되었습니다.
- **통합 테스트**: 단위/서비스 간의 상호 작용 테스트
- **성능 테스트**: 로드, 스트레스, 확장성 테스트
- **엔드 투 엔드 테스트**: 전체 사용자 워크플로
- **계약 테스트**: 서비스 간 API 계약 유효성 검사
- **보안 테스트**: 취약점 검색, 침투 테스트

---

## 2단계: 빌드 지침 생성

`aidlc-docs/construction/build-and-test/build-instructions.md` 만들기:

```markdown
# Build Instructions

## Prerequisites
- **Build Tool**: [Tool name and version]
- **Dependencies**: [List all required dependencies]
- **Environment Variables**: [List required env vars]
- **System Requirements**: [OS, memory, disk space]

## Build Steps

### 1. Install Dependencies
\`\`\`bash
[Command to install dependencies]
# Example: npm install, mvn dependency:resolve, pip install -r requirements.txt
\`\`\`

### 2. Configure Environment
\`\`\`bash
[Commands to set up environment]
# Example: export variables, configure credentials
\`\`\`

### 3. Build All Units
\`\`\`bash
[Command to build all units]
# Example: mvn clean install, npm run build, brazil-build
\`\`\`

### 4. Verify Build Success
- **Expected Output**: [Describe successful build output]
- **Build Artifacts**: [List generated artifacts and locations]
- **Common Warnings**: [Note any acceptable warnings]

## Troubleshooting

### Build Fails with Dependency Errors
- **Cause**: [Common causes]
- **Solution**: [Step-by-step fix]

### Build Fails with Compilation Errors
- **Cause**: [Common causes]
- **Solution**: [Step-by-step fix]
```

---

## 3단계: 단위 테스트 실행 지침 생성

`aidlc-docs/construction/build-and-test/unit-test-instructions.md` 만들기:

```markdown
# Unit Test Execution

## Run Unit Tests

### 1. Execute All Unit Tests
\`\`\`bash
[Command to run all unit tests]
# Example: mvn test, npm test, pytest tests/unit
\`\`\`

### 2. Review Test Results
- **Expected**: [X] tests pass, 0 failures
- **Test Coverage**: [Expected coverage percentage]
- **Test Report Location**: [Path to test reports]

### 3. Fix Failing Tests
If tests fail:
1. Review test output in [location]
2. Identify failing test cases
3. Fix code issues
4. Rerun tests until all pass
```

---

## 4단계: 통합 테스트 지침 생성

`aidlc-docs/construction/build-and-test/integration-test-instructions.md` 만들기:

```markdown
# Integration Test Instructions

## Purpose
Test interactions between units/services to ensure they work together correctly.

## Test Scenarios

### Scenario 1: [Unit A] → [Unit B] Integration
- **Description**: [What is being tested]
- **Setup**: [Required test environment setup]
- **Test Steps**: [Step-by-step test execution]
- **Expected Results**: [What should happen]
- **Cleanup**: [How to clean up after test]

### Scenario 2: [Unit B] → [Unit C] Integration
[Similar structure]

## Setup Integration Test Environment

### 1. Start Required Services
\`\`\`bash
[Commands to start services]
# Example: docker-compose up, start test database
\`\`\`

### 2. Configure Service Endpoints
\`\`\`bash
[Commands to configure endpoints]
# Example: export API_URL=http://localhost:8080
\`\`\`

## Run Integration Tests

### 1. Execute Integration Test Suite
\`\`\`bash
[Command to run integration tests]
# Example: mvn integration-test, npm run test:integration
\`\`\`

### 2. Verify Service Interactions
- **Test Scenarios**: [List key integration test scenarios]
- **Expected Results**: [Describe expected outcomes]
- **Logs Location**: [Where to check logs]

### 3. Cleanup
\`\`\`bash
[Commands to clean up test environment]
# Example: docker-compose down, stop test services
\`\`\`
```

---

## 5단계: 성능 테스트 지침 생성(해당되는 경우)

`aidlc-docs/construction/build-and-test/performance-test-instructions.md` 만들기:

```markdown
# Performance Test Instructions

## Purpose
Validate system performance under load to ensure it meets requirements.

## Performance Requirements
- **Response Time**: < [X]ms for [Y]% of requests
- **Throughput**: [X] requests/second
- **Concurrent Users**: Support [X] concurrent users
- **Error Rate**: < [X]%

## Setup Performance Test Environment

### 1. Prepare Test Environment
\`\`\`bash
[Commands to set up performance testing]
# Example: scale services, configure load balancers
\`\`\`

### 2. Configure Test Parameters
- **Test Duration**: [X] minutes
- **Ramp-up Time**: [X] seconds
- **Virtual Users**: [X] users

## Run Performance Tests

### 1. Execute Load Tests
\`\`\`bash
[Command to run load tests]
# Example: jmeter -n -t test.jmx, k6 run script.js
\`\`\`

### 2. Execute Stress Tests
\`\`\`bash
[Command to run stress tests]
# Example: gradually increase load until failure
\`\`\`

### 3. Analyze Performance Results
- **Response Time**: [Actual vs Expected]
- **Throughput**: [Actual vs Expected]
- **Error Rate**: [Actual vs Expected]
- **Bottlenecks**: [Identified bottlenecks]
- **Results Location**: [Path to performance reports]

## Performance Optimization

If performance doesn't meet requirements:
1. Identify bottlenecks from test results
2. Optimize code/queries/configurations
3. Rerun tests to validate improvements
```

---

## 6단계: 추가 테스트 지침 생성(필요에 따라)

프로젝트 요구 사항에 따라 추가 테스트 지침 파일을 생성합니다.

### 계약 테스트(마이크로서비스용)
`aidlc-docs/construction/build-and-test/contract-test-instructions.md` 만들기:
- 서비스 간 API 계약 검증
- 소비자 중심 계약 테스트
- 스키마 검증

### 보안 테스트
`aidlc-docs/construction/build-and-test/security-test-instructions.md` 만들기:
- 취약점 스캔
- 종속성 보안 검사
- 인증/권한 테스트
- 입력 검증 테스트

### 엔드투엔드 테스트
`aidlc-docs/construction/build-and-test/e2e-test-instructions.md` 만들기:
- 완전한 사용자 작업 흐름 테스트
- 서비스 간 시나리오
- UI 테스트(해당되는 경우)

---

## 7단계: 테스트 요약 생성

`aidlc-docs/construction/build-and-test/build-and-test-summary.md` 만들기:

```markdown
# Build and Test Summary

## Build Status
- **Build Tool**: [Tool name]
- **Build Status**: [Success/Failed]
- **Build Artifacts**: [List artifacts]
- **Build Time**: [Duration]

## Test Execution Summary

### Unit Tests
- **Total Tests**: [X]
- **Passed**: [X]
- **Failed**: [X]
- **Coverage**: [X]%
- **Status**: [Pass/Fail]

### Integration Tests
- **Test Scenarios**: [X]
- **Passed**: [X]
- **Failed**: [X]
- **Status**: [Pass/Fail]

### Performance Tests
- **Response Time**: [Actual] (Target: [Expected])
- **Throughput**: [Actual] (Target: [Expected])
- **Error Rate**: [Actual] (Target: [Expected])
- **Status**: [Pass/Fail]

### Additional Tests
- **Contract Tests**: [Pass/Fail/N/A]
- **Security Tests**: [Pass/Fail/N/A]
- **E2E Tests**: [Pass/Fail/N/A]

## Overall Status
- **Build**: [Success/Failed]
- **All Tests**: [Pass/Fail]
- **Ready for Operations**: [Yes/No]

## Next Steps
[If all pass]: Ready to proceed to Operations phase for deployment planning
[If failures]: Address failing tests and rebuild
```

---

## 8단계: 상태 추적 업데이트

`aidlc-docs/aidlc-state.md` 업데이트:
- 빌드 및 테스트 단계를 완료로 표시
- 현재 상태 업데이트

---

## 9단계: 사용자에게 결과 제시

다음 구조에 완료 메시지가 표시됩니다.
     1. **완료 알림**(필수): 항상 다음으로 시작하세요.

```markdown
# 🔨 Build and Test Complete
```

     2. **AI 요약**(선택 사항): 빌드 및 테스트 결과에 대한 구조화된 주요 항목 요약을 제공합니다.
        - 형식: "빌드 및 테스트가 다음 결과와 함께 완료되었습니다."
        - 빌드 상태 및 아티팩트 나열
        - 카테고리별(단위, 통합, 성능 등) 테스트 결과 나열
        - 생성된 지침 파일 나열
        - 워크플로 지침을 포함하지 마세요("검토해 주세요", "알려주세요", "다음 단계로 진행하세요", "진행하기 전에").
        - 사실에 기반하고 내용 중심을 유지하세요.
     3. **형식화된 워크플로 메시지**(필수): 항상 다음과 같은 정확한 형식으로 끝납니다.

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the build and test summary at: `aidlc-docs/construction/build-and-test/build-and-test-summary.md`



> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the build and test instructions based on your review
> ✅ **Approve & Continue** - Approve build and test results and proceed to **Operations**

---
```

---

## 10단계: 상호작용 기록

**필수**: `aidlc-docs/audit.md`에 단계 완료를 기록합니다.

```markdown
## Build and Test Stage
**Timestamp**: [ISO timestamp]
**Build Status**: [Success/Failed]
**Test Status**: [Pass/Fail]
**Files Generated**:
- build-instructions.md
- unit-test-instructions.md
- integration-test-instructions.md
- performance-test-instructions.md
- build-and-test-summary.md

---
```
