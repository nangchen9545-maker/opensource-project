# Pet Care

## Revision date

03/18/2026  
03/26/2026

## Revision history

| Version # | Description |
|---|---|
| 0.01 | First Draft |
| 0.02 | 초안 수정 및 검토 |

---

## Contents

1. Business purpose  
2. System context diagram  
3. Use case list  
4. Concept of operation  
5. Problem statement  
6. Glossary  
7. References

---

## 1. Business purpose

- Project background, motivation, Goal, Target market etc.
- 12pt, 160%

현 시대에 반려동물과 함께 지내거나 사는 사람들이 많다. 이런 사람들 대부분 반려동물들이 처음에 병에 걸리거나 아프거나 혹은 예방접종 등 해야할 일들이 많은 것으로 알고 있다. 기존 반려동물 어플이나 앱 같은 경우 품종에 관계 없이 거의 모든 동물들에게 동일한 가이드를 제공해준다는 것이다. 그리고 보호자들은 예방접종 이력이나 일정, 복약 이력, 증상, 병원 방문 기록 등을 정확하게 기억하기가 힘들다.

이것을 개선하기 위해 **Pet Care**는 반려동물의 건강 상태를 계속 기록하고 필요한 시점에 동물병원 진료나 맞춤형 예방접종 등을 지원하는 서비스이다. 이 시스템은 단순한 필기 기록이 아닌 반려동물의 품종, 나이, 체중, 예방접종 이력, 먹었던 약 등을 종합적으로 관리하여 맞춤형 반려동물 건강 관리를 제공하는 것과, 어디에 어떤 병원이 있는지를 보호자의 위치에 기반하여 병원 추천 등을 목표로 한다.

이 시스템은 다양한 반려동물을 키우는 모든 사람들을 타겟으로 하며, 반려동물의 건강 데이터를 체계적으로 저장·관리하고 병원 예약을 간편하게 하며, 품종·나이 등을 포함한 맞춤형 건강 가이드 제공과 예방, 보호자와 병원 간의 위치를 기반으로 최적의 병원을 추천하는 것을 목적으로 둔다. 이 시스템으로 인해 보호자는 반려동물의 상태 변화를 장기적으로 추적 가능하며, 병원은 정확한 정보를 통하여 효율적인 진료 준비가 가능해진다. 또한 맞춤형 알림 및 가이드를 통해 질병 예방과 초기 발견 가능성을 늘려 반려동물의 건강을 책임질 수 있다.

---

## 2. System context diagram

- Build a diagram to show the relationships between “System” and “Users”.
- Context models are used to illustrate the operational context of a system. They show what lies outside the system boundaries.
- Make your description for the terms in the diagram.
- 12pt, 160%

### 구성 요소

- **시스템 관리자**: breed-specific vaccine DB를 관리하고 system maintenance를 수행한다.
- **수의사**: check-up results를 업데이트하고 medical insights를 제공한다.
- **반려인**: 반려동물을 등록하고 건강 기록을 입력하며 예약을 진행하고 알림을 받는다.
- **결제 게이트**: 병원 예약에 대한 secure payments 및 settlements를 처리한다.
- **동물병원**: nearby veterinary hospitals의 위치 및 서비스 데이터를 제공한다.
- **백신정보**: standard vaccination schedules를 제공한다.
- **반려동물 건강관리 시스템**: 전체 정보를 통합 관리하는 핵심 시스템.

---

## 3. Use case list

- Find use cases in your project.
- Make your short description for each use case (table type).
- 12pt, 160%

| No. | Use Case | Actor | Description |
|---|---|---|---|
| 1 | Login | User | 사용자가 시스템에 가입하고 로그인한다. |
| 2 | Register Pet Information | User | 반려동물의 기본 정보를 등록한다. |
| 3 | Health record creation | User | 식사, 체중, 증상, 복약, 배변, 병원기록 등의 기록을 입력한다. |
| 4 | Health record Inquiry | User, Hospital | 저장된 건강 기록을 조회한다. |
| 5 | Check vaccination schedule | User | 접종 예정일 및 이력을 확인한다. |
| 6 | Hospital search | User | 지역, 진료 과목, 운영 시간에 따라 병원을 검색한다. |
| 7 | Hospital appointment application | User | 원하는 날짜와 시간으로 예약을 요청한다. |
| 8 | Reservation Approval/Rejection | Hospital | 병원이 예약을 승인하거나 조정한다. |
| 9 | Send reservation reminder | System | 예약 확정 및 일정 알림을 사용자에게 전달한다. |
| 10 | Providing personalized health guides | System | 품종 및 건강 상태에 따른 가이드를 제공한다. |
| 11 | Hospital Information Management | Admin | 병원 계정 및 운영 정보를 관리한다. |
| 12 | User Inforamation Management | Admin | 사용자 계정 및 데이터 상태를 관리한다. |

