
# Pet Care - Analysis

## 반려동물 건강 기록 및 병원 예약 통합 시스템

| Student No | |
|---|---|
| Name | |
| E-Mail | |

---

# Revision History

| Revision date | Version # | Description | Author |
|---|---|---|---|
| 03/18/2026 | 0.01 | Conceptualization First Draft | |
| 03/26/2026 | 0.02 | 초안 수정 및 검토 | |
| 04/28/2026 | 1.00 | Analysis 문서 재구성 및 상세 설계 보완 | |

---

# Contents

1. Introduction  
2. Use case analysis  
3. Domain analysis  
4. User Interface prototype  
5. Non-functional requirements  
6. Glossary  
7. References  

---

# 1. Introduction

## 1.1 Executive Summary

Pet Care는 반려동물을 키우는 보호자가 반려동물의 건강 상태, 예방접종 이력, 복약 기록, 증상 기록, 병원 방문 기록, 예약 정보를 하나의 시스템에서 통합적으로 관리할 수 있도록 하는 서비스이다.

기존 반려동물 앱은 단순 다이어리 기능에 머무르거나 품종과 나이에 관계없이 동일한 건강 가이드를 제공하는 경우가 많다. Pet Care는 이러한 한계를 개선하여 반려동물의 종, 품종, 나이, 체중, 건강 기록을 기반으로 맞춤형 관리 가이드를 제공하고, 위치 기반 동물병원 검색과 예약 신청까지 연결한다.

즉, 본 시스템은 보호자의 기억에 의존하던 건강 관리 과정을 데이터 기반으로 전환하고, 병원은 진료 전에 필요한 정보를 확인하여 더 효율적으로 진료를 준비할 수 있도록 지원한다.

결과적으로 보호자는 질병 예방과 초기 발견 가능성을 높일 수 있고, 병원은 예약과 진료 정보를 체계적으로 관리할 수 있다.

---

# 2. Use case analysis

## 2.1 Use Case List

| Use Case Name | ID | Korean Name | Primary Actor |
|---|---|---|---|
| Login | #1 | 로그인 | Pet Owner, Hospital, Admin |
| Register Pet Information | #2 | 반려동물 정보 등록 | Pet Owner |
| Create Health Record | #3 | 건강 기록 작성 | Pet Owner |
| Inquiry Health Record | #4 | 건강 기록 조회 | Pet Owner, Hospital |
| Check Vaccination Schedule | #5 | 예방접종 일정 확인 | Pet Owner |
| Search Hospital | #6 | 동물병원 검색 | Pet Owner |
| Request Reservation | #7 | 병원 예약 신청 | Pet Owner |
| Approve or Reject Reservation | #8 | 예약 승인/거절 | Hospital |
| Send Notification | #9 | 알림 전송 | System |
| Provide Customized Guide | #10 | 맞춤형 건강 가이드 제공 | System |

---

# 3. Domain analysis

## Core Classes

### User
- userId
- name
- phone

### Pet
- petId
- species
- breed
- age
- weight

### HealthRecord
- date
- weight
- symptom
- medicine

### Hospital
- hospitalId
- location
- service

### Reservation
- date
- status

### Notification
- type
- message

---

# 4. User Interface prototype

## Login Interface
사용자가 앱을 실행하면 로그인 화면이 표시된다.

## Register Pet Interface
반려동물 등록 화면에서는 이름, 종, 품종, 나이, 체중 등을 입력한다.

## Health Dashboard
최근 체중, 예방접종 예정일, 복약 알림 등을 확인할 수 있다.

## Hospital Search and Reservation
위치 기반 병원 검색과 예약 기능을 제공한다.

---

# 5. Non-functional requirements

| Category | Requirement |
|---|---|
| Performance | 일반 조회와 저장 기능은 3초 이내 응답 |
| Security | 인증된 사용자만 접근 가능 |
| Availability | 예약 및 알림 기능 안정성 보장 |
| Usability | 직관적인 UI 제공 |
| Scalability | 데이터 구조 확장 가능 |
| Maintainability | 기능 모듈 분리 |
| Privacy | 개인정보 보호 |

---

# 6. Glossary

| Term | Description |
|---|---|
| Pet Owner | 반려동물을 기르는 사용자 |
| Veterinary Hospital | 반려동물 진료를 제공하는 병원 |
| Health Record | 건강 기록 데이터 |
| Reservation | 병원 예약 절차 |
| Vaccination Schedule | 예방접종 일정 정보 |
| Customized Guide | 맞춤형 건강 가이드 |

---

# 7. References

1. Pet Care Conceptualization Document, Yeungnam University, 2026.
2. Software Engineering Use Case Analysis Template.
3. Object-Oriented Analysis and Design principles.
