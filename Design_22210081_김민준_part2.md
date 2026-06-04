# PetCare\_Design\_Part2



<!-- Page 16 -->

Pet Care - Design Yeungnam University
3.5 Search Hospital
사용자가 병원 검색을 선택하면 App은 GPS 위치 또는 직접 입력 지역을 기반으로 서버에 검색 조건을
전달한다. 서버는 병원 DB에서 위치, 운영시간, 진료과목 기준으로 결과를 정렬해 반환한다.
설계 항목 내용
Primary Actor Pet Owner
Precondition 로그인 및 필요한 기본 정보 등록 완료
Success Result 요청 처리 결과가 DB에 저장되고 화면 또는 알림으로 사용자에게 전달됨
Exception 입력값 오류, 권한 만료, 네트워크 오류, 예약 시간 마감 시 예외 응답 제공

* 16 -



<!-- Page 17 -->

Pet Care - Design Yeungnam University
3.6 Request Reservation
보호자는 병원, 날짜, 시간, 진료 사유, 공유할 건강 기록 범위를 선택한다. 서버는 예약 가능 여부와 필수값을
확인한 뒤 병원에 요청을 전달하고, 병원의 승인/거절/조정 결과를 알림으로 전송한다.
설계 항목 내용
Primary Actor Pet Owner
Precondition 로그인 및 필요한 기본 정보 등록 완료
Success Result 요청 처리 결과가 DB에 저장되고 화면 또는 알림으로 사용자에게 전달됨
Exception 입력값 오류, 권한 만료, 네트워크 오류, 예약 시간 마감 시 예외 응답 제공

* 17 -



<!-- Page 18 -->

Pet Care - Design Yeungnam University
3.7 Approve or Reject Reservation
병원은 예약 요청 목록을 확인하고 진료 사유와 공유된 건강 기록을 검토한다. 승인, 거절, 시간 조정 중 하나를
선택하면 서버가 예약 상태를 변경하고 보호자에게 알림을 전송한다.
설계 항목 내용
Primary Actor Veterinary Hospital
Precondition 로그인 및 필요한 기본 정보 등록 완료
Success Result 요청 처리 결과가 DB에 저장되고 화면 또는 알림으로 사용자에게 전달됨
Exception 입력값 오류, 권한 만료, 네트워크 오류, 예약 시간 마감 시 예외 응답 제공

* 18 -



<!-- Page 19 -->

Pet Care - Design Yeungnam University
4. State machine diagram
상태 머신 다이어그램은 클라이언트와 서버가 요청 처리 과정에서 어떤 상태를 거치는지 표현한다.
클라이언트는 사용자의 화면 이동과 알림 수신을 중심으로 설계하고, 서버는 요청 수신, 검증, 처리, 저장, 알림,
응답 흐름을 중심으로 설계한다.

* 19 -



<!-- Page 20 -->

Pet Care - Design Yeungnam University
4.1 Client State Machine Diagram
클라이언트는 Idle 상태에서 앱 실행 시 Login으로 이동한다. 인증 성공 후 Dashboard에 진입하고, 사용자는
건강 기록 작성, 병원 검색, 예약 신청, 알림 확인 화면으로 이동할 수 있다. 각 기능 처리 후에는 Dashboard로
돌아와 최신 상태를 확인한다.
State Description
Idle 앱 실행 전 또는 백그라운드 상태
Login 사용자 인증 진행 상태
Dashboard 반려동물 요약 정보, 다음 접종일, 최근 증상 표시
Record 건강 기록 작성/조회 상태
Search 위치 기반 병원 검색 상태
Reservation 예약 신청 및 상태 확인 상태
Notification 예약/접종/복약 알림 확인 상태

* 20 -



<!-- Page 21 -->

Pet Care - Design Yeungnam University
4.2 Server State Machine Diagram
서버는 Waiting 상태에서 요청을 기다린다. 요청이 들어오면 Validate 상태에서 인증, 권한, 필수값을
검사한다. 정상 요청은 Process로 이동하여 비즈니스 로직을 수행하고, 저장이 필요한 경우 DB Save로
이동한다. 알림이 필요한 경우 Notify를 거쳐 Response를 반환한다.
State Description
Waiting 클라이언트 요청 대기
Validate 토큰, 권한, 입력값, 예약 가능 여부 검사
Process 로그인, 기록 저장, 병원 검색, 예약 처리 등 핵심 로직 수행
DB Save 사용자, 반려동물, 기록, 예약 상태 저장
Notify 푸시 알림 또는 내부 알림 생성
Response 성공/실패 결과 반환
Error 검증 실패 또는 서버 오류 처리

* 21 -



<!-- Page 22 -->

