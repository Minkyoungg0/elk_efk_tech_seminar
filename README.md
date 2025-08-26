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

**ELK·EFK의 특징과 장단점**을 명확히 이해해 상황에 맞게 선택할 수 있는 역량을 기르는 것,<br>
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
      EFK 실험 환경 구성 및 로그 분석
    </td>
    <td align="center">
      ELK 실험 환경 구성 및 성능 측정
    </td>
    <td align="center">
      파이프라인 성능비교, 결과 정리
    </td>
  </tr>
</table>

<br/>

## 실험 시나리오
1. JMeter에서 thread 수: 100개, Ramp-up: 60초, Loop Count: 8000회 설정을 통해 총 80만건의 로그데이터 생성
2. Spring Boot App에서 POST 메소드를 통해 각 파이프라인에 80만건의 로그데이터를 JSON 형식으로 전송
3. Filebeat & Logstash와 Fluentd가 각각의 설정을 기반으로 Elasticsearch에 데이터 가공/필터링 및 적재
4. Metricbeat가 각 파이프라인이 실행되는 동안 리소스(Memory, CPU 등) 사용율 측정
5. Kibana를 통해 Metricbeat가 수집한 각 파이프라인의 성능 측정 데이터 시각화

<br/>

## 성능 측정 핵심 지표
- CPU 사용량
- Memory 사용량
- Disk 사용량


<br/>

## 성능 테스트 결과 요약

성능 측정 실험은 Metricbeat를 사용하여 각 파이프라인에 총 80만건의 로그데이터를 수집하는 과정을 3번 거쳐서 성능 데이터 수집 및 분석

