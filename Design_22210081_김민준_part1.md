# PetCare\_Design\_Part1



<!-- Page 1 -->

Pet Care - Design Yeungnam University
3. Design
Pet Care
반려동물 건강 기록 및 병원 예약 통합 시스템
Student No Name E-Mail
Object-Oriented Design / Software Engineering Project

* 1 -



<!-- Page 2 -->

Pet Care - Design Yeungnam University
\[ Revision history ]
Revision date Version # Description Author
03/18/2026 0.01 Conceptualization First Draft
03/26/2026 0.02 초안 수정 및 검토
04/28/2026 1.00 Analysis 문서 재구성 및 상세 설계 보완
06/05/2026 1.00 Design 문서 작성, UML/Sequence/State/ERD 설계 반영

* 2 -



<!-- Page 3 -->

Pet Care - Design Yeungnam University
= Contents =

1. Introduction ..........................................................................................
2. Class diagram ..........................................................................................
3. Sequence diagram ..........................................................................................
4. State machine diagram ..........................................................................................
5. Implementation requirements ..........................................................................................
6. Glossary ..........................................................................................
7. References ..........................................................................................
* 3 -



<!-- Page 4 -->

Pet Care - Design Yeungnam University

1. Introduction
본 문서는 Pet Care 시스템의 설계 단계 산출물이다. Pet Care는 반려동물 보호자가 건강 상태, 예방접종 이력,
복약 기록, 증상 기록, 병원 방문 기록, 예약 정보를 하나의 시스템에서 관리하도록 돕는 서비스이다. Analysis
단계에서 정의된 기능 요구사항을 실제 구현 가능한 객체 구조, 데이터 구조, 상호작용 흐름, 상태 전이 구조로
구체화하는 것이 본 문서의 목적이다.
설계의 핵심 방향은 첫째, 건강 기록과 병원 예약을 하나의 플랫폼으로 통합하는 것이다. 보호자는 반려동물의
기본 정보와 건강 데이터를 누적하고, 병원은 예약 전에 필요한 정보를 확인하여 진료 준비 시간을 줄일 수
있다. 둘째, 종과 품종, 나이, 체중, 접종 이력을 반영한 맞춤형 관리 가이드를 제공하는 것이다. 셋째, 위치 기반
병원 검색과 예약 승인/거절 알림을 통해 보호자와 병원의 연결 과정을 단순화하는 것이다.
객체지향 설계 관점에서는 Pet을 중심 도메인 객체로 두고 Dog, Cat, Hamster와 같은 하위 클래스를 통해
종별 특성을 확장한다. 예방접종 일정 계산은 VaccinationStrategy 인터페이스로 분리하여 종별 정책이
바뀌어도 핵심 서비스 로직을 수정하지 않도록 설계한다. 예약 상태는 Pending, Confirmed, Rejected,
Cancelled와 같은 상태 객체로 분리하여 State Pattern을 적용한다.
본 문서의 구성은 Design 표준 양식에 맞춰 Introduction, Class diagram, Sequence diagram, State
machine diagram, Implementation requirements, Glossary, References 순서로 작성하였다. 각
다이어그램 뒤에는 설계 의도와 구현 시 고려사항을 표와 설명으로 함께 제시하여, 단순한 그림이 아니라 실제
개발에 연결 가능한 설계 문서가 되도록 구성하였다.
Figure 1은 Pet Care 시스템의 전체 운영 구조를 나타낸다. 사용자 앱, 병원 웹, 관리자 콘솔이 Application Server를 통해 DB,
백신 데이터, 위치/푸시/결제 외부 서비스와 연결된다.
* 4 -



<!-- Page 5 -->

Pet Care - Design Yeungnam University
1.1 Design Scope
구분 설계 내용 반영 기능
사용자 영역 회원 인증, 반려동물 등록, 건강 기록 작성/조회, Login, Register Pet, Create/View Record, Search
병원 검색, 예약 신청 Hospital, Request Reservation
병원 영역 예약 승인/거절/시간 조정, 진료 전 건강 기록 조회 Approve/Reject Reservation, Inquiry Health
Record
관리자 영역 사용자 정보, 병원 정보, 백신 DB, 시스템 운영 관리 Manage User, Manage Hospital, Update Vaccine
DB
공통 영역 알림, 권한 검증, 데이터 저장, 상태 관리 Notification, Authorization, State Pattern
설계 범위는 모바일/웹 클라이언트, 서버 애플리케이션, 관계형 데이터베이스, 외부 위치/푸시 서비스까지
포함한다. 실제 구현에서는 각 영역을 독립된 모듈로 분리하여 기능 추가와 유지보수가 쉬운 구조를 목표로
한다.
비기능 요구사항으로는 3초 이내의 주요 조회/저장 응답, 인증된 사용자만 접근 가능한 건강 기록 보호, 예약 및
알림의 안정성, 직관적인 UI, 동물 종류와 병원 수 증가에 대응 가능한 확장성을 고려한다.

