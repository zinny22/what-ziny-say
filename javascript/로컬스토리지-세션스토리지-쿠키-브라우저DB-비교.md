# 로컬스토리지, 세션스토리지, 쿠키, 브라우저DB 비교

웹에서는 데이터를 저장하는 방식이 다양합니다.<br/>
자주 사용하는 4가지 방식에 대해서 비교해보겠습니다.

## ✅ 요약 비교

| 항목 | 로컬스토리지 | 세션스토리지 | 쿠키 | indexedDB |
|------|--------------|--------------|------|-----------|
| 저장 위치 | 브라우저 | 브라우저 | 브라우저 (자동 전송) | 브라우저 DB |
| 수명 | 영구 | 탭 닫힐 때까지 | 수동 설정 | 영구 또는 설정 |
| 용량 | 5~10MB | 약 5MB | 4KB | 수십~수백 MB |
| 서버 전송 | ❌ | ❌ | ✅ | ❌ |
| 주 사용처 | 토큰, 설정 | 임시 상태 | 세션, 인증 | 대용량 캐시 |

### 1. LocalStorage
- 브라우저에 영구적으로 저장됩니다. 즉 사용자가 지우기 전까지는 남아있습니다.
- window.localStorage.setItem(key, value)의 형태로 사용합니다.
- 간단하고 빠르고 영구적입니다.
- ❌ 보안에 취약합니다. (XSS 대응이 필요합니다)
> 토큰, 사용자 설정, 다크모드에 사용됩니다.

### 2. SessionStorage
- 브라우저의 탭 단위로 저장됩니다 즉 탭을 닫으면 사라집니다.
- window.sessionStorage.setItem(key,value)의 형태로 사용합니다.
- 탭 마다 고유한 값으로 관리가 가능합니다.
- ❌ 탭을 닫으면 값이 휘발되고, 브라우저 재시작시 초기화 됩니다.
> 일시적인 폼데이터, 온보딩의 흐름, 비로그인 상태관리 등에 사용됩니다.

### 3. Cookie
- 도메인 기준으로 매 요청마다 자동으로 서버에 전송됩니다.
- document.cookie 또는 서버에서 Set-Cookie 헤더를 사용합니다.
- 도메인, 경로, httpOnly 등 설정이 가능합니다.
- 서버와 자동으로 동기화 됩니다.
- ❌ 용량의 제한이 있고(4KB), 보안 위협이 존재합니다.
> 로그인 세션, 사용자 트래킹에 사용됩니다.

### 4. indexeDB
- 구조화된 데이터를 저장할 수 있는 브라우저 내장 데이터베이스 입니다.(JSON형태)
- 큰 용량과, 구조화된 저장이 가능합니다.
- ❌ 사용법이 복잡하고, API가 다소 번거롭습니다.
> 오프라인 앱, 이미지 캐시, 채딩 로그 등에 사용됩니다.

## 🗣️ What-ziny-say

**로컬스토리지**는 브라우저가 닫혀도 저장되어있는 영구적인 저장소 입니다.<br/>
사용이 간단하고 빠르지만, 보안에 취약하다는 단점이 있습니다.

**세션스토리지**는 브라우저의 탭 단위로 관리되는 저장소 입니다.<br/>
탭을 닫으면 저장한 데이터가 사라지기 때문에 탭 마다 고유한 값으로 관리가 가능합니다.<br/>
데이터가 휘발된다는 장점과 단점을 동시에 가지고 있기 때문에<br/>
일시적인 데이터나, 비로그인의 상태관리 등에 사용하기 적합합니다.

**쿠키**에 저장된 데이터는 도메인 기준으로 매 요청마다 자동으로 서버에 전송하는 장점이 있습니다.<br/>
영구적으로 저장할지, 휘발성있게 저장할지는 설정할수 있으나,<br/>
용량이 작고, 보안 위협이 존재한다는 단점이 있습니다.

**인덱스DB**는 브라우저 내장 데이터베이스 입니다.<br/>
JSON형식으로 구조화된 데이터를 저장할 수 있고, 용량이 크다는 장점을 가지고 있습니다.<br/>
하지만 사용법이 복잡하다는 단점이 있습니다.

개발 초기에는 로컬스토리지에 모든 정보를 저장해서 사용했습니다.<br/>
하지만 보안에 취약하다는 사실을 알고나서는 httpOnly 쿠키를 사용했습니다.

모든 인증 정보를 항상 쿠키에만 저장해야 되는것은 아니라고 생각합니다.<br/>
개발의 환경이나 특정 상황에 맞춰서 적절히 저장소를 골라 사용하면 된다고 생각합니다.
