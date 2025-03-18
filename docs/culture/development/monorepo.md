# Monorepo와 Turborepo

## 개요

Monorepo(모노레포)는 여러 개의 프로젝트나 패키지를 단일 저장소에서 관리하는 개발 방식입니다. Strangers on Bus 프로젝트는 모노레포 접근 방식을 채택하고, 효율적인 빌드 및 종속성 관리를 위해 Turborepo를 사용합니다.

## Monorepo란?

### 정의
모노레포는 여러 개의 관련된 프로젝트들이 하나의 버전 관리 시스템 저장소 내에서 관리되는 소프트웨어 개발 전략입니다.

### 주요 특징

1. **코드 공유**: 여러 프로젝트 간에 코드와 구성 요소를 쉽게 공유할 수 있습니다.
2. **atomic 커밋**: 여러 프로젝트에 걸친 변경사항을 하나의 커밋으로 처리할 수 있습니다.
3. **중앙화된 종속성 관리**: 모든 프로젝트의 종속성을 중앙에서 관리합니다.
4. **일관된 개발 환경**: 모든 프로젝트가 동일한 툴체인과 설정을 공유합니다.
5. **조정된 릴리스**: 관련 프로젝트 간의 릴리스를 동기화할 수 있습니다.

### 모노레포 vs 멀티레포

| 특징 | 모노레포(Monorepo) | 멀티레포(Multirepo) |
|------|-------------------|-------------------|
| 코드 공유 | 쉬움 | 어려움 (패키지 배포 필요) |
| 통합 테스트 | 간단함 | 복잡함 |
| 리팩토링 | 쉬움 (전체 영향 파악 가능) | 어려움 |
| 저장소 크기 | 커짐 | 작음 |
| 접근 제어 | 세분화 어려움 | 세분화 쉬움 |
| 빌드 속도 | 최적화 필요 | 개별적으로 빠름 |

## Turborepo 소개

Turborepo는 JavaScript/TypeScript 모노레포를 위한 고성능 빌드 시스템입니다. Vercel에서 개발한 이 도구는 빌드 프로세스를 가속화하고, 모노레포의 복잡성을 관리하는 데 도움을 줍니다.

### 주요 기능

1. **캐싱**: 이전에 실행된 태스크의 결과를 캐시하여 중복 작업을 방지합니다.
2. **병렬 실행**: 태스크를 병렬로 실행하여 빌드 시간을 단축합니다.
3. **종속성 인식**: 패키지 간의 종속성을 이해하고 올바른 순서로 태스크를 실행합니다.
4. **원격 캐싱**: 팀 전체에서 빌드 캐시를 공유할 수 있습니다.
5. **프로파일링**: 빌드 성능을 분석하고 병목 현상을 식별할 수 있습니다.

## Strangers on Bus 프로젝트 모노레포 구조

```
strangers-on-bus/
├── apps/
│   ├── web/             # 웹 애플리케이션 (Angular)
│   └── api/             # API 서버 (NestJS)
├── packages/
│   ├── ui/              # 공유 UI 컴포넌트
│   ├── eslint-config/   # 공유 ESLint 설정
│   ├── tsconfig/        # 공유 TypeScript 설정
│   └── utils/           # 공유 유틸리티 함수
├── docs/                # 프로젝트 문서
├── package.json         # 루트 package.json
├── turbo.json           # Turborepo 설정
└── yarn.lock            # 의존성 잠금 파일
```

## Turborepo 환경 설정

### 설치

```bash
npm install turbo --global
```

### 루트 package.json

```json
{
  "name": "strangers-on-bus",
  "version": "0.0.0",
  "private": true,
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "scripts": {
    "build": "turbo run build",
    "dev": "turbo run dev --parallel",
    "lint": "turbo run lint",
    "test": "turbo run test",
    "clean": "turbo run clean && rm -rf node_modules",
    "format": "prettier --write \"**/*.{ts,tsx,md}\""
  },
  "devDependencies": {
    "prettier": "^2.8.8",
    "turbo": "^1.10.3"
  }
}
```

### turbo.json 설정

```json
{
  "$schema": "https://turbo.build/schema.json",
  "globalDependencies": ["**/.env.*local"],
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**", ".next/**", "public/dist/**"]
    },
    "test": {
      "dependsOn": ["^build"],
      "outputs": ["coverage/**"],
      "inputs": ["src/**/*.tsx", "src/**/*.ts", "test/**/*.ts", "test/**/*.tsx"]
    },
    "lint": {
      "outputs": []
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "clean": {
      "cache": false
    }
  }
}
```

## 워크스페이스 패키지 설정

각 애플리케이션과 패키지는 자체 `package.json` 파일을 가집니다.

### 애플리케이션 예시 (apps/web/package.json)

```json
{
  "name": "web",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "dev": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint"
  },
  "dependencies": {
    "@angular/common": "^13.0.0",
    "@angular/core": "^13.0.0",
    "ui": "*",
    "utils": "*"
  },
  "devDependencies": {
    "@angular/cli": "^13.0.0",
    "eslint-config-custom": "*",
    "tsconfig": "*"
  }
}
```

### 공유 패키지 예시 (packages/ui/package.json)

```json
{
  "name": "ui",
  "version": "0.0.0",
  "main": "./index.ts",
  "types": "./index.ts",
  "license": "MIT",
  "scripts": {
    "lint": "eslint .",
    "build": "tsc"
  },
  "devDependencies": {
    "eslint-config-custom": "*",
    "tsconfig": "*",
    "typescript": "^4.9.5"
  }
}
```

## 일반적인 워크플로우

### 1. 개발 환경 시작

```bash
# 모든 애플리케이션과 패키지를 개발 모드로 실행
yarn dev

# 특정 애플리케이션만 실행
yarn workspace web dev
```

### 2. 빌드 실행

```bash
# 모든 패키지와 애플리케이션 빌드
yarn build

# 특정 애플리케이션만 빌드
yarn workspace api build
```

### 3. 테스트 실행

```bash
# 모든 테스트 실행
yarn test

# 특정 패키지의 테스트만 실행
yarn workspace utils test
```

### 4. 새 패키지 추가

```bash
mkdir -p packages/new-package
cd packages/new-package
yarn init
```

## 모범 사례

1. **패키지 간 명확한 경계 설정**: 패키지의 책임과 API를 명확히 정의합니다.
2. **내부 종속성 버전 관리**: 내부 패키지는 `*` 또는 워크스페이스 프로토콜(`workspace:*`)을 사용합니다.
3. **공유 설정 활용**: ESLint, TypeScript 등의 설정을 공유 패키지로 추출합니다.
4. **캐시 활용**: `.turbo` 캐시 디렉토리를 Git에서 무시하고, CI에서는 캐시를 활용합니다.
5. **CI 파이프라인 최적화**: `turbo prune`을 사용하여 CI에서 필요한 패키지만 빌드합니다.

## 문제 해결

### 캐싱 문제

```bash
# 캐시 지우기
turbo clean

# 캐시 무시하고 실행
turbo run build --force
```

### 종속성 문제

```bash
# 종속성 그래프 확인
turbo run build --dry-run --graph
```

## 참고 자료

- [Turborepo 공식 문서](https://turbo.build/repo/docs)
- [Yarn Workspaces 문서](https://classic.yarnpkg.com/en/docs/workspaces/)
- [NX 모노레포 도구](https://nx.dev/)
- [Lerna](https://lerna.js.org/)