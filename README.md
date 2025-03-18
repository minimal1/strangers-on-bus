# Strangers on Bus

**Strangers on Bus**는 서울특별시 버스정류장에서 버스를 기다리는 시간을 즐겁게 만들어주는 위치 기반 미니게임 서비스입니다. 이용자들은 버스 정류장에서 간단한 미니게임을 통해 점수를 기록하고, 다른 익명의 사용자들과 경쟁할 수 있습니다.

## 핵심 기능

- **위치 기반 게임**: GPS를 활용하여 버스정류장 근처에서만 게임 참여 및 기록 가능
- **간단한 미니게임**: 약 1분 내외로 즐길 수 있는 단순하면서도 중독성 있는 게임
- **정류장별 랭킹**: 각 버스정류장마다 기록된 최고 점수와 랭킹 확인 가능
- **도장깨기 요소**: 여러 버스정류장을 방문하며 다양한 장소에서 기록 도전

## 기술 스택

- **프론트엔드**: Angular, TypeScript, SCSS
- **백엔드**: NestJS, TypeScript, PostgreSQL
- **인프라**: Docker, GitHub Actions

## 프로젝트 문서 구조

Strangers on Bus 프로젝트는 다음과 같은 문서 구조로 관리됩니다:

- **[제품 스펙](docs/product.md)**: 프로젝트의 기능적 요구사항과 사용자 경험에 대한 설명
- **[기술 스펙](docs/technology.md)**: 프로젝트에 사용된 기술 스택과 아키텍처 상세 정보
- **문화 및 프로세스**:
  - [프로젝트 관리](docs/culture/work/project-management.md): 스크럼 프로세스 및 GitHub Project 활용 방법
  - [커뮤니케이션](docs/culture/work/communication.md): 팀 커뮤니케이션 채널과 원칙
  - [문서화 규칙](docs/culture/work/documentation.md): 문서 작성 가이드라인
- **개발 규칙**:
  - [Conventional Commit](docs/culture/development/conventional-commit.md): 커밋 메시지 형식
  - [PR 형식 가이드](docs/culture/development/pr-format.md): Pull Request 작성 가이드
  - [Trunk Based Development](docs/culture/development/trunk-based-development.md): 브랜치 전략
  - [모노레포 관리](docs/culture/development/monorepo.md): Turborepo를 활용한 모노레포 관리
- **온보딩**:
  - [프론트엔드 온보딩](docs/onboarding/frontend.md): 프론트엔드 개발 환경 설정 가이드
  - [백엔드 온보딩](docs/onboarding/backend.md): 백엔드 개발 환경 설정 가이드

## 프로젝트 설정 및 실행

```bash
# 의존성 설치
npm install

# 개발 서버 실행
npm run dev

# 빌드
npm run build

# 테스트
npm run test
```

## 외부 리소스

- [GitHub 저장소](https://github.com/minimal1/strangers-on-bus)
- [Slack 워크스페이스](https://strangersonsubway.slack.com)
- [Figma 디자인](https://www.figma.com/files/team/1462005587373981919/project/326294968/Team-project?fuid=1462005585749641576)
- [프로젝트 문서](https://minimal1.github.io/strangers-on-bus/#/)

## 공개 API 활용

다음 서울특별시 공개 API를 활용합니다:

- [서울특별시 정류소정보조회 서비스](https://www.data.go.kr/data/15000303/openapi.do)
- [서울특별시 버스위치정보조회 서비스](https://www.data.go.kr/data/15000332/openapi.do)
- [서울특별시 버스도착정보조회 서비스](https://www.data.go.kr/data/15000314/openapi.do)