---

## 4. Concept of operation

- Describe how to operate the use cases (table type).
- 12pt, 160%

### 1) Login

- **Purpose**: 앱을 사용하기 위해 등록된 사용자인지 확인
- **Approach**: 사용자가 앱 실행 후 회원 가입 후 로그인 시 ID, PW를 입력하고 로그인 요청을 하면 서버에서 회원 정보를 조회한 뒤 로그인 성공/실패 여부를 확인한다.
- **Dynamics**: 앱 실행 시 로그인할 경우
- **Goals**: 로그인 기능을 구현한다.

### 2) Register Pet Information

- **Purpose**: 보호자가 어떤 반려동물을 키우는지 등록
- **Approach**: 사용자가 앱 실행 후 반려동물에 대한 기본정보(품종, 나이 등)를 입력 후 서버에 저장한다.
- **Dynamics**: 앱 실행 후 반려동물을 등록할 때
- **Goals**: 반려동물 정보 저장을 구현한다.

### 3) Health record creation

- **Purpose**: 반려동물의 체중, 증상, 복약, 병원기록 등을 등록
- **Approach**: 사용자가 반려동물 정보 입력 후 세부사항을 입력하여 서버에 저장한다.
- **Dynamics**: 앱 실행 후 세부사항 입력할 경우
- **Goals**: 반려동물에 대한 세부사항 정보 저장을 구현한다.

### 4) Health record Inquiry

- **Purpose**: 보호자와 병원이 반려동물의 건강기록 조회
- **Approach**: 앱 실행 후 조회를 누르면 반려동물에 대한 기본정보와 세부사항을 조회할 수 있게 한다.
- **Dynamics**: 조회를 누르는 경우
- **Goals**: 반려동물의 기본정보, 세부사항을 조회할 수 있게 한다.

### 5) Check vaccination schedule

- **Purpose**: 보호자가 반려동물의 예방접종 예정일, 기록 등을 조회한다.
- **Approach**: 사용자가 예방접종 예정일과 기록을 조회할 수 있게 한다.
- **Dynamics**: 앱 실행 후 예방접종 및 조회를 누르는 경우
- **Goals**: 반려동물의 예방접종 예정일, 기록, 예방접종 등을 조회할 수 있게 한다.

### 6) Hospital search

- **Purpose**: 보호자가 근처의 병원을 빠르게 찾을 수 있도록 한다.
- **Approach**: 사용자 위치를 기반으로 하여 근처 지역, 진료과목, 운영시간에 따른 동물병원을 몇 곳 추천해준다.
- **Dynamics**: 근처 동물병원을 찾을 경우
- **Goals**: GPS 기반 동물병원 추천을 진행한다.

### 7) Hospital appointment application

- **Purpose**: 보호자가 빠르게 동물병원을 예약할 수 있게 한다.
- **Approach**: 사용자가 원하는 날짜, 시간, 의사로 예약을 요청할 수 있도록 도와준다.
- **Dynamics**: 사용자가 동물병원 진료를 원할 경우
- **Goals**: 동물병원 진료 예약을 진행한다.

### 8) Reservation Approval/Rejection

- **Purpose**: 동물병원에서 보호자가 요청한 진료에 대해 예약을 승인하거나 거절할 수 있도록 한다. 동물병원에서 보호자가 필요로 하는 치료가 자신의 분야와 일치하는지 본다.
- **Approach**: 예약을 승인하거나 조정, 거절한다.
- **Dynamics**: 보호자가 예약을 진행 후 병원에서 진료를 결정할 경우
- **Goals**: 진료 예약 승인/거절을 보내 보호자에게 알린다.

### 9) Send reservation reminder

- **Purpose**: 시스템에서 동물병원이 보호자에게 진료 예약이 승인/거절/조정 났는지를 확정 및 일정 알림으로 전달한다.
- **Approach**: 동물병원에서 진료 예약을 확정/거절/일정 조정의 답을 보내면 시스템에서 보호자에게 알림이 가도록 한다.
- **Dynamics**: 진료 확정 및 일정이 정해졌을 경우
- **Goals**: 알림 발송을 구현한다.

### 10) Providing personalized health guides