Pet Care - Design Yeungnam University
4.3 Reservation State Transition Detail
Current State Event Next State Action
Pending Hospital approves Confirmed 예약 확정 저장, 보호자에게 확정 알림
전송
Pending Hospital rejects Rejected 거절 사유 저장, 보호자에게 거절 알림
전송
Pending Hospital suggests time Pending 대체 시간 제안, 보호자 확인 대기
change
Confirmed Pet Owner cancels Cancelled 취소 사유 저장, 병원에 취소 알림 전송
Confirmed Visit completed Completed 진료 결과 입력 및 건강 기록 연결
Rejected Owner searches another Search 다른 병원 검색 화면으로 이동
hospital
예약 상태 전이 설계에서 중요한 점은 현재 상태에서 허용되지 않는 이벤트를 차단하는 것이다. 예를 들어 이미
Rejected 상태인 예약은 approve 이벤트를 직접 받을 수 없고, 보호자는 다른 병원을 검색하거나 새 예약을
생성해야 한다.
상태 전이는 서버에서 최종적으로 검증해야 한다. 클라이언트 UI에서 버튼을 숨기더라도 네트워크 요청은
위조될 수 있으므로 서버의 ReservationState 객체가 가능한 전이만 수행하도록 구현한다.

* 22 -



<!-- Page 23 -->

Pet Care - Design Yeungnam University
5. Implementation requirements
Category Requirement Reason
Client Android 또는 반응형 Web UI. 보호자, 병원, 관리자 화면을 사용자와 병원이 서로 다른 목적의 기능을
역할별로 분리한다. 사용하기 때문이다.
Backend Java 17 이상, Spring Boot 기반 REST API 서버를 사용한다. 객체지향 구조와 계층형 서비스 구현에
적합하다.
Database MySQL 또는 PostgreSQL을 사용하고 FK와 Index를 건강 기록과 예약 데이터의 무결성 및 조회
설정한다. 성능이 중요하다.
Authentication JWT 기반 인증과 Role 기반 접근 제어를 적용한다. 보호자 건강 기록은 인증된 사용자와 권한
있는 병원만 접근해야 한다.
Notification Firebase Cloud Messaging 또는 동등한 Push Gateway를 예약 확정, 접종, 복약 알림을 안정적으로
사용한다. 전달하기 위함이다.
Location 지도/GPS API와 병원 위치 DB를 연동한다. 거리순 병원 검색과 운영시간 필터링이
필요하다.
Deployment Cloud VM 또는 Container 기반 배포, HTTPS 적용. 확장성과 보안성을 확보하기 위함이다.

* 23 -



<!-- Page 24 -->

Pet Care - Design Yeungnam University
5.1 Layered Architecture
Layer Components Responsibility
Presentation Layer Mobile App, Hospital Web, Admin 사용자 입력 수집, 화면 표시, 알림 확인
Console
Controller Layer AuthController, PetController, REST API 엔드포인트 제공 및 요청/응답 DTO 처리
RecordController,
ReservationController
Service Layer AuthService, PetService, 비즈니스 규칙, 상태 전이, 전략 패턴 실행
HealthRecordService,
ReservationService, NotificationService
Repository Layer UserRepository, PetRepository, DB 조회/저장/수정/삭제 처리
RecordRepository,
ReservationRepository
Infrastructure Layer DB, Push Gateway, Map API, Payment 외부 시스템 연동과 데이터 영속성 제공
Gateway
계층형 아키텍처는 Presentation, Controller, Service, Repository, Infrastructure 계층으로 구분한다. 이
구조를 사용하면 UI 변경, DB 변경, 외부 API 변경이 발생해도 영향 범위를 제한할 수 있다.
서비스 계층에는 핵심 비즈니스 규칙을 집중시킨다. 예를 들어 예약 신청 시 시간 중복 검사, 응급 증상 키워드
판단, 건강 기록 공유 권한 검사는 ReservationService가 처리한다. 예방접종 일정 계산은
VaccinationService가 Strategy 객체를 선택하여 수행한다.

* 24 -



<!-- Page 25 -->

Pet Care - Design Yeungnam University
5.2 Non-functional Requirements Mapping
NFR Design Decision Verification Method
Performance 주요 조회/저장 API 3초 이내 응답, 자주 조회되는 병원/백신 데이터 API 응답 시간 측정, 부하 테스트
캐싱
Security JWT 인증, Role 권한, 건강 기록 접근 제어, HTTPS 권한 테스트, 취약점 점검
Availability 알림 재시도 로그, 장애 시 내부 알림 보존 장애 상황 시나리오 테스트
Usability 카드형 기록 UI, 단순한 예약 단계, 명확한 오류 메시지 사용자 테스트, 화면 리뷰
Scalability Pet 하위 클래스와 Strategy 추가로 종 확장, 병원 DB 인덱싱 종 추가 테스트, 데이터 증가 테스트
Maintainability 계층형 구조, Repository/Service 분리, 상태/전략 패턴 적용 코드 리뷰, 변경 영향 분석

* 25 -



<!-- Page 26 -->

