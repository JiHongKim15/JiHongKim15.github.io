---
title: CloudEndure
categories: [AWS]
comments: true
---

# CloudEndure

> 기존 legacy 환경을 그대로 머신에 복제하여 Cut-Over

- Left & Shift

- rehost에서 사용 (6r)
- 단순히 복제해서 새로 만드는 것은 무료 (migration)

#### 사전조건

- Linux
  - Nitro instance 사용 필요

#### 동작 방식

![image-20210113140910699](..\assets\img\image-20210113140910699.png)

> 별도의 VPC 사용 -> 인터넷 연결 ❌

1. Source가 되는 서버

   - Agent 설치 (Python 프로그램): 간단한 명령어로 설치
   
     - python, cloudendure(443 SSL open)
2. 복제가 되는 CloudEndure 서버 (자동 생성)
   - 자동 복제: 원본 서버가 켜있을 경우
   - S3와 연결 (필수) < NAT 시 연결 ⭕
   - 1500 통신
   - IAM을 통한 인증
   - **디스크**는 보존, 서버는 돌려가면서 사용
3. CloudEndure

   - 443 통신
   
4. Target 서버

   > BluePrint 셋팅값에 따라 생성된 Target 서버



- Converter 서버 : 테스트 또는 Cutover시 Replication 서버에서 Donverter 서버로 데이터 이관이 이루어지고 최종적으로 BluePrint 세팅값에 따라 Target 서버가 생성됨 Converter 서버는 생성되었다가 종료됨

#### 실행 방법

###### Agent 설치

1. CloudEndure과 AWS 연결

   > Access Key 등록

   - other infara==a 

2. 물리 서버에 Agent 설치

   - --no-replication: 자동 실행 방지

     > 실행 시 replication 서버 생성 및 실행

3. CloudEndure에서 작업 실행

###### 테스트

4. CloudEndure Blue print를 설정

   : Blue Print로 환경 설정

   > SandBox(외부 유입/유출이 없는 갇혀있는 환경)

5. 테스트 실행

   - Test Mode == Cutover

     > 동작하는 작동이 비슷함

   - Cut Over

     > 운영중이던 Service Application을 내리고 다른 시스템으로 이관하는 일련의 행위 
     >
     > +) Backout: 시스템 적용 중에 문제 등이 발생하여 더이상 적용 진행이 불가능한 경우 원래의 시스템으로 되돌리는 것



###### 기타 (정리안됨)

- Security Group 미리 생성해야 함
- 중복되어있는 스케줄 관리 필요
- 충돌되는 것들(보안) 확인
- DB는 ❌



###### 체크사항

- OS 등 기본적인 물리 사항