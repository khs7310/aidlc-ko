# 보안 기준 — 옵트인

**확장**: 보안 기준

## 옵트인 프롬프트

이 확장이 로드되면 질문을 명확하게 하는 요구 사항 분석에 다음 질문이 자동으로 포함됩니다.

```markdown
## Question: Security Extensions
Should security extension rules be enforced for this project?

A) Yes — enforce all SECURITY rules as blocking constraints (recommended for production-grade applications)
B) No — skip all SECURITY rules (suitable for PoCs, prototypes, and experimental projects)
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```
