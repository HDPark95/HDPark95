<h1 align="center">Hi there, I'm 박현두 (Hyundoo Park)</h1>

<p align="center">
  <a href="mailto:qkrgusen456@gmail.com"><img src="https://img.shields.io/badge/Gmail-EA4335?style=flat-square&logo=gmail&logoColor=white"/></a>
  <a href="https://decisionlog.dev"><img src="https://img.shields.io/badge/Blog-decisionlog.dev-0A0A0A?style=flat-square&logo=hashnode&logoColor=white"/></a>
  <a href="https://github.com/HDPark95"><img src="https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white"/></a>
  <img src="https://komarev.com/ghpvc/?username=HDPark95&style=flat-square&color=blue" alt="profile views"/>
</p>

<p align="center"><b>Backend Developer who traces problems to the root and fixes them structurally.</b></p>

---

### Interests

- **System Stability** — Distributed transactions, caching, monitoring
- **Payment & E-commerce** — Raffle systems, PortOne integration, payment module design
- **Automation** — CI/CD pipelines, test automation, crawling pipelines
- **Architecture** — Clean Architecture, Hexagonal, MSA, design patterns for structural improvement

---

### Open Source

**머지된 PR**

| 프로젝트 | 내용 |
|---|---|
| [redis/jedis#4677](https://github.com/redis/jedis/pull/4677) | 파라미터가 이미 설정된 상태에서 `FTSearchParams.params(Map)`이 값을 버리던 버그 수정 |
| [redis/jedis#4684](https://github.com/redis/jedis/pull/4684) | `reduceDim`을 쓰는 `vaddFP32`에서 null `VAddParams` 허용 |
| [OpenAPITools/openapi-generator#24718](https://github.com/OpenAPITools/openapi-generator/pull/24718) | [python] `Optional[...]` 응답 타입 역직렬화 |
| [objectionary/eo#6546](https://github.com/objectionary/eo/pull/6546) | `xslMeasures`를 절대 경로로 만든 뒤 parent를 가져오도록 수정 |
| [objectionary/eo#5669](https://github.com/objectionary/eo/pull/5669) | 너비·정밀도·플래그가 붙은 `printf` 지정자까지 세도록 수정 |
| [lysine-dev/okhttp#9634](https://github.com/lysine-dev/okhttp/pull/9634) | `@Nested` JUnit 5 테스트에서 `@StartStop` MockWebServer가 기동되도록 수정 |

**코드를 바꾼 리뷰**

리뷰할 때는 눈으로 읽는 데서 멈추지 않습니다. 브랜치를 받아서 주장대로 재현되는지 확인하고 깨뜨려 봅니다.

깨지면 의견 대신 실패하는 케이스를 스레드에 가져갑니다. 아래는 그 케이스가 실제 코드 변경으로 이어진 리뷰입니다.

- **[spring-cloud/spring-cloud-stream#3244](https://github.com/spring-cloud/spring-cloud-stream/pull/3244)** —
  `StreamBridge` 캐시 충돌을 고치는 PR이었습니다. diff는 맞아 보였지만 `int` 하나로 만든 해시 키가 "절대 공유되지 않는다"는 주석을 정말 보장하는지 의심스러웠습니다.
  실제로 서로 다른 두 바인딩 설정이 같은 해시 `682613285`로 충돌했고 처음 제보와 동일한 `Partition key cannot be null` 실패를 재현했습니다.
  작성자가 확인하고 캐시를 값 기반 키로 다시 만들었습니다. 그 커밋이 main에 들어갔습니다.
  이 캐시는 `StreamBridge`의 모든 전송이 거칩니다. 설정 조합에 따라 무작위로 터지던 파티션 전송 실패가 다음 릴리스부터 사라집니다.
- **[datahub-project/datahub#19144](https://github.com/datahub-project/datahub/pull/19144)** —
  DataHub 커뮤니티에서 Airflow lineage 작업을 계속 지켜보던 터라, URN 매핑 PR이 올라왔을 때 패치를 그대로 믿지 않고 직접 돌려봤습니다.
  재현 결과를 올리자 메인테이너가 답했습니다. *"You were right, and thanks for actually running it — that repro is what made me look at the whole path instead of just the message."*
  그 재현으로 자산을 lineage에서 조용히 빠뜨리던 스킵 로직이 사라졌습니다.
  lineage 그래프는 자산이 소리 없이 빠지는 순간 신뢰를 잃습니다. 이제 매핑이 없어도 자산은 그래프에 남고 경고가 로그에 남습니다.
- **[micronaut-projects/micronaut-core#12871](https://github.com/micronaut-projects/micronaut-core/pull/12871)** —
  Netty 쿠키 디코더를 직접 구현한 디코더로 바꾸는 PR이었습니다. 두 디코더가 깨진 입력에서도 같은 답을 내는지는 아무도 확인하지 않았습니다.
  `a="unterminated; b=2`를 넣자 Netty는 `[b]`를, 새 디코더는 닫히지 않은 따옴표가 붙은 채로 `[a, b]`를 돌려줬습니다.
  *"Good catch. Pushed df0455c"* — 새 디코더가 Netty처럼 미종결 값을 버리도록 고쳐졌습니다.
  쿠키 파싱은 구현체마다 답이 달라지면 그 차이가 호환성 버그로 돌아옵니다. 교체가 나가기 전에 두 구현의 동작을 맞춰 놓았습니다.
- **[ollama/ollama#17630](https://github.com/ollama/ollama/pull/17630)** —
  로컬 모델을 직접 돌리는 입장이라 `Failed to parse tools` 버그를 그냥 지나칠 수 없었습니다.
  에러 문자열을 grep 하니 Go 코드 어디에도 없었습니다. native runner가 돌려주는 메시지였습니다. 이슈의 스택이 가리키는 실제 로깅 지점을 추적해 알렸습니다.
  *"good catch on both points, done"* — 단위·통합 테스트와 함께 반영됐습니다.
  스레드에 *"please merge this bro you have no idea the type of tomfoolery and rabbit holes i've went down"* 이라는 댓글이 달릴 만큼 사람들이 헤매던 버그였습니다.
- **[spring-cloud/spring-cloud-gateway#4261](https://github.com/spring-cloud/spring-cloud-gateway/pull/4261)** —
  수정은 한 줄이었지만 그 한 줄이 무엇을 함께 켜는지까지 확인했습니다. MVC 쪽에서 한 번도 검증된 적 없는 `Shortcut.Type.LIST_TAIL_FLAG`가 같이 켜지고 있었습니다.
  작성자가 *"yeah fair"*라며 두 플래그 형태를 모두 검증하는 e2e 라우트 테스트를 추가한 뒤 머지됐습니다.
  문서에 없는 플래그가 검증 없이 켜진 채 나가는 일을 막았습니다.
- **[ollama/ollama#17809](https://github.com/ollama/ollama/pull/17809)** —
  파서 강화 PR을 케이스 단위로 검증했습니다. 테스트 하나는 main에서도 통과하는 무의미한 케이스였고, 검증되지 않은 회귀는 Modelfile 전체를 실패시킬 수 있었습니다.
  작성자가 제 케이스 여섯 개를 전부 재현해 그대로임을 확인했습니다.
  머지됐다면 보이지 않는 문자 하나로 Modelfile 전체가 거부되는 회귀였습니다. 릴리스 전에 잡혔습니다.

---

### Tech Stack

<p>
  <img src="https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=OpenJDK&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kotlin-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring%20Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring%20Batch-6DB33F?style=for-the-badge&logo=spring&logoColor=white"/>
  <img src="https://img.shields.io/badge/MyBatis-000000?style=for-the-badge&logoColor=white"/>
  <img src="https://img.shields.io/badge/JPA-59666C?style=for-the-badge&logo=hibernate&logoColor=white"/>
</p>
<p>
  <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white"/>
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
  <img src="https://img.shields.io/badge/MSSQL-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kafka-231F20?style=for-the-badge&logo=apachekafka&logoColor=white"/>
</p>
<p>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white"/>
  <img src="https://img.shields.io/badge/GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white"/>
  <img src="https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonwebservices&logoColor=white"/>
  <img src="https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white"/>
</p>
