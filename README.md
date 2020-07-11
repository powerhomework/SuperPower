<p align=center>
    <img src="img/sample.jpg">
</p>

# Q1. 기술적으로 어떤 것이 문제 였나?

> < 실무자와 주고받은 메일 속 내용 기술 부분 > ← 어떻게 넣을지 수정 필요

## 1) Responder 프로그램 자체의 문제

### 1-1) dynamic analysis 기능 작동에 관한 서술
- 여러 종류의 컴퓨터에서 Responder의 **dynamic analysis** 기능X
    - Windows XP En SP2/SP3, Windows XP Ko SP3, Windows 2003 SP2
- dynamic analysis 외의 기능은 정상 동작 확인
    - 커널 디버거, 가상머신 메모리 분석, 메모리 덤프 (static)
    - runtime analysis 본래 기능 X (5830)

### 1-2) dynamic analysis 기능 미작동 증명 동영상
- 구매자의 지뢰 찾기 프로그램 통한 작동 검증 동영상 (3에 자세하게 기술)
    - dynamic analysis 기능에서 analyze binary를 실행
    - 완료된 폴더에서 Bookmarks, Strings, Symbols 폴더만 존재
    - Global 폴더가 존재하지 않아, dynamic analysis 불가능
    - Global 폴더는 binary에서 찾은 모든 함수와 코드가 포함된 폴더로 **동적 분석에 필수적**
- 동영상 내용
    - 영상 속 프로그램 및 버전 : 
        - hbGary Responder professional Edition ver1.3.0.376
        - OS : windows XP SP3 
    - 영상 속 액션 : 
        - `project 생성` → `working canvas` → `debugging` → `target process : winmine.exe`, `PID:5600 → dynamic analysis` → `"Write not yet available, waiting ... "`
    - 영상 속 액션2 :
        - `winmine.exe 우클릭` → `package` → `analyze binary` → `bookmarks , strings, symbols 생성` → `기능 오류 (5830)`

## 2) Responder 프로그램을 대처하는 Flypaper의 기술적 부족

### 2-1) Flypaper가 완벽하게 대체 가능하다고 가정한 실무자의 요구사항
- Runtime demo 영상에서 설명한 feature
- blackhat 2007에서 언급된 Active Reversing

### 2-2) HBGary에서 기술한 Flypaper의 일부 기술
- proximity browsing and layers (live debugging data)
- checkpoint locations on the recording timeline
    - 그래프에 색깔 넣어서 다른 행동을 보이는 그룹들 분리 가능
    - blackhat 발표 내용과 비슷한 기능

### 2-3) HBGary에서 기술한 Flypaper의 부족한 기술
- Class Reconstricton (당시 추가 계획만 잡았었음)
- phase-space analysis (계획 조차 X)

---

> < hbGary 사건 발생에 대한 기술적 배경 > ← 어떻게 넣을지 수정 필요

## 1) 회사가 입은 실질적 피해

### 1-1) 회사 내부 피해
- 회사 웹 사이트가 오프라인 상태로 접속이 불가
- 1TB의 백업이 삭제
- 개인 iPad가 지워짐

### 1-2) 회사 외부 피해
- 이메일 유출
    - HBGary Federal에서 호스팅하는 Google 이메일로 이동하는 데 사용되는 웹 사이트에서 사용자 비밀번호를 신속하게 가져와 해독
    - HBGary Federal의 이메일 7만 여 건 유출
- 데이터 삭제
    - HBGary Federal의 웹 사이트를 손상시키고 백업 데이터를 삭제
- 계정 차단
    - Greg Hoglund의 rootkit.com 사이트를 인수 및 암호 변경 통해	
    - 두 회사의 전자 메일 계정 차단

## 2) 피해를 유발한 취약점

### 2-1) 회사 웹페이지의 SQL Injection 취약점을 통한 DB 내용 누출
- hbgaryfederal.com 의 CMS는 데이터를 SQL DB에 저장, 쿼리를 통해 데이터 가져옴
    - SQL 쿼리에 담기는 파라미터 처리과정에서 코드의 결함이 있어 **SQL Injection**이 가능.
    - hbgaryfederal.com 을 익스플로잇 하는데 사용된 URL
        `www.hbgaryfederal.com/pages.php?pageNav=2&page=27`

    위 URL의 `pageNav`, `page parameter`가 안전하게 처리가 되지 않아 해커들은 **Database에서 여러 정보 탈취** 가능

### 2-2) 취약한 알고리즘 사용(MD5)과 Password Hash값을 해독하는 Rainbow tables
- 해당 사이트에서 **해시 알고리즘으로 MD5를 채택**
    - HBgary는 해당 사이트에서 Password를 저장할 때 MD5 알고리즘을 적용
    - salting이나 반복해싱을 적용하지 않았다. → **레인보우 테이블 공격 취약**
    - 다행히도 레인보우 테이블의 key limitation 때문에 복잡한 암호는 안전

    문제는 HBgary는 매우 단순한 암호를 사용했고 공격자는 쉽게 **평문화된 Password를 탈취**
    
