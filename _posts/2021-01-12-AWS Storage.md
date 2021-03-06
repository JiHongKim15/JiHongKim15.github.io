---
title: AWS 스토리지
categories: [AWS]
comments: true
---

# Storage

## EBS

:cloud: Elastic Block Store

> 대규모로 처리량과 트랜잭선 집약적 워크로드 모두를 지원하기 위해 EC2에서 사용하도록 설계된 사용하기 쉬운 **고성능 블록 스토리지 서비스**

- :one: SSD 지원 스토리지
  - 트랜잭션 워크로드
  - IOPS 집약적 데이터베이스 워크로드
  - 부트 볼륨 및 높은 IOPS가 필요한 워크로드
- :two: HDD 지원 스토리지
  - 처리량 워크로드
  - 빅 데이터 워크로드
  - 큰 I/O 크기 및 순차적 I/O 패턴 설계

:heavy_check_mark: POINT

- EC2 = 인스턴스가 활성화 되어 있는 동안만 저장된 데이터 지속
- EBS = 인스턴스 수명과 별도록 지속

#### 스냅샷

:cloud: EBS direct APIs for Snapshots

- 볼륨 탑재 안해도 됨
  - EBS 볼륨에 기록된 데이터만 캡처 :o:
  - 로컬 캐싱 데이터 :x:
  - 스냅샷이 일관성을 유지 :point_right: 볼륨 완전 분리 후 스냅샷 명령 후 볼륨 다시 연결

##### FSR

> 빠른 스냅샷 복원

- 데이터 복원 시, 데이터 액세스 지연 시간 걱정 or 초기 성능 히트를 피하려는 경우
- 스냅샷 생성 속도 :x:
- AZ 내의 스냅샷에서 API 호출
- 최대 10개의 초기화 볼륨



## S3

:cloud: Simple Storage Service

> 인터넷 상에서 데이터를 저장하고 검색할 수 있도록 구축된 객체 스토리지

#### :heavy_check_mark: POINT

- 뛰어난 확장성
- 사용한 만큼만 비용 지불
- 성능, 안정성 저하 :x:
- 애플리케이션 확장
- 뛰어난 유연성
  - 원하는 형식의 데이터를 원하는 만큼 저장
  - 동리한 데이터를 수백만 번 읽거나 비상 재해 복구 용도로 사용
  - **웹 애플리케이션 구축**
- 데이터 저장 방법 고민 :x: **혁신에 집중**

- REST 기반 웹 서비스 인터페이스 제공
- HTTPS 프로토콜을 사용하여 SSL 엔드포인트를 통해 S3에 데이터를 안전하게 저장
- 버전 관리 필요

##### 리전

> AWS가 짧은 지연 시간, 높은 처리량 및 중복성이 높은 네트워킹으로 연결된 지리적 위치

##### 가용 영역(AZ)

> 리전 내 물리적으로 격리된 위치

- S3: 리전 내 최소 3개의 AZ 운영

### S3 액세스 포인트

> 많은 애플리케이션에 대한 액세스를 제어하는 단일 버킷 정책 사용
>
> S3의 공유 데이터 세트를 사용하여 애플리케이션에 대한 대규모 데이터 액세스 관리를 간소화

- 버킷당 수백 개의 액세스 포인트를 쉽게 생성
- 공유 데이터 세트에 대한 액세스 프로비저닝하는 새로운 방식을 나타냄
- 고유한 호스트 이름과 액세스 포인트를 통해 생성된 모든 요청에 대해 특정 권한 및 네트워크 제어 적용하는 액세스 정책 사용
- 버킷으로 사용자 지정된 경로 제공
- 버킷과 버킷 콘텐츠에 대한 액세스 제공

###### 사용 이유

- S3에서 공유 데이터 집합으로 설정된 애플리케이션에 대한 데이터 액세스 관리하는 방법을 단순화
- 특정 앱에 맞게 조정된 정책으로 공유 데이터 집합에 대한 액세스 허용하는 앱 특정 액세스 포인트 생성

###### 버킷

- 객체의 논리적 스토리지 컨테이너

### S3 Intelligent-Tiering

> 알 수 없는 액세스 패턴 또는 알아보기 어려운 변화하는 액세스 패턴이 있는 데이터를 위한 S3 스토리지 클래스
>
> 액세스 패턴이 변화할 때 두 개의 액세스 계층 간에 객체를 이동하여 자동 비용 절감 효과를 제공하는 최초의 클라우드 스토리지 클래스
>
> - 자주 액세스하는 용도 최적화
> - 자주 액세스하지 않는 용도 (요금 저렴)

##### 사용

- 액세스 패턴을 예상할 수 없는 데이터 세트
- 업로드 직후 액세스가 빈번하지만 데이터 세트가 오래되면서 액세스가 감소할 때
  - 데이터 세트를 S3 One Zone-IA로 이동 or S3 Glacier에 아카이브

### S3 스탠다드-Infrequent Access(S3 스탠다드-IA)

> 액세스 빈도가 낮지만 필요할 때 빠르게 액세스해야 하는 데이터를 위한 S3 스토리지 클래스

### S3 One Zone-Infrequent Access(S3 One Zone-IA)

> 단일 가용 영역에 객체를 저장하도록 선택할 수 있는 S3 스토리지 클래스

- 데이터를 중복으로 저장

- S3 스탠다드-IA보다 20% 저렴

- 백업용

