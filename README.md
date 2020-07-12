```
Description
```

<p align=center>
    <img src="img/sample.jpg">
</p>

---

<p align=center><strong>< 실무자와 주고받은 메일 속 내용 기술 부분 ></strong></p>

# Q1. 기술적으로 어떤 것이 문제 였나?

## 1) Responder 프로그램 자체의 문제

### 1-1) dynamic analysis 기능 작동에 관한 서술
- 여러 종류의 컴퓨터에서 Responder의 **dynamic analysis** 기능 X
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
        - `project 생성` → `working canvas` → `debugging` → `target process : winmine.exe`, `PID:5600 → dynamic analysis` → `"Write not yet available, waiting ..."`
    - 영상 속 액션2 :
        - `winmine.exe 우클릭` → `package` → `analyze binary` → `bookmarks, strings, symbols 생성` → `기능 오류 (5830)`

## 2) Responder 프로그램을 대처하는 Flypaper의 기술적 부족 <sup id="1"><a href="https://youtu.be/PAFM9jSIXEQ">1</a></sup>

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

# Q2. 실무자가 원했던 기능은 무엇이었나? 

## 1) Responder의 동적 분석 기능 및 문제 해결

### 1-1) 기술적인 기능

- 실무자는 **Runtime Analysis 기능**을 사용하기 위해서 **Responder**를 구매
    - 해당 기능은 HBGary의 주요 홈페이지에 게시된 비디오 및 2007년 블랙햇 발표<sup id="2"><a href="https://www.blackhat.com/presentations/bh-usa-07/Hoglund/Presentation/bh-usa-07-hoglund.pdf">2</a></sup>에서 소개
    - 해당 기능은 리버스 엔지니어링 문제를 쉽게 해결하는 것이 목적
    - 코드와 데이터의 흐름을 동적수집 후 그래프에 저장
    - 객체와 이벤트간의 상관관계, 특정 데이터의 존재등을 분석 → 실무자가 원했던 구체적인 기능

- 구입한 **Responder**의 기능 문제 발생
    - 실무자는 구매 후 User Guide<sup id="3"><a href="https://wikileaks.org/hbgary-emails//fileid/57263/15937">3</a></sup>의 Instruction에 따라 **1차 테스트를 시도**
    - "Analyze Binary" 섹션을 얻는데는 성공
    - subroutine과 binary code에 대한 access를 제공하는 **"Global" 섹션 획득 실패**하였음.

### 1-2) 업무적인 기능
- 명세와 테스트 영상을 통한 **문의**
    - Support Team에 문제상황을 문의하여 설명서 PDF파일을 수령
    - 설명서 PDF를 토대로 2차 Responder 사용을 시도하였으나, 버그 해결에 실패
    - 문제를 적어 놓을 PDF를 통한 수차례 **문의**
    - 포럼 가입 및 여러 사람에게 메일 전송 등 **실무자의 해결 의지**
    - 실패한 테스트 상황에 대한 명세 및 테스트 영상을 직접 **문의**

        → 버그 해결을 원했지만 회사 내 **Communication 부족으로 답변 지연**

    - **답변이 없는 동안** 추가적으로 설명서 PDF에 명시된 여러 지원 환경에서 해당 기능에 대한 3차 테스트를 시도하였으나 모두 실패

---

# Q3. HBgary의 문제점은 무엇이었나?

## 1) 제품 관련 업무 Process 문제

### 1-1) HBGary사의 정확한 제품 기능 미고지
- HBGary사 측은 블랙햇 컨퍼런스에서 발표한 Responder 제품에 내장된 기능에 대해 정확히 고지를 하지 않음.
- 블랙햇 컨퍼런스 당시 HBGary사에서 소개한 기능
    - 웹 사이트에 게시된 데모 비디오에서 Dynamic Analysis 소개
    - 블랙햇 컨퍼런스에서 Active Reversing 연구 발표
- 실제 제품에 반영된 정도
    - proximity browsing and layers 기능은 정상 사용이 가능하였음.
    - class reconstruction과 phase-space analysis 기능은 블랫햇 발표내용과 달리 사용이 불가능 하였음.

    컨퍼런스에서 연구 결과가 제품에 추가될 것처럼 발표해서 구매를 유도하였음.
    
    사실상 해당 기능을 상용화해서 추가할 여력이 없었던 것으로 보임.


### 1-2) 제품 관리 프로세스 부재
- 기존 제품에 대한 유지보수 절차의 부재
    - 해당 제품인 HBGary Responder professional Edition이 end of life로 이전되어 업데이트 계획이 없음.
- 대안으로 새 제품 구입을 권고함
    - 기존제품을 연구개발하는 것이 무의미하다고 생각하여 지원을 만료함
    - 실무자가 제보한 버그를 업데이트 하기보다 현재 회사 내 판매 중인 다른 디버거 제품 이용을 추천 

        → 해당 제품의 구매 전, 권고 사항 공지의 부재