- **Purpose**: 보호자가 키우는 반려동물의 맞춤형 서비스를 제공한다.
- **Approach**: 사용자가 반려동물의 품종 및 건강 상태에 따라 가이드를 제공하여 편리하고 안정적으로 반려동물을 키울 수 있도록 한다.
- **Dynamics**: 사용자가 ‘가이드’를 눌렀을 경우
- **Goals**: 품종 및 다른 세부사항과 함께 가이드라인을 제공한다.

### 11) Hospital Information Management

- **Purpose**: 관리자가 병원 계정과 운영정보를 관리한다.
- **Approach**: 관리자가 병원 및 운영정보가 새어나가지 않도록 관리한다.
- **Dynamics**: 위약정책에 동의를 누른 경우
- **Goals**: 병원의 정보 및 운영방법을 노출시키지 않는다.

### 12) User Inforamation Management

- **Purpose**: 관리자가 사용자의 계정 및 데이터 상태를 관리한다.
- **Approach**: 어떤 일로 인해 사용자가 로그아웃되었을 때 데이터를 보존하기 위하도록 한다.
- **Dynamics**: 관리자가 데이터 설정을 하는 경우
- **Goals**: 사용자 데이터를 저장한다.

---

## 5. Problem statement

- Describe the problems the project should be considered (including technical difficulties).
- Describe the Non-Functional Requirements (NFRs).
- 12pt, 160%

기존의 반려동물 관련 서비스들은 대부분의 앱들이 단순한 다이어리 기능, 즉 기록만 하는 기능에 머물러져 있다. 사용자는 식사, 산책 등을 기록하지만 이러한 기록이 반려동물에 대한 실제 의료 관리나 병원 방문과 유기적으로 연결되지 않는다. 또한 품종별 특성을 고려한 관리 역시 충분하지 않다.

요즘 다양한 반려동물을 키우는 사람들이 많고 품종마다 다르게 케어를 해줘야 하는데, 대부분 품종과 관계없이 동일한 관리 가이드를 제공하는 경우가 많다. 그리고 병원 예약 역시 보호자들이 전화나 별도의 플랫폼을 통해 예약해야 하므로 불편을 겪는다. 병원에 방문했을 때도 예방접종, 복약, 정기검진 등 중요한 관리를 보호자들이 하나하나 다 기억하기 힘들고, 이를 놓쳤을 때 반려동물의 질병 예방 실패 및 치료 지연으로 이어질 수 있다.

위 문제들을 해결하기 위해서는 첫 번째로 종, 품종, 연령 등을 기반으로 반려동물 맞춤형 건강 가이드를 제공하고, 건강 기록과 병원 예약을 하나의 플랫폼으로 통합 관리할 수 있도록 만들어야 한다. 또한 예방접종, 복약 등 검진 일정에 대해 알림 기능을 제공하고 병원 측이 진료 전 건강 기록을 확인할 수 있게 하여 진료의 효율성을 늘린다. 보호자들은 장기적인 건강 데이터를 통해 반려동물의 상태 변화를 추적하여 반려동물의 건강 상태를 더욱 잘 관리할 수 있도록 한다.

현 시대에 반려동물을 키우는 보호자들이 많아지고 있으므로 더 체계적이고 전문적인 반려동물 건강 관리가 필요하며, 특히 반려동물을 가족 구성원으로 생각하는 인식이 늘어나고 있으므로 단순 기록과 같은 전문성이 낮은 서비스보다 의료 연계형 건강 관리 시스템이 필요해지고 있다.

---

## 6. Glossary

- Specifically describe all of the terms used in this documents.
- 12pt, 160%

| Term | Description |
|---|---|
| Pet Owner | 반려동물을 기르며 시스템을 사용하는 보호자 |
| Veterinary Hospital | 반려동물 진료를 제공하는 동물병원 |
| Health Record | 반려동물 식사, 체중, 증상, 복약 등을 기록 |
| Reservation | 병원 진료를 위해 일정과 시간을 신청하는 절차 |
| Vaccination Schedule | 예방접종 예정일과 이력을 관리하는 정보 |
| Customized Guide | 반려동물 품종, 나이 등을 기반으로 제공되는 맞춤형 관리 가이드 |
| Notification | 예약, 접종, 복약 등의 일정을 사용자에게 알려주는 기능 |
| Symptom Log | 보호자가 기록하는 반려동물의 증상 메모 |

---

## 7. References

- Describe all of your references (book, paper, technical report etc).
- 12pt, 160%
