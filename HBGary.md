<p align=center>
    <img src="img/sample.jpg">
</p>

<p align=center style="font-size:30px"><strong>Q. 기술적으로 어떤 것이 문제 였나?</strong></p>

---

<p style="font-size:24px"><strong>1) Responder 프로그램 자체의 문제<sup>1</sup></strong></p>

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
        - project 생성 → working canvas → debugging → target process : winmine.exe , PID:5600 → dynamic analysis → "Write not yet available, waiting ... "
    - 영상 속 액션2 :
        - winmine.exe 우클릭 → package → analyze binary → bookmarks , strings, symbols 생성 → 기능 오류 (5830)

--- 

<sup>1</sup>Reference :

*https://wikileaks.org/hbgary-emails/emailid/64323*

*https://wikileaks.org/hbgary-emails/emailid/66780*

*https://wikileaks.org/hbgary-emails/emailid/57263* 첨부파일

*https://wikileaks.org/hbgary-emails/emailid/57263* → attachments → Responder Bug video.rarefe

---