## 2) Communication 문제

### 2-1) 고객 관리 부실 및 확인 절차 부재
- 고객 지원을 위한 support.hbgary.com 사이트 개설
- 사이트 계정 발급 전, 고객 확인 절차로  salesforce.com 를 확인
- 고객이 자사의 소프트웨어를 어떻게 쓰고 있는지 대한 설문지를 보낼 것을 언급 

    새로운 사이트 개설에 앞서, 기존 HBgary 사이트의 계정과 연동할 수 있는 방법, 고객 여부를 확인하는 방법을 고려하지 않았음

### 2-2) 지원팀 업무 점검 프로세스 문제
- 실무자는 Responder의 일부 기능이 작동하지 않는 문제로 bob에게 메일 전송
- bob은 문제가 제기된 메일을 지원팀에게 그대로 전달
- 지원팀은 해당 메일을 보낸 실무자에게 연락을 취하지 않았음
- bob은 실무자에게 거듭 메일을 받은 후에야 지원팀의 무응답을 파악함
- 지원팀 업무 처리에 대한 점검 프로세스가 부재
    - 지원팀이 전달 받은 메일을 확인했는지
    - 문제를 제기한 실무자에게 응답을 했는지
    - 해당 문제가 해결되고 있는지 

### 2-3) 팀 내의 소통 문제
- Owner 과 직원 간의 소통의 부재(프로세스가 잡혀있다면 부재가 일어나지 않았음)
    - Bob은 기존 디버거 지원이 중단되므로 새 제품을 구입하라고 실무자에게 권고하고 이를 팀원들(Greg, Rich, Penny, Alex)에게 일방적으로 통보
    - 실무자와의 이메일을 전달하면서 버그패치 및 환불에 대한 의견을 물음.
- 회사의 환불 절차 미규격화
    - 이미 환불이 진행 중인 것으로 확인됨.
    - 실무자가 새 제품 구매를 원한다면 할인을 해주는 것이 필요하나 새제품에 그 기능이 언제 추가될지 단언할 수 없음.

---

<p align=center><strong>< HBGary 사건 발생에 대한 기술적 배경 ></strong></p>

## 1) 회사가 입은 실직적 피해

### 1-1) 회사 내부 피해
- 회사 웹 사이트가 오프라인 상태로 접속이 불가
- 1TB의 백업이 삭제
- 개인 iPad가 지워짐
- 외부에서 들어오는 Input 대처 확인 X

### 1-2) 회사 외부 피해<sup id="4"><a href="https://arstechnica.com/tech-policy/2011/02/virtually-face-to-face-when-aaron-barr-met-anonymous/">2</a></sup>
- 이메일 유출
    - HBGary Federal에서 호스팅하는 Google 이메일로 이동하는 데 사용되는 웹 사이트에서 사용자 비밀번호를 신속하게 가져와 해독
    - HBGary Federal의 이메일 7만 여 건 유출
- 데이터 삭제
    - HBGary Federal의 웹 사이트를 손상시키고 백업 데이터를 삭제
- 계정 차단
    - Greg Hoglund의 rootkit.com 사이트를 인수 및 암호 변경 통해 두 회사의 전자 메일 계정 차단

### 1-3) HBGary의 윤리적 문제로 인한 이미지 실추 
- HBGary는 Wikileaks에게 해킹을 시도
- Wikileaks에 가짜문서를 배포함으로서 지지자 신뢰를 잃게함
- 다른 두개의 보안회사(Palantir, Berico Technologie)와 사이버 공격 협력 제의
- Anonymous는 HBGary의 음모를 파악하고 역으로 공격
- 공격 후 메일을 공개함으로써 CEO 자리를 물러가게 했음

## 2) 피해를 유발한 취약점<sup id="5"><a href="https://arstechnica.com/tech-policy/2011/02/anonymous-speaks-the-inside-story-of-the-hbgary-hack/">5</a></sup>

### 2-1) 회사 웹페이지의 SQL Injection 취약점을 통한 DB 내용 누출
- hbgaryfederal.com 의 CMS는 뉴스기사를 검색해 보기 위해 매개 변수를 사용하였고 데이터를 SQL DB에 저장, 쿼리를 통해 데이터 가져옴
    - SQL 쿼리에 담기는 파라미터 처리과정에서 코드의 결함이 있어 **SQL Injection**이 가능.
    - hbgaryfederal.com 을 익스플로잇 하는데 사용된 URL 

        `www.hbgaryfederal.com/pages.php?pageNav=2&page=27`
    
        위 URL의 pageNav, page 파라미터가 안전하게 처리되지 않아 해커들은 **Database에서 여러 정보 탈취** 가능

