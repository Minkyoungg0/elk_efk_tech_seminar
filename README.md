# 🚀 대용량 로그 수집/분석 ELK vs EFK 파이프라인 프로젝트

![image](./first_page.png)

## 💡 프로젝트 소개

**Elasticsearch의 동작 원리**와 **ELK·EFK의 핵심 차이**를 비교·분석하고, <br>
**로그 파이프라인을 직접 구축**하여 활용 방안을 탐구했습니다.<br>
또한 **CPU·메모리·디스크 사용량을 측정**해 두 스택의 **성능 차이**를 수치로 확인했습니다.

<br/>

## 🎯 주제 선정 이유

**많은 기업들**이 **ELK 스택을 사용**하며, **EFK도 함께 언급**됩니다. <br>
ELK와 EFK는 모두 로그 분석에 사용되지만, 수집 도구의 차이로 특징과 장단점이 다릅니다.<br>
이에 **두 스택을 직접 구현·비교**하여, **환경별 최적 선택 기준**과 **효율적인 활용 방안**을 도출하고자 했습니다.

<br/>

## 🏆 목표

**ELK·EFK의 특징과 장단점**을 명확히 이해해 상황에 맞게 선택할 수 있는 역량을 기르는 것,
또한 **로그 파이프라인 구축**과 **내부 설정 이해**를 통해 **실제 서비스에서 효과적으로 활용**할 수 있도록 하는 것입니다.

<br/>

## 🛠️ 기술 스택

| 구분        | 사용 기술 |
|-------------|-----------|
| ELK Stack   | Elasticsearch 8.13.4, Filebeat 8.13.4, Logstash 8.13.4, Kibana 8.13.4 |
| EFK Stack   | Elasticsearch 8.13.4, Fluentd 1.16.3, Kibana 8.13.4 |
| Monitoring  | Metricbeat 8.13.4 |
| Application | Spring Boot 3.5.3 |
| Test Tool   | JMeter 5.6.3 |

<br/>

## 👨‍👩‍👧 팀 역할 분담


<table>
  <tr>
    <!-- 이름 (링크) -->
    <td align="center">
      <a href="https://github.com/imewuzin"><strong>임유진</strong></a>
    </td>
    <td align="center">
      <a href="https://github.com/dlacowns21"><strong>임채준</strong></a>
    </td>
    <td align="center">
      <a href="https://github.com/Minkyoungg0"><strong>문민경</strong></a>
    </td>
    <td align="center">
      <a href="https://github.com/Gill010147"><strong>황병길</strong></a>
    </td>
  </tr>
  <tr>
    <!-- 프로필 사진 -->
    <td align="center">
      <img src="https://github.com/imewuzin.png" width="100"/>
    </td>
    <td align="center">
      <img src="https://github.com/dlacowns21.png" width="100"/>
    </td>
    <td align="center">
      <img src="https://github.com/Minkyoungg0.png" width="100"/>
    </td>
    <td align="center">
      <img src="https://github.com/Gill010147.png" width="100"/>
    </td>
  </tr>
  <tr>
    <!-- 역할 -->
    <td align="center">
      PPT, Elasticsearch 자료 정리
    </td>
    <td align="center">
      ELK 실험 환경 구성 및 로그 분석
    </td>
    <td align="center">
      EFK 실험 환경 구성 및 성능 측정
    </td>
    <td align="center">
      파이프라인 성능비교, 결과 정리
    </td>
  </tr>
</table>

<br/>

## 🛠️ 전체 아키텍처

![구성도 예시 삽입](your-architecture-diagram-url.png)

| 구분        | 사용 기술 |
|-------------|-----------|
| ELK Stack   | Elasticsearch 8.13.4, Filebeat 8.13.4, Logstash 8.13.4, Kibana 8.13.4 |
| EFK Stack   | Elasticsearch 8.13.4, Fluentd 1.16.3, Kibana 8.13.4 |
| Monitoring  | Metricbeat 8.13.4 |
| Application | Spring Boot 3.5.3 |
| Test Tool   | JMeter 5.6.3 |

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

- <a href="./FISA_1차 기술세미나_ELKvsEFK.pptx" download>
  발표 PPT 링크
</a>

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


## 🛠 트러블슈팅 (Troubleshooting)

> 해당 세션에서는 구성 중 발생한 다양한 문제들 (ex. 권한 오류, mount 오류 등)에 대한 해결 방법이 포함되어 있습니다.

> 👉 자세한 내용은 발표 자료(PPT)를 참고하세요.


---

## 📎 참고 자료

- [Elastic 공식 문서](https://www.elastic.co/guide/index.html)
- [Fluentd 공식 문서](https://docs.fluentd.org/)
- [Filebeat 공식 문서](https://www.elastic.co/beats/filebeat)