Pet Care - Design Yeungnam University
5.3 UI Design Requirements
Screen Main Components Design Note
Login ID, Password, Login Button, Join Link 오류 메시지는 입력 필드 아래에 표시한다.
Dashboard 반려동물 카드, 오늘 체중, 다음 접종일, 복약 알림, 보호자가 가장 자주 확인하는 정보를 한 화면에
최근 증상 배치한다.
Pet Profile 이름, 종, 품종, 나이, 체중, 성별, 사진 필수 입력값과 선택 입력값을 구분한다.
Health Record 기록 유형 탭, 날짜, 상세 내용, 사진 첨부 기록 유형별 입력 폼을 다르게 제공한다.
Hospital Search 지도, 목록, 거리, 운영시간, 진료과목 필터 위치 권한 거부 시 직접 지역 입력을 제공한다.
Reservation 병원, 날짜, 시간, 진료 사유, 공유 기록 범위 예약 전 최종 확인 화면을 제공한다.
Notification 예약/접종/복약/검진 알림 목록 읽음/안읽음 상태를 구분한다.
UI는 보호자의 기술 숙련도가 다양하다는 점을 고려하여 복잡한 메뉴를 줄이고, 핵심 기능을 하단
내비게이션으로 배치한다. Home, Pet, Record, Hospital, Reservation, Guide 메뉴를 기본 구조로 둔다.
병원 검색과 예약 화면은 사용자의 결정 과정을 줄이는 것이 중요하다. 병원 목록에는 거리, 운영시간, 진료
가능 항목, 예약 가능 여부를 함께 표시하여 보호자가 전화나 외부 검색 없이 바로 선택할 수 있도록 한다.

* 26 -



<!-- Page 27 -->

Pet Care - Design Yeungnam University
5.4 API and Validation Requirements
API Method Description Validation
/auth/login POST 로그인 인증 ID/PW 필수, 형식 검사
/pets POST 반려동물 등록 이름, 종, 나이, 체중 필수
/pets/{id}/records POST 건강 기록 생성 recordType, date 필수
/pets/{id}/records GET 건강 기록 조회 소유자 또는 승인된 병원 권한 필요
/vaccinations/{petId} GET 예방접종 일정 조회 Pet 정보 존재 여부 확인
/hospitals/search GET 병원 검색 위치 또는 지역명 필요
/reservations POST 예약 신청 병원, 날짜, 시간, 진료 사유 필수
/reservations/{id}/decisio PATCH 예약 승인/거절 병원 권한 및 현재 상태 확인
n

* 27 -



<!-- Page 28 -->

Pet Care - Design Yeungnam University
6. Glossary
Term Description
Pet Owner 반려동물을 기르며 Pet Care 시스템을 사용하는 보호자이다.
Veterinary Hospital 반려동물 진료를 제공하며 예약 승인/거절과 진료 결과 입력을 수행하는 병원이다.
Health Record 반려동물의 식사, 체중, 증상, 복약, 배변, 병원 방문 기록을 포함하는 건강 데이터이다.
Reservation 보호자가 병원 진료를 위해 날짜와 시간을 신청하고 병원이 처리하는 절차이다.
Vaccination Schedule 반려동물의 종, 나이, 접종 이력을 기반으로 관리되는 예방접종 일정이다.
Customized Guide 품종, 나이, 체중, 건강 상태를 기반으로 제공되는 맞춤형 관리 가이드이다.
Notification 예약, 접종, 복약, 검진 일정을 사용자에게 알려주는 기능이다.
Symptom Log 보호자가 반려동물의 이상 증상, 지속 시간, 심각도 등을 기록하는 메모이다.

* 28 -



<!-- Page 29 -->

Pet Care - Design Yeungnam University
6. Glossary - Continued
Term Description
Pending 병원 예약 요청이 접수되었지만 아직 승인 또는 거절되지 않은 상태이다.
Confirmed 병원이 예약을 승인하여 진료 일정이 확정된 상태이다.
Rejected 병원이 예약 요청을 거절한 상태이다.
Cancelled 보호자 또는 병원 사정으로 예약이 취소된 상태이다.
Strategy Pattern 알고리즘을 인터페이스로 분리하여 실행 시점에 교체할 수 있게 하는 객체지향 설계 패턴이다.
State Pattern 객체의 상태에 따라 행동을 다르게 수행하도록 상태를 클래스로 분리하는 설계 패턴이다.

* 29 -



<!-- Page 30 -->

Pet Care - Design Yeungnam University
7. References
\[1] Ian Sommerville, Software Engineering, 10th Edition, Pearson.
\[2] Martin Fowler, UML Distilled: A Brief Guide to the Standard Object Modeling Language,
Addison-Wesley.
\[3] Gamma, Helm, Johnson, Vlissides, Design Patterns: Elements of Reusable Object-Oriented
Software, Addison-Wesley.
\[4] Oracle, Java Platform Standard Edition Documentation.
\[5] Spring, Spring Boot Reference Documentation.
\[6] Firebase, Firebase Cloud Messaging Documentation.
\[7] Pet Care Analysis Document, 반려동물 건강 기록 및 병원 예약 통합 시스템, 2026.
\[8] Pet Care Conceptualization Document, 반려동물 건강 기록 및 병원 예약 통합 시스템, 2026.
본 문서는 업로드된 Conceptualization 및 Analysis 문서의 기능 요구사항, 유스케이스, 도메인 분석, 비기능
요구사항을 바탕으로 Design 표준 양식에 맞추어 재구성하였다.

* 30 -

