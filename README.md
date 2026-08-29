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

**Merged PRs**

| Project | What |
|---|---|
| [redis/jedis#4677](https://github.com/redis/jedis/pull/4677) | `FTSearchParams.params(Map)` dropped values when parameters were already set |
| [redis/jedis#4684](https://github.com/redis/jedis/pull/4684) | Accept null `VAddParams` in `vaddFP32` with `reduceDim` |
| [OpenAPITools/openapi-generator#24718](https://github.com/OpenAPITools/openapi-generator/pull/24718) | [python] Deserialize `Optional[...]` response types |
| [objectionary/eo#6546](https://github.com/objectionary/eo/pull/6546) | Resolve `xslMeasures` to an absolute path before taking its parent |
| [objectionary/eo#5669](https://github.com/objectionary/eo/pull/5669) | Count `printf` specifiers carrying width, precision or flags |
| [lysine-dev/okhttp#9634](https://github.com/lysine-dev/okhttp/pull/9634) | Start `@StartStop` MockWebServer for `@Nested` JUnit 5 tests |

**Reviews that changed shipped code**

I don't read PRs — I run them. Check out the branch, reproduce the claim, then try to break it.
When it breaks, I bring the failing case to the thread, not an opinion. These are the reviews
where the code changed because of what I found:

- **[spring-cloud/spring-cloud-stream#3244](https://github.com/spring-cloud/spring-cloud-stream/pull/3244)** —
  A fix for `StreamBridge`'s cache collisions looked right until I asked the question the diff didn't answer:
  can a single `int` hash key ever guarantee "must never share"? It can't — I found two accepted binding
  configurations whose `Objects.hash` collide (`682613285`, both of them) and showed the send failing with
  `Partition key cannot be null`, the exact symptom from the original report. The author confirmed the repro
  and rebuilt the cache around a value key; that commit is what got cherry-picked into `main`.
- **[datahub-project/datahub#19144](https://github.com/datahub-project/datahub/pull/19144)** —
  I'd been following the Airflow lineage work in the DataHub community, so when the URN-mapping PR landed
  I didn't take the patch's word for it — I ran it. My repro made the maintainer rethink the whole code path:
  *"You were right, and thanks for actually running it — that repro is what made me look at the whole path
  instead of just the message."* The silent skip that dropped assets from lineage is gone because of that run.
- **[micronaut-projects/micronaut-core#12871](https://github.com/micronaut-projects/micronaut-core/pull/12871)** —
  A PR replaced Netty's cookie decoder with a hand-rolled one, and nobody had checked whether the two still
  agreed on garbage input. I fed both decoders malformed values until they disagreed:
  `a="unterminated; b=2` — Netty returns `[b]`, the new decoder returned `[a, b]` with the unmatched quote
  still attached. *"Good catch. Pushed df0455c"* — the new decoder now drops unterminated values exactly like Netty.
- **[ollama/ollama#17630](https://github.com/ollama/ollama/pull/17630)** —
  I run local models, so a `Failed to parse tools` bug wasn't abstract to me. I grepped the error string,
  found it exists nowhere in the Go tree — it comes back from the native runner — and traced the real
  logging site the issue's stack trace pointed at. *"good catch on both points, done"*, plus the unit and
  integration tests that came with it.
- **[spring-cloud/spring-cloud-gateway#4261](https://github.com/spring-cloud/spring-cloud-gateway/pull/4261)** —
  The fix was one line, but I read what the line *activates*: it silently turned on
  `Shortcut.Type.LIST_TAIL_FLAG`, declared but never exercised on the MVC side. The author agreed
  (*"yeah fair"*) and added end-to-end route tests covering both flag shapes before merging the behavior in.
- **[ollama/ollama#17809](https://github.com/ollama/ollama/pull/17809)** —
  Verified a parser hardening PR case by case and proved one of its tests was dead weight — it passed on
  `main` too — while an untested regression could fail whole Modelfiles. The author re-ran all six of my
  cases and confirmed every one.

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