### 2-3) 계정 관련 취약점
- CMS 비밀번호가 이메일, 트위터 및 링크를 포함한 여러 계정에도 사용
    
    → 침입자는 최고 운영 책임자의 이메일 계정에 액세스 가능
- 6자와 두 자리의 숫자로만 구성된 비밀번호를 사용하였고, 해당 비밀번호를 수많은 다른 서비스(플랫폼)에 사용한 것도 모자라 기술지원용 리눅스 머신(support.hbgary.com)에도 사용 <sub>(Gyunka, Benjamin & Abikoye, Oluwakemi. (2017). Analysis of Human Factors in Cyber Security: A Case Study of Anonymous Attack on Hbgary. Computing and Information Systems Journal. 21. 10-18. )</sub>
- HBGary와 HBGary Federal 두 회사 모두 동일한 구글 앱스 계정을 사용하여 피해가 가중 

### 2-4) 1-day attack
- 기술지원용 리눅스 머신(support.hbgary.com)에도 큰 허점이 있었는데, 
**서버 OS 버전을 업데이트** 하지 않아 1day 권한 상승 익스플로잇이 사용되어 해커들이 루트 권한을 획득 <sub>(Gyunka, Benjamin & Abikoye, Oluwakemi. (2017). Analysis of Human Factors in Cyber Security: A Case Study of Anonymous Attack on Hbgary. Computing and Information Systems Journal. 21. 10-18. )</sub>

### 2-4-1) 권한 상승 추가 설명
- GNU C로더의 보안 취약점
    - 이에 취약한 Linux 시스템 운영을 통해 인가되지 않은 사용자가 시스템에서 루트 권한을 획득한 후 백업 및 연구 데이터에 접근
    - GNU-C 라이브러리의 특정 런타임 보호 메커니즘은 `argv[0]` 및 역 추적 정보를 인쇄함으로써 프로세스 메모리에서 민감한 정보 획득
    - CVE-2010-3192
- Set-UID 프로그램
    - 구현시, 스택 기반 버퍼 오버플로우 오류 포함
    - Set-UID 권한으로 동적 링크 라이브러리를 로드하면 라이브러리에서 루트 권한으로 실행
    - 하드 링크, 파일 디스크립터 리디렉션 및 환경 변수 설정을 포함한 다양한 트릭을 통해 루트 권한을 얻을 수 있음

---

# Q2. 실무자가 원했던 기능은 무엇이었나?

 실무자는 Responder 디버거에서 제공하는 **`Runtime Analysis`** 기능을 원했다.

## 1) 실무자의 Runtime Analysis를 위한 Responder 디버거 구매

- 실무자는 **Runtime Analysis 기능을 사용하기 위해서 Responder를 구매**했다. **Runtime Analysis 기능**은 HBGary의 주요 홈페이지인  Runtime Malware Analysis에 게시된 비디오에서, 그리고 2007년 블랙햇 발표에서 소개되었다.

- Runtime Analysis는 리버스 엔지니어링 문제를 쉽게 해결하는 것이 목적이다. 주요 기능으로는 코드와 데이터의 흐름을 동적수집 후 이를 그래프에 저장하여, 객체와 이벤트간의 상관관계, 특정 데이터의 존재등을 분석하는 기능을 한다.

## 2) 실무자의 Responder 사용 및 문제발생 후 해결시도

- 실무자는 HBGary Responder를 구매한 후 **User Guide 의 Instruction**에 따라 **1차 테스트를 시도**하여, "Analyze Binary" 섹션을 얻는데는 성공하였다. 하지만 **subroutine과 binary code에 대한 access를 제공하는 부분인 "Global" 섹션을 얻을 수가 없는 문제가 발생**하였다.

- 실무자는 Runtime Analysis 기능문제의 해결을 위해 다음과 같은 방법을 시도하였다.
    - HBGary Support Team에 문제상황을 문의하여 설명서 PDF파일을 수령하였다(수령).
    - 설명서 PDF를 토대로 2차 Responder 사용을 시도하였으나, 버그 해결에 실패하였다.
    - 앞서 실패한 테스트 상황에 대한 명세 및 테스트 영상을 HBGray사에 직접 문의하였다.
    - HBGary 지원포럼에 가입하여 문제 상황 문의 및 해결을 시도하였다.
    - 추가적으로 설명서 PDF에 명시된 여러 지원 환경에서 해당기능에 대한 **3차 테스트를 시도**하였으나 모두 **실패**하였다. (테스트 환경: `Windows 2003 SP1 Ko`, `Windows XP SP2 En`, `Windows XP SP3 En`, `Windows XP SP3 Ko`)

**User Guide에 명시되어 있는 적합한 환경에서 테스트를 하고 pdf와 동영상을 HBGary사에 적극적으로 보낸 것으로 보아 실무자는 Responder의 ‘Runtime Analysis’ 기능이 필요 했음을 명백히 확인 할 수 있다.**