* 5 -



<!-- Page 6 -->

Pet Care - Design Yeungnam University
2. Class diagram
아래 클래스 다이어그램은 Pet Care 시스템의 핵심 도메인 객체와 관계를 나타낸다. User는 공통 계정 정보를
관리하고, PetOwner와 Admin은 역할별 기능을 확장한다. Pet은 건강 기록, 예약, 예방접종 일정의 중심
객체이며 Dog, Cat, Hamster는 종별 정책을 오버라이딩한다.
Figure 2. Pet Care Class Diagram

* 6 -



<!-- Page 7 -->

Pet Care - Design Yeungnam University
2.1 Class Responsibility Table
Class Responsibility Attributes Main Methods
User 공통 계정, 연락처, 권한 관리 userId, name, phone, role, login(), logout(), viewRecord()
passwordHash
PetOwner 보호자 기능 수행 ownerId, pets, reservations registerPet(), createRecord(),
searchHospital(),
requestReservation()
Admin 운영 정보 관리 adminId, permissionLevel manageUser(),
manageHospital(),
updateVaccineDB()
Pet 반려동물 프로필의 중심 객체 petId, name, species, breed, age, updateProfile(), getGuide(),
weight, sex getVaccineSchedule()
Dog/Cat/Hamster 종별 특성 확장 riskDiseaseList, requiredVaccines getVaccineList(),
getRiskDisease()
HealthRecord 날짜별 건강 기록 저장 recordId, date, weight, symptom, create(), update(), inquiry(),
medicine, memo attachFile()

* 7 -



<!-- Page 8 -->

Pet Care - Design Yeungnam University
2.1 Class Responsibility Table - Continued
Class Responsibility Attributes Main Methods
Hospital 병원 정보 및 예약 처리 hospitalId, name, location, service, search(), approveReservation(),
openHours rejectReservation()
Reservation 예약 날짜, 시간, 상태 관리 reservationId, date, time, reason, request(), approve(), reject(),
status cancel()
Notification 예약/접종/복약 알림 생성 notificationId, type, message, send(), markRead(), retry()
targetTime
VaccinationStrate 종별 예방접종 계산 정책 species, ruleSet calculateSchedule(), nextDose()
gy
ReservationState 예약 상태별 행동 정의 stateName approve(), reject(), cancel(),
notifyUser()
Pet 클래스는 본 시스템의 핵심 객체이다. 건강 기록과 예방접종 일정, 예약 정보는 모두 특정 반려동물에
연결되므로 Pet 객체의 식별자와 프로필 데이터가 정확히 관리되어야 한다.
HealthRecord는 식사, 체중, 증상, 복약, 배변, 병원 방문 기록을 모두 저장할 수 있도록 확장 가능한 기록
유형을 가진다. 기록 유형이 추가되더라도 기존 테이블과 서비스 구조를 크게 수정하지 않도록 recordType과
detail 구조를 분리한다.
Reservation은 병원 예약 신청부터 승인, 거절, 취소까지의 상태 흐름을 가진다. 상태별 동작을 if-else로
분산시키지 않고 ReservationState 계층으로 분리하면 예약 정책 변경 시 유지보수가 쉬워진다.

* 8 -



<!-- Page 9 -->

Pet Care - Design Yeungnam University
2.2 Object-Oriented Pattern Design
Pattern 적용 위치 설계 이유 예상 효과
Abstract Class Pet -> Dog/Cat/Hamster 공통 프로필과 종별 기능을 동시에 종 추가 시 기존 코드 수정
표현 최소화
Strategy Pattern VaccinationStrategy 종별 예방접종 계산 로직 분리 개/고양이/햄스터별 일정 정책
교체 가능
State Pattern ReservationState 예약 상태에 따른 동작 분리 승인/거절/취소 흐름 명확화
Repository Pattern User/Pet/Record/Reservatio DB 접근 로직 분리 서비스 로직과 저장소 로직
n Repository 결합도 감소
DTO Pattern Request/Response DTO 클라이언트 입력값과 내부 엔티티 분리 보안 및 검증 용이
Strategy Pattern은 예방접종 일정 계산에 적합하다. 종별 백신 종류와 권장 주기가 다르기 때문에 하나의
메서드에 모든 조건을 넣으면 코드가 복잡해진다. VaccinationStrategy 인터페이스를 만들고
DogVaccinationStrategy, CatVaccinationStrategy, HamsterVaccinationStrategy를 구현하면 종별
로직을 독립적으로 관리할 수 있다.
State Pattern은 예약 상태 관리에 적합하다. 예약은 Pending에서 Confirmed 또는 Rejected로 이동하고,
보호자 또는 병원 사정에 따라 Cancelled로 이동할 수 있다. 상태 객체가 현재 상태에서 가능한 행동만
허용하면 잘못된 상태 전이를 줄일 수 있다.

