# anser-b2b-service
앤서테크놀로지스 b2b 서비스 구현 부분

**프로젝트 설명**

*앤서테크놀로지스의 B2B 서비스 플랫폼을 구축하는 프로젝트입니다.* 

*기존 사내 DB 정보를 바탕으로 B2B 플랫폼용  DB를 모델링하였습니다.*

*최대주주현황, 손익계산서, 실시간 주가정보, 캔들차트 등 다양한 API 를 구축했습니다.*

**담당업무**

- B2B 서버 API 개발 인턴(백엔드 PM)
- 현직 프론트엔드 개발자들과 협업
- Python Django / PostgreSQL / Redis 기반의 API 개발

**기술스택**

- Python 3, Python Django, Websocket, PostgreSQL

**주요업무**

- Websocket 과 multithreading 을 통한 주가정보 push 로직 구현
- 네이버 클라우드 SMS API 서비스를 이용한 서드파티 문자인증 구현
- 주가 캔들차트 기능 구현
- Excel 파일 exporter 구현
- 손익계산서 표시 로직 구현
- PostgreSQL 로 기존 데이터를 B2B 서비스에 맞게 DB 모델링
- 소스관리 : Git, Github
- 협업 툴 : Slack

**세부내용**

***Websocket & Multithreading***

- Django Channels 와 Redis 를 사용하여 주식 종목 별로 소켓 채널을 그룹화
- 주식 종목 별 thread 를 생성/종료 하는 Manager Thread 를 서버 시작시 Background Thread 로 실행
- Websocket Consumer 와 Manager Thread 간 queue 를 이용한 유저 접속 정보 통신 구현
- Manager Thread 에서 queue 로 전달받은 유저 정보를 바탕으로 종목코드 N 개당 스레드 N 개 생성 & 스레드 자신이 연결된 소켓 그룹에 주가정보를 broadcasting 하는 로직을 구현하여 주식 정보 API call 을 대폭 감소

***Pandas***

- Python 의 pandas 모듈과 xlwt 를 사용하여 엑셀 파일 생성 및 여러 엑셀 파일을 통합하여 하나의 엑셀 파일 내부에서 여러개의 sheet 로 나누는 로직 구현
- pandas 모듈로 csv 파일을 읽어온 뒤 모델링 한 PostgreSQL db 에 데이터를 넣는 로직 구현

***Django ORM***

- 기존 캔들차트(일봉, 주봉, 월봉)가 Application Level 에서 시간복잡도 O(N^2) 으로 그려지던 것을 DB Level 에서 처리하도록 로직을 변경하여 처리속도 16배 개선

***Redis***

- 회원가입 문자인증 과정 중 필요한 정보를 Redis 서버에 캐싱
- Django channels 사용 시 socket channel 과 group 을 캐싱

***PostgreSQL***

- 기존 사내 DB 정보를 바탕으로 B2B 플랫폼용 DB 모델링
- Django model 의 one-to-many, many-to-many 관계 적극 활용

***Django DRF***

- Django DRF Serializer 를 활용해 데이터를 타입에 맞게 가공 및 추출
