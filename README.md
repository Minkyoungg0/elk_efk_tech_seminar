# 🚀 대용량 로그 수집/분석 ELK vs EFK 파이프라인 프로젝트

## 💡 프로젝트 소개

이 프로젝트는 ELK(Elasticsearch, Logstash, Kibana)와 EFK(Elasticsearch, Fluentd, Kibana) 기반의 로그 파이프라인을 구축·운영하여  
실제 서비스 환경에서 로그와 시스템 성능 모니터링을 어떻게 최적화할 수 있는지 실험한 결과를 정리한 것입니다.  
대용량 부하 생성(JMeter), 실시간 로그 수집 및 시각화, 그리고 두 파이프라인의 성능 비교 결과까지 모두 포함합니다.

<br/>

## 🛠️ 전체 아키텍처

![구성도 예시 삽입](your-architecture-diagram-url.png)

| 구성요소      | 버전      | 역할                |
|---------------|-----------|---------------------|
| Filebeat      | 8.13.4    | 로그 수집 (ELK)     |
| Logstash      | 8.13.4    | 데이터 가공/전송    |
| Fluentd       | -         | 데이터 가공/전송(EFK) |
| Elasticsearch | 8.13.4    | 로그 저장/검색      |
| Kibana        | 8.13.4    | 로그 시각화         |
| Metricbeat    | 8.13.4    | 시스템 성능 측정    |
| Spring Boot   | 3.5.3     | API/로그 생성       |
| JMeter        | 5.6.3     | 부하 생성           |

<br/>

## 🌐 실험 환경 및 실행 방법

- **운영체제:** Ubuntu
- **로그·ELK·Metricbeat:** Docker Compose로 통합 관리
- **Spring Boot:** Ubuntu에 직접 기동  
- **부하테스트:** JMeter (더미 CSV데이터로 80만 건 POST 요청)


- Spring Boot 실행 후 부하 실행:  
  JMeter 스레드 수/루프/램프업 등 실전 설정 info 제공

<br/>

## 🔎 실험 시나리오 및 로그 흐름

1. JMeter → Spring Boot `/login` API POST 요청 (총 80만 건)
2. Spring Boot에서 로그파일(JSON) 저장
3. Filebeat/Fluentd가 로그파일 실시간 감지, Logstash/Fluentd로 전송
4. Logstash/Fluentd → 데이터 파싱·정제 → Elasticsearch 색인
5. Kibana(Metricbeat)로 로그/자원(CPU/Mem등) 실시간 시각화/모니터링

<br/>

## 📊 성능 비교 요약

| 측정 항목     | ELK (Logstash)          | EFK (Fluentd)         |
|---------------|------------------------|-----------------------|
| **CPU 사용률** | 피크 80% 부근, 고자원 소비 | 피크 48%, 부하 짧고 낮음 |
| **메모리**     | 7.2GB(설정치 94%)       | 6GB(설정치 80%)       |
| **디스크**     | 15%, 반복 시 점진 증가   | 8%, 안정적 유지        |
| **네트워크**   | 최대 4.3Mbit/s, 높음    | 최대 1.0Mbit/s, 낮음   |
| **장점 요약**  | 복잡/대규모/고급분석 강점 | 경량/효율/실시간 처리강점 |

<br/>

## 💬 실무 현장 도입 예시

- **ELK**: 넷플릭스/이베이/카카오 등 복잡 파싱·시각화 요구, 장애·보안 로그 중심  
- **EFK**: 쿠버네티스 기반, 대규모 서버군, 자원 제한 환경 실시간 모니터링 중심

<br/>

## 💡 Metricbeat의 활용

- 시스템·컨테이너의 CPU/Memory/네트워크 등 성능지표를 실시간으로 수집  
- Kibana Observability로 부하테스트 중 자원 변화까지 시각화

<br/>

## 🧑‍💻 실험 결과물 예시

- [발표 PPT 링크](your-ppt-link.pdf)  
- 실시간 로그 인덱스 캡처, metricbeat 대시보드 샘플 등 첨부

---

## 📝 프로젝트 활용/시사점

- **ELK**는 복잡한 로그처리, 파싱, 대시보드가 필요한 엔터프라이즈 서비스에 적합
- **EFK**는 경량 실시간 로그수집이나 컨테이너/IoT/스타트업·SMB 환경에 적합
- 두 파이프라인 모두 현대 DevOps/Observability 인프라의 핵심

```

## 📂 디렉토리 구성

├── docker-compose.yml
├── logstash/
│ ├── config/
│ └── pipeline/
├── filebeat/
│ └── filebeat.yml
├── springboot/ (jar/소스)
├── jmeter/ (테스트 시나리오, 더미 CSV)
├── 발표자료/
├── metricbeat/
│ └── metricbeat.yml
└── README.md


```

## 📢 발표/리드미 작성자  
> [본인의 이름] (소속/깃허브/이메일 등)