### 2-2) 취약한 알고리즘 사용(MD5)과 Password Hash값을 해독하는 Rainbow tables
- 해당 사이트에서 **해시 알고리즘으로 MD5를 채택**
    - HBgary는 해당 사이트에서 Password를 저장할 때 MD5 알고리즘을 적용
    - salting이나 반복해싱을 적용하지 않았다. → **레인보우 테이블 공격 취약**
    - 다행히도 레인보우 테이블의 key limitation 때문에 복잡한 암호는 안전
    
    문제는 HBgary는 매우 단순한 암호를 사용했고 공격자는 쉽게 **평문화된 Password를 탈취** 

### 2-3) 계정 관련 관리적 취약점
- CMS 비밀번호가 CEO 및 COO의 이메일, 트위터 및 링크를 포함한 여러 계정에도 사용
	
    → 침입자는 해킹한 Aron의구글메일을 통해  Greg의 계정에 접근        

- 취약하게 구성된 비밀번호를 사용하였고, 해당 비밀번호를 수많은 다른 서비스(플랫폼)에 사용한 것도 모자라 기술지원용 리눅스 머신에도 사용
    </br><sub>(Gyunka, Benjamin & Abikoye, Oluwakemi. (2017). Analysis of Human Factors in Cyber Security: A Case Study of Anonymous Attack on Hbgary. Computing and Information Systems Journal. 21. 10-18. )</sub>

- HBGary와 HBGary Federal 두 회사 모두 동일한 구글 앱스 계정을 사용하여 피해가 가중<sup id="6"><a href="https://krebsonsecurity.com/2011/02/hbgary-federal-hacked-by-anonymous/">6</a></sup>

### 2-4) 1-day attack
- 기술지원용 리눅스 머신(support.hbgary.com)에도 큰 허점이 있었는데, 지속적으로 **서버 OS 버전을 업데이트** 하지 않아 1day 권한 상승 익스플로잇이 사용되어 해커들이 루트 권한을 획득
    </br><sub>(Gyunka, Benjamin & Abikoye, Oluwakemi. (2017). Analysis of Human Factors in Cyber Security: A Case Study of Anonymous Attack on Hbgary. Computing and Information Systems Journal. 21. 10-18. )</sub>

### 2-4-1) 권한 상승 추가 설명<sup id="7"><a href="https://www.heise.de/security/meldung/Root-Rechte-durch-Schwachstelle-im-GNU-C-Loader-Update-1110035.html">7</a></sup>
- GNU C로더의 보안 취약점
    - 이에 취약한 Linux 시스템 운영을 통해 인가되지 않은 사용자가 시스템에서 루트 권한을 획득한 후 백업 및 연구 데이터에 접근
    - GNU-C 라이브러리의 특정 런타임 보호 메커니즘은 `argv[0]` 및 역 추적 정보를 인쇄함으로써 프로세스 메모리에서 민감한 정보 획득
    - CVE-2010-3192<sup id="8"><a href="https://www.cvedetails.com/cve/CVE-2010-3192/">8</a></sup>
- Set-UID 프로그램
    - 구현시, 스택 기반 버퍼 오버플로우 오류 포함
    - Set-UID 권한으로 동적 링크 라이브러리를 로드하면 라이브러리에서 루트 권한으로 실행
    - 하드 링크, 파일 디스크립터 리디렉션 및 환경 변수 설정을 포함한 다양한 트릭을 통해 루트 권한을 얻을 수 있음

## 3) 침해 사고 이전 이후 대응의 문제

### 3-1) 관리적 문제  
- 지속적인 보안 서버 패치를 하지 않았고 결국엔 침해사고를 일으킴 
	
### 3-2) 침해사고 이후 사후관리
- 침해사고가 일어나고 자신들이 잘못한 점과 피해사례를 알고 보완을 했어야 했는데 미온적인 태도를 보였음
- 침해 사고 이유 대응(정책 보완)이 미흡

---

### Reference

<sup><a href="#1">1</a></sup> https://youtu.be/PAFM9jSIXEQ (blackhat 2007의 Active Reversing 개요)

<sup><a href="#2">2</a></sup> https://www.blackhat.com/presentations/bh-usa-07/Hoglund/Presentation/bh-usa-07-hoglund.pdf

<sup><a href="#3">3</a></sup> https://wikileaks.org/hbgary-emails//fileid/57263/15937

<sup><a href="#4">4</a></sup> https://arstechnica.com/tech-policy/2011/02/virtually-face-to-face-when-aaron-barr-met-anonymous/

<sup><a href="#5">5</a></sup> https://arstechnica.com/tech-policy/2011/02/anonymous-speaks-the-inside-story-of-the-hbgary-hack/

<sup><a href="#6">6</a></sup> https://krebsonsecurity.com/2011/02/hbgary-federal-hacked-by-anonymous/ 

<sup><a href="#7">7</a></sup> https://www.heise.de/security/meldung/Root-Rechte-durch-Schwachstelle-im-GNU-C-Loader-Update-1110035.html

<sup><a href="#8">8</a></sup> https://www.cvedetails.com/cve/CVE-2010-3192/

---
