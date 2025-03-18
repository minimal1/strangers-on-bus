# Conventional Commit

## 개요

Conventional Commit은 커밋 메시지에 사람과 기계 모두가 이해할 수 있는 의미를 부여하기 위한 명세입니다. 이 규칙을 따르면 자동화된 도구를 사용하여 변경 로그를 생성하거나 의미론적 버전(Semantic Versioning)을 자동으로 결정할 수 있습니다.

## 기본 구조

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Strangers on Bus 프로젝트 커밋 메시지 가이드

### 1. Type (필수)

커밋의 유형을 나타내며, 다음 중 하나를 사용합니다:

- **feat**: 새로운 기능 추가
- **fix**: 버그 수정
- **docs**: 문서 변경만 있는 경우
- **style**: 코드 의미에 영향을 주지 않는 변경사항 (공백, 서식, 세미콜론 누락 등)
- **refactor**: 버그 수정이나 기능 추가가 아닌 코드 변경
- **perf**: 성능 개선 관련 코드 변경
- **test**: 테스트 추가 또는 기존 테스트 수정
- **build**: 빌드 시스템이나 외부 종속성에 영향을 미치는 변경사항 (예: npm, webpack, gulp)
- **ci**: CI 설정 파일 및 스크립트 변경
- **chore**: 소스나 테스트 파일을 수정하지 않는 기타 변경사항
- **revert**: 이전 커밋을 되돌리는 경우

### 2. Scope (선택사항)

커밋이 영향을 미치는 범위를 나타냅니다. 우리 프로젝트에서는 다음과 같은 범위를 사용합니다:

- **api**: 백엔드 API 관련 변경
- **ui**: 사용자 인터페이스 관련 변경
- **auth**: 인증/인가 관련 변경
- **db**: 데이터베이스 관련 변경
- **config**: 설정 파일 관련 변경
- **i18n**: 국제화/지역화 관련 변경
- **deps**: 종속성 관련 변경
- **core**: 핵심 로직 관련 변경

### 3. Description (필수)

- 명령형 현재 시제를 사용합니다. (예: "변경", "수정", "추가" - "변경됨", "수정됨"이 아님)
- 첫 글자는 소문자로 시작합니다.
- 끝에 마침표(.)를 사용하지 않습니다.
- 영어로 작성할 경우 위 규칙을 따르고, 한글로 작성할 경우에도 동일한 원칙을 적용합니다.

### 4. Body (선택사항)

- 커밋의 변경 사항에 대한 자세한 설명을 제공합니다.
- 왜 변경했는지, 이전과 어떻게 다른지 설명합니다.
- 각 줄은 100자를 넘지 않도록 합니다.

### 5. Footer (선택사항)

- **Breaking Changes**: 주요 변경사항이 있는 경우 `BREAKING CHANGE:`로 시작하는 푸터를 추가합니다.
- **이슈 참조**: 관련 이슈를 참조하는 경우 `Refs: #123` 또는 `Closes: #123` 형식을 사용합니다.

## 예시

### 기능 추가
```
feat(auth): 소셜 로그인 기능 구현

- Google OAuth2.0 연동
- 사용자 프로필 정보 자동 동기화
- 토큰 갱신 로직 추가

Refs: #42
```

### 버그 수정
```
fix(ui): 모바일 환경에서 네비게이션 메뉴 표시 오류 수정

iPhone SE 등 작은 화면에서 메뉴가 잘리는 문제를 해결하기 위해
반응형 디자인 적용

Closes: #103
```

### 문서 업데이트
```
docs(api): API 문서 예제 코드 업데이트

신규 엔드포인트 호출 방법에 대한 예제 추가
```

### 주요 변경사항
```
refactor(core): 사용자 인증 로직 전면 개편

JWT 기반 인증에서 세션 기반 인증으로 변경

BREAKING CHANGE: 기존 토큰 기반 API 인증 방식이 더 이상 동작하지 않습니다.
신규 세션 기반 인증 방식으로 클라이언트 코드를 업데이트해야 합니다.
```

## 도구 및 자동화

### commitlint 설정

```js
// commitlint.config.js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      [
        'feat',
        'fix',
        'docs',
        'style',
        'refactor',
        'perf',
        'test',
        'build',
        'ci',
        'chore',
        'revert',
      ],
    ],
    'scope-enum': [
      2,
      'always',
      ['api', 'ui', 'auth', 'db', 'config', 'i18n', 'deps', 'core'],
    ],
  },
};
```

### husky 설정

```json
// package.json 일부
{
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  }
}
```

## 이점

- **명확한 커밋 이력**: 변경 사항의 목적을 쉽게 파악할 수 있습니다.
- **자동화 가능**: CHANGELOG 생성 및 버전 관리를 자동화할 수 있습니다.
- **프로젝트 역사 파악**: 프로젝트의 발전 과정을 더 쉽게 이해할 수 있습니다.
- **협업 개선**: 다른 개발자의 변경 사항을 더 쉽게 이해할 수 있습니다.

## 참고 자료

- [Conventional Commits 공식 명세](https://www.conventionalcommits.org/en/v1.0.0/)
- [Angular Commit Message Guidelines](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit)
- [Semantic Versioning](https://semver.org/)