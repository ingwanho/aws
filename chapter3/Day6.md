# 서버리스 활용법 이해

## aws lambda를 다른 서비스와 연계해 사용
lambda 함수는 입력받은 데이터를 처리하기도 하지만 다른 aws 서비스와 연계해 사용자 접속이나 데이터 연동과 같은 처리를 자동으로 수행할 수 있다.
- api gateway 서비스를 활용하여 사용자의 http 요청을 lambda 함수로 처리할 수 있다.(http 메서드나 api 등의 요청 경로에 대한 정보를 요청받고 함수로 실행 시킨 후 응답해준다.)
- s3 버킷에 데이터가 저장되면 저장된 데이터를 lambda로 자동 처리할 수 있다.(데이터에 포함된 정보가 가공해 다른 s3 버킷에 저장하는 등의 처리가 가능하다.)
- eventbridge를 활용해 람다와 연계해 주기적으로 정해진 시간에 규칙이라는 형태로 lambda함수를 주기적으로 실행가능하다.
- 이외에도 다양한 형태로 lambda함수와 연계해 구현가능하다.

## aws lambda 추가 설정 및 모니터링
실행환경은 일반적으로 aws에서 관리해서 특별히 서버 설정에 관여하지 않아도 된다. 하지만 3가지 설정은 사용자가 변경 가능하다.
- 메모리용량, 타임아웃 시간, 환경 변수
- lambda는 s3에 데이터를 저장하거나 ec2를 시작/중지하는 등 다른 aws 자원을 조작하기 위한 목적으로 사용되는 경우가 많다.
- lambda 함수를만들면 서비스에 대한 조작 권한은 iam을 통해 부여해주어야 정상적으로 처리가능해진다.
- lambda 함수를 만들어 배포하면 aws 모니터링 서비스인 cloudwatch에 자동으로 정보가 전송 되므로 실행 상황을 확인할 수 있다.(실행횟수, 실행시간, 오류 수와 실행 성공률)
- 함수가 출력한 로그도 cloudwatch 로그로 자동 전송된다.

## lambda 이용 요금
lambda에 요금은 요청 수와 처리 시간에 따라 청구된다.
- 프리티어가 있다(요청: 매월 100만건 무료/ 실행시간: 매월 40만GB 초 무료)
- 프로비전드 컨커런시라는 동시 실행 성능을 설정했을때나 외부로 데이터를 전송하는 경우 별도 요금이 발생 가능하다.

# 컨테이너

컨테이너란 한 서버내에서 os와 물리자원을 나눠 사용하지 않고 공동으로 이용하는것을 의미한다.(네트워크가 분리되어 있지만 외부 네트워크와의 통신이나 컨테이너 간 통신을 할때 컨테이너 런타임을 통해 이루어진다.)
- 가상화 서버와 비교하면 가상화 서버를 아파트라고 비유가능하고, 컨테이너는 쉐어하우스로 비교가능하다.(가구=자원이 공용이냐 아니면 개인 가구이냐의 차이다.)
- 컨테이너 안에는 어떤 프로그램을 사용할지 미리 정의해둔 파일이 있다. 이것을 컨테이너 이미지 라고 한다.

## 컨테이너의 장점
컨테이너는 가상화에 비해 가볍고 빠르다. 이는 컨테이너는 가상서버에 비해 포함된 것이 적기 때문이다.
- 기본적으로 응용프로그램을 실행하기 위한 의존성 패키지만 포함되므로 가상 서버에 비해 가볍다.
- 가상 서버보다 경유하는 곳이 적으므로 처리 시간이 짧다.(가상서버는 가상서버안에 앱, 미들웨어, os등을 거쳐야 하지만 컨테이너는 물리서버에 컨테이너 런타임을 통해 서버 자원을 빠르게 사용한다.)

## 배포의 용의성
컨테이너 이미지는 한번 생성하면 다른서버에서 바로 사용할 수 있다.(다른 환경에서도 이미지 안의 내용이 바뀌지 않기 때문에 개발할때 설정해둔 내용이나 라이브러리를 그대로 사용가능하다.)
- 의존성 환경을 맞춰줄 필요가 없어진다.
- 컨테이너 이미지를 압축 파일 형태로 내보내거나 가져오기 가능(이미지 리포지토리를 이용해 이미지를 업로드하거나 내려받을 수 있다.)
- 컨테이너는 완성된 이미지이므로 컨네이너가 종료되면 안에 저장된 내용도 사라진다.(컨테이너 실행시 호스트 서버의 디렉터리를 마운트해서 작업내용을 저장하는 등의 처리를 해줘야 한다.)

