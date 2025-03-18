# Trunk Based Development

## 개요

Trunk Based Development(TBD)는 소프트웨어 개발 방법론으로, 모든 개발자가 단일 브랜치('trunk' 또는 'main')에 직접 커밋하거나 짧게 유지되는 기능 브랜치를 통해 작업하는 방식입니다. 이 접근 방식은 지속적 통합(CI)과 지속적 배포(CD)의 핵심 요소입니다.

## 핵심 원칙

1. **단일 메인 브랜치**: 'trunk', 'main' 또는 'master'라고 불리는 하나의 주요 브랜치가 존재합니다.
2. **짧은 수명의 브랜치**: 기능 브랜치는 1-2일 이내로 짧게 유지됩니다.
3. **작은 단위의 커밋**: 큰 변경 사항보다는 작고 독립적인 변경 사항을 자주 커밋합니다.
4. **지속적 통합**: 모든 코드 변경 사항은 빈번하게 통합됩니다.
5. **빠른 피드백**: 자동화된 테스트와 코드 리뷰를 통해 문제를 조기에 발견합니다.

## Strangers on Bus 프로젝트 적용 가이드

### 브랜치 전략

```
main (또는 trunk) - 배포 가능한 코드만 존재
  ↑
feature/<기능명> - 짧은 수명 (1-2일)
```

### 작업 흐름

1. **기능 분기**: 새로운 기능을 개발할 때는 `main`에서 새로운 브랜치를 생성합니다.
   ```bash
   git checkout -b feature/user-authentication main
   ```

2. **작은 단위로 커밋**: 작은 단위의 완성된 기능을 자주 커밋합니다.

3. **자주 리베이스**: `main` 브랜치의 변경 사항을 자주 가져와 통합 충돌을 미리 해결합니다.
   ```bash
   git fetch origin
   git rebase origin/main
   ```

4. **PR 제출 및 통합**: 기능이 완성되면 PR을 생성하고, 리뷰 후 `main`에 병합합니다.

5. **배포**: `main` 브랜치는 항상 배포 가능한 상태를 유지합니다.

### 기술적 운영 방안

1. **Feature Toggle**: 아직 완성되지 않은 기능은 Feature Toggle을 사용하여 프로덕션 환경에서 비활성화할 수 있습니다.

2. **자동화된 테스트**: 모든 PR은 자동화된 테스트를 통과해야 병합될 수 있습니다.

3. **코드 리뷰**: 모든 PR은 최소 1명 이상의 팀원에게 리뷰를 받아야 합니다.

4. **CI/CD 파이프라인**: GitHub Actions를 활용하여 자동화된 빌드, 테스트, 배포 파이프라인을 구성합니다.

## 장점

- **지속적 통합**: 병합 충돌 감소 및 코드 품질 향상
- **빠른 피드백 루프**: 문제를 조기에 발견하고 수정
- **배포 주기 단축**: 작은 변경 사항을 자주 배포
- **팀 협업 증진**: 코드 가시성 향상 및 지식 공유

## 단점 및 극복 방안

- **미완성 코드 노출**: Feature Toggle을 사용하여 해결
- **테스트 자동화 필요**: 테스트 주도 개발(TDD) 권장
- **신중한 계획 필요**: 작은 단위로 나눌 수 있는 작업 계획

## 참고 자료

- [Trunk Based Development 공식 사이트](https://trunkbaseddevelopment.com/)
- [마틴 파울러의 브랜치 관리 전략](https://martinfowler.com/articles/branching-patterns.html)
- [Feature Toggle 패턴](https://martinfowler.com/articles/feature-toggles.html)