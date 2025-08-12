# 🚀 대용량 로그 수집/분석 ELK vs EFK 파이프라인 프로젝트

![image](./first_page.png)
**ppt 자료**
<a href="./FISA_1차 기술세미나_ELKvsEFK.pptx" download>
  ppt 자료
</a>
## 💡 프로젝트 개요

많은 기업들이 로그 분석을 위해 ELK 스택을 활용하고 있으며, 이와 유사하게 EFK 스택도 함께 언급됩니다. 두 스택은 모두 로그 분석을 목적으로 하지만,<br>
Logstash와 Fluentd라는 수집 도구의 차이가 있습니다. <br>
본 프로젝트에서는 ELK와 EFK를 직접 구현하고, 성능·구성·활용성 측면에서 비교 분석하였습니다. 이를 통해 실제 도입 시 고려해야 할 요소와 <br>
환경별 효율적인 활용 방안을 도출하고자 하였습니다.

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

## 🛠️ 기술 스택

| 구성요소      | 버전      | 역할                |
|---------------|-----------|---------------------|
| Filebeat      | 8.13.4    | 로그 수집 (ELK)     |
| Logstash      | 8.13.4    | 데이터 가공/전송    |
| Fluentd       | 1.16.3    | 데이터 가공/전송(EFK) |
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
- **Spring Boot 실행 후 부하 실행:**  
  JMeter 스레드 수/루프/램프업 등 실전 설정 info 제공

<br/>

## 📂 디렉토리 구성
```

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

| 화면 설명                             | 미리보기                      |
| --------------------------------- | ------------------------- |
| 🌐 **CPU-ELK**       | 📌 ![CPU-ELK](https://github.com/user-attachments/assets/117409be-50af-47cd-8f66-0c355786f168)  |
| 📂 **CPU-EFK** | 📌 ![CPU-EFK](https://github.com/user-attachments/assets/b42a0911-dd5c-4a4f-bdcd-49b445300b44) |
| ❌ **Memory-ELK**  | 📌  ![Memory-ELK](https://github.com/user-attachments/assets/092cc077-3a07-47b9-a2ff-d416b60bc370)    |
| 🔎 **Memory-EFK**   | 📌  ![Memory-EFK](https://github.com/user-attachments/assets/cea3e046-314e-4f99-93e2-7f3412a01eca)      |
| 🏠 **Disk-ELK**        | 📌 ![Disk-ELK](https://github.com/user-attachments/assets/c0f4292f-f653-4d14-9840-122ae063ec40)        |
| ⚠️ **Disk-EFK**         | 📌 ![Disk-EFK](https://github.com/user-attachments/assets/a817c249-0d96-4b84-92ef-31ad030ce031)          |



---

## 📝 프로젝트 활용/시사점

- **ELK**는 복잡한 로그처리, 파싱, 대시보드가 필요한 엔터프라이즈 서비스에 적합
- **EFK**는 경량 실시간 로그수집이나 컨테이너/IoT/스타트업·SMB 환경에 적합
- 두 파이프라인 모두 현대 DevOps/Observability 인프라의 핵심


## 🛠 트러블슈팅 (Troubleshooting)

> 해당 세션에서는 구성 중 발생한 다양한 문제들 (ex. 권한 오류, mount 오류 등)에 대한 해결 방법이 포함되어 있습니다.


---

## 📎 참고 자료

- [Elastic 가이드북](https://esbook.kimjmin.net/)
- [Fluentd 공식 문서](https://docs.fluentd.org/)
- [Filebeat 공식 문서](https://www.elastic.co/beats/filebeat)