### 컨테이너 오케스트레이션
컨테이너 오케스트레이션이란 컨테이너를 이용할때는 각 기능을 컨테이너로 분리해 여러 컨테이너를 하나의 시스템 처럼 유기적으로 결합해 사용된다. 이때 따로 관리하는 컨테이너의 관리 부하나 비정상적인 종료를 자동화해 관리해주는 도구이다.

# ECS
ECS는 aws의 관리형 컨테이너 오케스트레이션 서비스다.
- 아마존에서 컨테이너 시스템을 구축하는 경우 일반적을 amazon elastic container service(ECS)와 amazon elatic kubernetes service(EKS)가 있다.
- 차이점은 누가 관리 하느냐다. ECS는 aws에서 관리하고, EKS는 쿠버네티스에서 관리한다.
- amazon elastic container registry(ECR)을 통햏 컨테이너 이미지 레지스트리 서비스도 제공한다.
- 사용자 정의된 컨테이너 이미즈를 aws에서 관리할 수도 있고 ECS에도 쉽게 배포 가능하다.

## EC2와 Fargate
ECS를 이용해 컨테이너를 기동할 때 어떤 플랫폼에서 시작할 지 선택가능하다. 이때 두 선택지가 EC2와 Fargate다.
- EC2를 사용할 경우 컨테이너 런타임이 설치된 서버를 직접 관리해야 한다.
- Fargate는 aws에서 서버를 관리해준다.(플랫폼 버전(PV)으로 관리되므로 실행할 때 이 PV를 지정해야 한다.)
- 노드(컨테이너를 실행하는 서버)에서 할당하는 하드웨어 자원의 할당 방법도 차이가 있다.
- EC2는 인스턴스 타입과 스토리지 불륨크기를 지정하면된다, Fargate는 cpu와 메모리를 직접 할당할 수 있지만, 지정한 cpu 값에 따라 할당할 수 있는 메모리 크기가 정해져 있다.(스토리지는 사전에 Fargate 태스크 스토리지로 태스크 실행 시 할당되며 할당량은 PV에 따라 다르다.)
- Fargate에 장점은 aws에서 서버를 관리하기 떄뮨에 유지보수 작업에 대한 부분을 편리하게 넘어갈 수 있다.

## 쿠버네티스(EKS)
EKS는 구글에서 개발한 컨테이너 오케스트레이션 도구다.
- 현재 CNCF(cloud native computing foundation)라는 단체에서 오픈 소스 소프트웨어로 공개하고 있다.
- 제일 앞글자 K와 마지막 글자 s 사이에 8글자가 들어간다해서 K8s로 표기하기도 한다.
- 일반적으로 aws를 사용한다면 ECS를 사용하는게 무난하지만, 구축하려는 시스템이 K8s 사용을 전제로 하는 경우라면 EKS를 사용해야 한다.

# 서버지식없이 사용가능한 서비스 5가지

## aws lightsail
자주 사용되는 구성의 가상 서버를 쉽고 빠르게 구축하는 서비스다
- 생성한 인스턴스에는 웹 브라우저를 통해 콘솔 접속을 할 수 있다.
- vpc를 통해 다른 aws서비스와 연동할 수 있다.
- EC2와 다르게 미들웨어 버전을 고려해 설치하거나 ELB를 이용해 유연한 부하 분산을 설정할 수 없다.
- 위 기능이 필요하다면 EC2로 마이그레이션하기 위한 기능도 제공된다.

## aws Elastic Beanstalk
Beanstalk은 자바,.NET, PHP, Python, Ruby, Go, Docker 등의 주요 언어나 환경에서 개발된 응용 프로그램을 배포하기 위한 실행 환경을 자동으로 만들어주는 서비스다.
- EC2나 RDS와 같은 aws 서비스가 이용되므로 사용자가 스스로 구축할 수 있는 시스템을 서버 지식 없이도 구축할 수 있다는 장점이 있다.

## aws App Runner
App Runner은 컨테이너화된 응용 프로그램을 손쉽게 배포하는 서비스다.
- 컨테이너 이미지를 지정하고 서비스 포트나 auto scaling과 같은 몇 가지 설정만으로 컨테이너를 Fargate에 배포 가능하다.

## aws Batch
aws Batch는 지정된 프로그램을 EC2나 Fargate에서 실행하는 서비스다.
- Lambda와 똑같아 보이지만 실행 프로그램의 순서 관리나 다수 프로그램 병렬 구동과 같은 복잡한 제어에 특화되어 있다.

## aws Outposts
온프레미스 환경에서 aws서비스를 사용할 수 있도록 aws를 프라이빗 클라우드화해서 물리서버를 대여하는 서비스다.
- 데이터 센터에 설치할 수 있는 서버 기기 세트가 제공된다.
- 내부에는 ECS, RDS, S3와 같은 서비스가 동작하게 만들어졌다.
- AWS Outposts를 AWS의 새로운 가용 영역으로 사용할 수 있다.