### S3 Glacier

> 백업 및 아카이브 스토리지 서비스

- 저렴

### S3 Glacier Depp Archive

> 1년에 한두 번 정도 액세스하는 데이터의 장기 보존을 위한 안전하고 안정적인 객체 스토리지

### 쿼리지원

### 이벤트 알림

### S3 Transfer Acceleration

> 거리가 먼 클라이언트와 S3 버킷 간에 파일을 빠르고, 쉽고, 안전하게 전송
>
> 전 세계적으로 분산된 Amazon CloudFront의 AWS 엣지 로케이션을 활용
>
> 데이터가 AWS 엣지 로케이션에 도착하면 최적화된 네트워크 경로를 통해 S3 버킷으로 라우팅



## Glacier

> 데이터 아카이빙을 위한 장기적이고 안전하며 내구성 있는 S3 객체 스토리지 클래스

- 데이터 백업 및 아카이브를
- 유연성
- 매우 저렴
- 용량 계확, 하드웨어 프로비저닝, 데이터 복제, 하드웨어 장애 탐지 및 수리, 시간 소모적인 하드웨어 마이그레이션 :x:

#### :heavy_check_mark: POINT

- 빠른 검색
- 내구성 및 확장성
- 가장 포괄적인 보안 및 규정 준수 기능
- 저렴한 비용
- 가장 많은 파트너



## Cloud Front

> 빠르고 안전하며 프로그래밍 가능한 콘텐츠 전송 네트워크(CDN)

- 개발자 칙화적 환경에서 짧은 지연 시간과 빠른 전송 속도로 데이터, 동영상, 앱, API를 전 세계 고객에게 안전하게 전송하는 고속 콘텐츠 전송 네트워크 서비스
- AWS 글로벌 인프라와 직접 연결된 물리적 위치 + 다른 AWS Services와도 통합

#### :heavy_check_mark: POINT

- 빠른 속도 및 세계적인 분산
  - 고도로 확장 가능
  - 전 세계에 분산
  - 220개 이상의 PoP(Point of Persence)
  - 고탄력성의 아마존 백본 네트워크 활용
  - 탁월한 성능과 가용성
- 엣지 보안
  - 네트워크, 앱 수준 보안 제공
  - 고객의 트래픽과 앱 추가 비용 없이 AWS Shield Standard 사용
- 고도로 프로그래밍 가능
  - 특정 앱 요구사항에 맞게 사용자 지정
  - Lambda@Edge 함수
    - 고객의 사용자 지정 코드를 전 세계 AWS 위치로 확장
    - 복잡한 앱 로직이라도 최종사용자에게 가깝게 배치하여 응답성 개선
  - DevOps 및 CI/CD 환경이 요구하는 다양한 도구 및 자동화 인터파에스와 통합 지원
- AWS와 긴밀한 통합



## Storage Gateway

> 온프레미스에서 무제한의 클라우드 스토리지 액세스
>
> 하이브리드 클라우드 스토리지 서비스

- 표준 스토리지 프로토콜 세트 제공
  - 기존 앱 다시 작성:x:
- 아마존 클라우드 스토리지 서비스에 데이터를 안전하고 안정적으로 저장하면서 자주 액세스하는 데이터는 온프레미스에 캐싱하여 **지연 시간이 짧은 성능을 제공**
- 변경된 데이터만 전송, 데이터 압축하여 AWS 데이터 **전송 최적화**

- 스토리지 관리 간소화 및 비용 절감 효과
  - S3 클라우드 스토리지를 활용하여 온프레미스 스토리지 공간 비용 감소
- 백업을 클라우드로 이동, 클라우드 스토리지에서 지원되는 온프레미스 파일 공유 사용
- 온프레미스 앱에서 AWS 데이터에 낮은 지연시간으로 액세스

1. **파일 게이트웨이**

   - NFS(Network File System), SMB(Server Message Block) 파일 프로토콜
   - S3에 객체 저장, 검색
   - S3에 직접 액세스

2. **테이프 게이트웨이**

   - 가상 미디어 체인저, 가상 테이프 드라이브, 가상 테이프로 구성된 iSCSI 가상 테이프 라이브러리(VTL) 인터페이스를 백업 앱에 제공
   - S3에 가상 테이브 저장, 아카이브

3. **볼륨 게이트웨이**

   > 어느 모드에서든 볼륨의 특정 시점을 스냅샷, EBS에 저장

   - iSCSI 연결을 사용하여 온프레미스 앱에 블록 스토리지 제공

   - 볼륨의 데이터는 S3에 저장

   - 사용자는 특정 시점의 볼륨 복사본 생성

     :point_right: EBS 스냅샷으로 저장

   - 볼륨의 복사본을 가져와 보조 관리:o:

   - 캐싱 모드

     - 기본 데이터가 S3에 저장,
     - 자주 액세스하는 데이터는 캐시에 보존

   - 저장 모드

     - 기본 데이터가 로컬에 저장
     - 데이터 세트가 제공되어 AWS에 비동기식으로 백업

#### :heavy_check_mark: POINT

- 간편성
- 클라우드 통합
- 짧은 지연 시간
- 내구성 및 보안
- 전송 최적화
- 확장성
- 기존 앱과 워크플로 내에서 AWS 스토리지를 효과적으로 활용하는 데 도움이 되는 기능 세트 제공
- iSCSI, SMB, NFS와 같은 표줄 프로토콜 세트 제공

