# 콘텐츠 검증 규칙

## 필수: 파일 생성 전 콘텐츠 유효성 검사

**중요**: 구문 분석 오류를 방지하려면 파일에 쓰기 전에 생성된 모든 콘텐츠의 유효성을 검사해야 합니다.

## ASCII 다이어그램 표준

**중요**: ASCII 다이어그램이 포함된 파일을 생성하기 전에:

1. **로드** `common/ascii-diagram-standards.md`
2. **검증** 각 다이어그램:
   - 줄당 문자 수 계산(모든 줄은 너비가 동일해야 함)
   - 다음에만 사용: `+` `-` `|` `^` `v` `<` `>` 및 공백
   - 유니코드 상자 그리기 문자 없음
   - 공백만(탭 없음)
3. **테스트** 상자 모서리가 수직으로 정렬되는지 확인하여 정렬

**패턴 및 검증 체크리스트는 `common/ascii-diagram-standards.md`을(를) 참조하세요.**

## 인어 다이어그램 검증

### 필수 검증 단계
1. **구문 검사**: 파일 생성 전에 Mermaid 구문 유효성을 검사합니다.
2. **문자 이스케이프**: 특수 문자가 올바르게 이스케이프되었는지 확인하세요.
3. **대체 콘텐츠**: Mermaid가 유효성 검사에 실패할 경우 대체 텍스트 제공

### 인어 검증 규칙
```markdown
## BEFORE creating any file with Mermaid diagrams:

1. Check for invalid characters in node IDs (use alphanumeric + underscore only)
2. Escape special characters in labels: " → \" and ' → \'
3. Validate flowchart syntax: node connections must be valid
4. Test diagram parsing with simple validation

## FALLBACK: If Mermaid validation fails, use text-based workflow representation
```

### 구현 패턴
```markdown
## Workflow Visualization

### Mermaid Diagram (if syntax valid)
```mermaid
[검증된 다이어그램 내용]
```

### Text Alternative (always include)
```
1단계: 시작
- 1단계: 작업공간 감지(완료)
- 2단계: 요구사항 분석(완료)
[텍스트 표현으로 계속]
```

## General Content Validation

### Pre-Creation Validation Checklist
- [ ] Validate embedded code blocks (Mermaid, JSON, YAML)
- [ ] Check special character escaping
- [ ] Verify markdown syntax correctness
- [ ] Test content parsing compatibility
- [ ] Include fallback content for complex elements

### Error Prevention Rules
1. **Always validate before using tools/commands to write files**: Never write unvalidated content
2. **Escape special characters**: Particularly in diagrams and code blocks
3. **Provide alternatives**: Include text versions of visual content
4. **Test syntax**: Validate complex content structures

## Validation Failure Handling

### When Validation Fails
1. **Log the error**: Record what failed validation
2. **Use fallback content**: Switch to text-based alternative
3. **Continue workflow**: Don't block on content validation failures
4. **Inform user**: Mention simplified content was used due to parsing constraints
