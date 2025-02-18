# aws의 6가지 특징

### 책임공유
aws는 클라우드 서비스이기 때문에 온프레미스와 달리 직접 책임지고 수리 및 복구 작업을 하지 않아도 된다. 이를 설명하자면 aws는 공동 책임 모델이라는 형태의 복구 시스템이 있다.
- 책임의 범위는 소프트웨어나 설정은 사용자가 관리하고 하드웨어와 같은 클라우드 서비스의 기본환경은 aws가 책임진다.

### 글로벌 시스템 구축
aws에서 관리하는 데이터 센터는 전 세계에 존재한다. 이것을 지역별로 묶어서 리전이라는 단위로 분리해서 나타낸다.
- 각 리전에는 가용 영역(avzilable zone)이 여러 개 존재하는데, 가용영역은 하나 이상의 데이터 센터로 구성된다.
- 쓰고 있던 가용 영역에 문제가 발생해도 다른 가용영역에서 서비스를 제공받을 수 있어서 장애시에 대기시간 등의 문제가 없다.

### 종량 과금제
aws는 사용한 만큼의 따른 요금제 부과가 설정되어 있다.
- 소규모 서비스를 시작할 때 유리하다.

### 서버의 자원과 수를 유연하게 변경가능
aws에서 생성한 서버는 자원이나 수를 원하는 만큼 쉽게 변경가능하다.

### 가용성능력
aws는 장애의 언제든 대처하기위해 발생한다는 전제로 설계되었다. 그렇기에 장애가 발생해도 지속적으로 사용할 수 있다.

### 효율적인 프레임 워크
aws는 well-architected 라는 안전하고 효율적인 인프라를 구축할수 있게 해주는 프레임워크를 제공한다.
- 운영 우수성, 보안, 성능 효율성, 비용 최적화, 지속 가능성 이 6가지의 원칙을 중심으로 한다.

# 서비스

### 컴퓨팅
응용 프로그램이나 미드웨어를 동작시키기 위한 가상 서버환경을 제공하는 서비스(컨테이너 서비스도 컴퓨팅 서비스의 해당)
- EC2, ECS

### 스토리지
파일을 저장하는 서비스 (가상 디스크, 가상 스토리지 등)
- EBS, S3

### 데이터 베이스
말 그대로 다양한 데이터를 보전하는 기능이다.(관계형 데이터베이스 및 키-값 데이터베이스 등 다양한 데이터베이스 보유)
- RDS, DynamoDB

### 네트워크 및 콘텐츠 전송
각 서비스를 연결하는 네트워크와 사용자에게 콘텐츠를 제공하는 기능을 가진 서비스
- 폐쇄형 클라우드 VPC, 고기능 Route 53, 서비스 처리를 분산시키는 ELB 등

### 보안 및 자격증명
시스템을 사이버 공격으로부터 보호하기 위한 서비스

### 관리형 서비스
서비스를 이용하기 위해 관리나 운영을 서비스 제공자가 수행하는 것을 말한다.
- RDS, S3 등

자동확장 기능이 있는 Aurora같은 사용자가 관리할 필요 없는 서비스를 '완전관리형 서비스'라고 한다.

### 비관리형 서비스
관리형과 반대로 사용자가 직접 os를 설정하고 관리하면서 장애의 대응하는 서비스를 의미한다.
- EC2 등