* 9 -



<!-- Page 10 -->

Pet Care - Design Yeungnam University
2.3 Database and ERD Design
Figure 3. Pet Care ERD
Relation Cardinality Description
USER - PET 1:N 한 명의 보호자는 여러 반려동물을 등록할 수 있다.
PET - HEALTH\_RECORD 1:N 한 반려동물은 여러 날짜의 건강 기록을 가진다.
PET - RESERVATION 1:N 한 반려동물은 여러 병원 예약 이력을 가질 수 있다.
HOSPITAL - RESERVATION 1:N 한 병원은 여러 예약 요청을 처리한다.
VACCINE\_SCHEDULE - PET Lookup 반려동물의 종과 나이에 따라 권장 접종 일정을 조회한다.

* 10 -



<!-- Page 11 -->

Pet Care - Design Yeungnam University
3. Sequence diagram
시퀀스 다이어그램은 Conceptualization 및 Analysis 단계에서 정의한 주요 Use Case를 실제 객체 간 메시지
흐름으로 전개한 것이다. 각 기능은 사용자 입력, 서버 검증, 데이터베이스 처리, 응답 및 알림 단계로 구성된다.

* 11 -



<!-- Page 12 -->

Pet Care - Design Yeungnam University
3.1 Login
사용자가 ID와 Password를 입력하면 App이 Server에 인증을 요청한다. Server는 User DB에서 계정과
권한을 확인하고 성공 시 역할별 메인 화면으로 이동한다. 실패 시 회원가입 안내, 비밀번호 오류, 네트워크
오류 메시지를 반환한다.
설계 항목 내용
Primary Actor Pet Owner
Precondition 로그인 및 필요한 기본 정보 등록 완료
Success Result 요청 처리 결과가 DB에 저장되고 화면 또는 알림으로 사용자에게 전달됨
Exception 입력값 오류, 권한 만료, 네트워크 오류, 예약 시간 마감 시 예외 응답 제공

* 12 -



<!-- Page 13 -->

Pet Care - Design Yeungnam University
3.2 Register Pet Information
보호자가 반려동물의 이름, 종, 품종, 나이, 체중, 성별을 입력하면 시스템은 필수값과 형식을 검증한 뒤 Pet
DB에 저장한다. 저장 후 대시보드에 등록된 반려동물 프로필이 표시된다.
설계 항목 내용
Primary Actor Pet Owner
Precondition 로그인 및 필요한 기본 정보 등록 완료
Success Result 요청 처리 결과가 DB에 저장되고 화면 또는 알림으로 사용자에게 전달됨
Exception 입력값 오류, 권한 만료, 네트워크 오류, 예약 시간 마감 시 예외 응답 제공

* 13 -



<!-- Page 14 -->

Pet Care - Design Yeungnam University
3.3 Create Health Record
보호자는 식사, 체중, 증상, 복약, 배변, 병원 방문 기록 중 하나를 선택해 입력한다. 서버는 해당 기록을
반려동물 ID와 날짜에 연결해 저장하고, 이후 보호자와 권한이 있는 병원이 조회할 수 있도록 한다.
설계 항목 내용
Primary Actor Pet Owner
Precondition 로그인 및 필요한 기본 정보 등록 완료
Success Result 요청 처리 결과가 DB에 저장되고 화면 또는 알림으로 사용자에게 전달됨
Exception 입력값 오류, 권한 만료, 네트워크 오류, 예약 시간 마감 시 예외 응답 제공

* 14 -



<!-- Page 15 -->

Pet Care - Design Yeungnam University
3.4 Check Vaccination Schedule
시스템은 Pet의 종, 품종, 나이와 접종 이력을 기준으로 Vaccine DB를 조회한다. 권장 일정과 실제 접종
이력을 비교하여 예정 접종, 완료 접종, 지연 접종 상태를 계산하고 알림을 예약한다.
설계 항목 내용
Primary Actor Pet Owner
Precondition 로그인 및 필요한 기본 정보 등록 완료
Success Result 요청 처리 결과가 DB에 저장되고 화면 또는 알림으로 사용자에게 전달됨
Exception 입력값 오류, 권한 만료, 네트워크 오류, 예약 시간 마감 시 예외 응답 제공

* 15 -