1. CPU 사용량
  - ELK Stack
  ![CPU-ELK](https://github.com/user-attachments/assets/117409be-50af-47cd-8f66-0c355786f168)
    - 데이터를 수집하는 시점에 CPU 사용량이 약 80%까지 급격히 증가
    - 데이터를 처리하는 동안 80%의 CPU 사용량 유지
    - 데이터 처리 완료 후 CPU 사용량 급격히 감소

  - EFK Stack
  ![CPU-EFK](https://github.com/user-attachments/assets/b42a0911-dd5c-4a4f-bdcd-49b445300b44)
    - 각 실험들의 CPU 사용량 피크를 보면 약 48%로 상대적으로 낮음
    - 지속시간 또한 짧게 형성되어 있음

2. Memory 사용량
  - ELK Stack
  ![Memory-ELK](https://github.com/user-attachments/assets/092cc077-3a07-47b9-a2ff-d416b60bc370)
    - 총 8GB 중 7.2GB의 메모리를 사용하며 약 94%의 메모리 점유율을 확인할 수 있음

  - EFK Stack
  ![Memory-EFK](https://github.com/user-attachments/assets/cea3e046-314e-4f99-93e2-7f3412a01eca)
    - 총 8GB 중 6GB의 메모리를 사용하며 약 80%의 메모리 점유율을 확인할 수 있음

3. Disk 사용량
  - ELK Stack
  ![Disk-ELK](https://github.com/user-attachments/assets/c0f4292f-f653-4d14-9840-122ae063ec40) 
    - 실험 시작과 동시에 디스크 사용량이 약 15%까지 빠르게 도달 
    - 실험을 반복할수록 점진적으로 디스크 사용량이 조금씩 증가

  - EFK Stack
  ![Disk-EFK](https://github.com/user-attachments/assets/a817c249-0d96-4b84-92ef-31ad030ce031)
    - 실험을 시작하고 디스크 사용량이 약 8% 정도 되는 것을 확인할 수 있음
    - 디스크 사용량이 안정적으로 유지되는 것을 볼 수 있음



<br/>

## 🔎 인사이트

1. **리소스 소비 구조 차이**
   - ELK 스택의 Logstash는 JVM 기반으로 동작하여 **CPU와 메모리를 많이 소모**하는 반면, Fluentd는 C와 Ruby 기반으로 구현되어 상대적으로 가볍게 실행
   - 실제 측정 결과에서도 ELK는 CPU 80%, 메모리 94%까지 치솟았으나, EFK는 CPU 48%, 메모리 80% 수준에서 안정적으로 동작
   - 이는 **클라우드 네이티브 환경**이나 **리소스 제약이 큰 시스템**에서는 EFK가 더 적합함을 보여줌

2. **처리 성능과 복잡성의 균형**
   - Logstash는 복잡한 데이터 파이프라인 구성, 고급 필터링, 멀티 파이프라인 등을 지원하여 **대규모·정교한 로그 처리**에 강점이 있음
   - 반면 Fluentd는 플러그인 중심으로 빠르게 확장 가능하지만, 복잡한 변환보다는 **경량화·단순성**에 초점이 맞춰져 있음
   - 즉, **처리량(throughput)·정밀 제어**가 중요하다면 ELK, **경량성과 유연성**이 중요하다면 EFK를 선택하는 것이 합리적

3. **운영 안정성과 유지보수성**
   - Filebeat는 경량·단일 목적의 수집기로 안정성이 높고, 설정이 단순하여 운영 부담이 적음
   - Fluentd는 플러그인 생태계가 풍부해 다양한 데이터 소스와 연동 가능하지만, 버전 충돌·의존성 관리 문제(예: Elasticsearch 플러그인 호환성 이슈)로 **운영 시 디버깅 비용**이 발생할 수 있음
   - 따라서 **운영의 단순성 vs 확장성**이라는 선택지가 존재

4. **데이터 손실 가능성**
   - 실험 과정에서 JMeter로 80만 건을 전송했음에도 Elasticsearch에 약 68.5%만 적재되는 현상이 발생
   - 이는 Filebeat/Fluentd 버퍼링, Logstash 파이프라인 지연, Elasticsearch 인덱싱 처리 속도 등 **복합적인 병목** 때문으로 추정
   - 따라서 대용량 로그 수집 환경에서는 **버퍼 튜닝, 파이프라인 최적화, 모니터링 강화**가 필수적

5. **실무적 선택 기준**
   - **대규모 트래픽 처리, 복잡한 데이터 전처리·분석**이 필요한 경우 → **ELK 우위**  
   - **리소스 효율성, 클라우드 네이티브 아키텍처, 단순 로그 수집·전송** 중심인 경우 → **EFK 우위**  
   - 궁극적으로는 두 스택을 배타적으로 고르는 것이 아니라, **Filebeat + Fluentd 조합** 혹은 **경량 로그는 EFK, 고급 분석은 ELK**처럼 **하이브리드 접근**이 가장 실무적으로 유효


<br/>

## 🛠 트러블슈팅 (Troubleshooting)

## 1. Metricbeat 권한 오류 ❌➡️✅

### 🔍 문제 상황
```bash
ERROR: Config file must be owned by the user identifier (uid=0) or root
```
- 원인: Docker 컨테이너 내에서 metricbeat.yml 파일 소유자가 root가 아님
- 증상: Metricbeat 컨테이너가 시작과 동시에 종료됨
- 발생 환경: Ubuntu + Docker Compose 환경

### 💡 해결 과정
권한 확인
```bash
ls -la metricbeat/metricbeat.yml
# -rw-r--r-- 1 ubuntu ubuntu 2048 Aug 15 16:30 metricbeat.yml
```
소유자 변경

```bash
sudo chown root:root ./metricbeat/metricbeat.yml
sudo chmod 0644 ./metricbeat/metricbeat.yml
```
재시작 및 확인
```bash
sudo docker-compose restart metricbeat
sudo docker logs -f efk-stack-metricbeat-1
```
### 📚 학습한 점
- Docker 컨테이너의 보안 정책: root가 아닌 사용자로 실행되는 컨테이너는 보안상 제한된 권한을 가짐
- Elastic Stack 구성요소들이 설정 파일 권한에 대해 엄격한 검사를 수행함
## 2. Fluentd Elasticsearch 플러그인 버전 충돌 ❌➡️✅
```bash
Unknown output plugin 'elasticsearch'. Run 'gem search -rd fluent-plugin' to find plugins
```
- 원인: Elasticsearch 9.1.0 버전과 fluent-plugin-elasticsearch 간 호환성 문제
- 증상: Fluentd가 Elasticsearch로 데이터 전송 실패
- 영향: EFK 파이프라인 전체 동작 불가
### 💡 해결 과정
1. 컨테이너 내부 진입하여 문제 진단
```bash
sudo docker exec -u root -it efk-stack-fluentd-1 sh
fluent-gem list | grep elastic
# elasticsearch (9.1.0) ← 문제 버전 확인
```
2. 호환되지 않는 버전 제거
```bash
gem uninstall elasticsearch -v '9.1.0' -aIx
gem uninstall elasticsearch-api -v '9.1.0' -aIx
gem uninstall elasticsearch-transport -v '9.1.0' -aIx
```
3. 호환 가능한 버전 설치
```bash
gem install elasticsearch -v '7.1.0' --no-document
gem install elasticsearch-api -v '7.1.0' --no-document
gem install elasticsearch-transport -v '7.1.0' --no-document
gem install fluent-plugin-elasticsearch -v '6.0.0' --no-document
```
4. 재시작 및 검증
```bash
exit
sudo docker restart efk-stack-fluentd-1
# 로그 확인으로 정상 동작 검증
curl -X GET "http://localhost:9200/fluentd-*/_count?pretty"
``` 
### 📚 학습한 점
- 의존성 관리의 중요성: 오픈소스 생태계에서 버전 호환성 확인이 필수
- 컨테이너 환경에서의 디버깅: 실행 중인 컨테이너 내부 접근을 통한 실시간 문제 해결
- Ruby Gem 관리: -aIx 옵션을 통한 강제 삭제 및 --no-document 옵션으로 설치 속도 개선

<details>
  <summary>해보고 싶은 트러블 슈팅</summary>
  
## 3. 데이터 수집량 차이 분석 📊
### 🔍 예상과 다른 결과
- 계획: JMeter로 80만건 POST 요청 → Elasticsearch에 80만건 저장

- 실제 결과: 548,427건만 저장됨 (약 68.5%)

- 차이 분석이 필요했던 이유: 성능 비교의 정확성을 위해

### 💡 분석 과정
1. 데이터 흐름 추적

- JMeter → Spring Boot → 로그파일 → Filebeat/Fluentd → Logstash/Fluentd → Elasticsearch

- 각 단계별 데이터 손실 지점 파악

2. 원인 분석

- 버퍼링 설정: Logstash/Fluentd의 배치 처리로 인한 지연

- 네트워크 처리량: 대량 데이터 처리 시 일시적 병목 현상

- Elasticsearch 인덱싱 속도: 동시 인덱싱 한계

3. 개선 방안 도출

### Logstash 설정 최적화 예시
```bash
pipeline.batch.size: 1000
pipeline.batch.delay: 50
```
### 📚 학습한 점
- 실시간 처리의 한계: 대용량 데이터 처리 시 버퍼링과 배치 처리 필요성

- 모니터링의 중요성: 실제 처리량과 예상 처리량 간의 차이를 지속적으로 모니터링
</details>

---

## 📎 참고 자료

- [Elastic 가이드북](https://esbook.kimjmin.net/)
- [Fluentd 공식 문서](https://docs.fluentd.org/)
- [Filebeat 공식 문서](https://www.elastic.co/beats/filebeat)


