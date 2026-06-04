# 속성 기반 테스트 — 옵트인

**확장**: 속성 기반 테스트

## 옵트인 프롬프트

이 확장이 로드되면 질문을 명확하게 하는 요구 사항 분석에 다음 질문이 자동으로 포함됩니다.

```markdown
## Question: Property-Based Testing Extension
Should property-based testing (PBT) rules be enforced for this project?

A) Yes — enforce all PBT rules as blocking constraints (recommended for projects with business logic, data transformations, serialization, or stateful components)
B) Partial — enforce PBT rules only for pure functions and serialization round-trips (suitable for projects with limited algorithmic complexity)
C) No — skip all PBT rules (suitable for simple CRUD applications, UI-only projects, or thin integration layers with no significant business logic)
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```
