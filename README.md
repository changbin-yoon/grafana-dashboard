# Grafana 대시보드 패키지

Kubernetes 환경에서 실행되는 데이터 플랫폼 서비스들을 모니터링하기 위한 Grafana 대시보드 모음입니다.

## 📋 개요

이 패키지는 다음 서비스들을 모니터링하기 위한 대시보드를 제공합니다:

- **Kafka 4.0**: 메시지 브로커 및 스트리밍 플랫폼
- **Trino 475**: 분산 SQL 쿼리 엔진
- **Airflow 3.0.6**: 워크플로우 오케스트레이션
- **MinIO**: S3 호환 객체 스토리지
- **HMS 3.1.3**: Hive Metastore
- **Spark 3.5.8**: 분산 데이터 처리 엔진

모든 서비스는 하나의 Kubernetes 네임스페이스에서 실행되며, Prometheus를 통해 메트릭을 수집합니다.

## 📁 대시보드 목록

### 1. 전체 서비스 개요 대시보드 (`overview-dashboard.json`)
모든 서비스의 주요 메트릭을 한눈에 볼 수 있는 통합 대시보드입니다.

**주요 패널:**
- 전체 서비스 CPU/메모리 사용률
- 서비스별 CPU 사용률 (Gauge)
- 서비스별 Pod 수
- Kafka 메시지 수신률
- Trino 쿼리 큐 길이
- Airflow DAG 처리 시간
- MinIO 디스크 사용량

### 2. Kafka 상세 대시보드 (`kafka-dashboard.json`)
Kafka 클러스터의 상세 메트릭을 제공합니다.

**주요 패널:**
- Broker 수, CPU/메모리/디스크 사용률
- Topic별 메시지 수신률 및 바이트 수신률
- Active Controller 수
- Under-replicated Partitions
- Topic별 로그 크기
- 네트워크 요청 시간
- 전체 메시지 처리량
- Pod별 CPU 사용률

### 3. Trino 상세 대시보드 (`trino-dashboard.json`)
Trino 쿼리 엔진의 성능 및 상태를 모니터링합니다.

**주요 패널:**
- Worker 수, CPU/메모리 사용률
- 실행 중인 쿼리 수
- 쿼리 큐 및 실행 상태
- 쿼리 실행 시간 (p50, p95, p99)
- 메모리 풀 사용량
- 쿼리 상태별 처리량
- 쿼리 데이터 처리량 (입력/출력)
- Pod별 CPU 사용률

### 4. Airflow 상세 대시보드 (`airflow-dashboard.json`)
Airflow 워크플로우의 실행 상태를 추적합니다.

**주요 패널:**
- Pod 수, CPU/메모리 사용률
- 총 DAG 수
- DAG Run 상태별 수
- DAG 파싱 시간
- Task Instance 상태별 수
- Task 실행 시간 (p50, p95, p99)
- Scheduler Heartbeat
- Pod별 CPU 사용률

### 5. MinIO 상세 대시보드 (`minio-dashboard.json`)
MinIO 객체 스토리지의 성능 및 사용량을 모니터링합니다.

**주요 패널:**
- Pod 수, CPU/메모리/디스크 사용률
- 디스크 사용량 (Used/Available)
- S3 요청 유형별 처리량
- 네트워크 트래픽 (수신/송신)
- 주요 S3 작업 처리량 (GET/PUT/DELETE)
- S3 요청 지연 시간 (p50, p95, p99)
- Pod별 CPU 사용률

### 6. HMS 상세 대시보드 (`hms-dashboard.json`)
Hive Metastore의 메타데이터 관리 상태를 확인합니다.

**주요 패널:**
- Pod 수, CPU/메모리 사용률
- 총 요청 수
- 작업별 요청 처리량
- 요청 지연 시간 (p50, p95, p99)
- 요청 성공/실패율
- 메타데이터 통계 (Database/Table/Partition)
- 커넥션 풀 상태
- Pod별 CPU 사용률

### 7. Spark 상세 대시보드 (`spark-dashboard.json`)
Spark 애플리케이션의 실행 상태 및 성능을 모니터링합니다.

**주요 패널:**
- Pod 수, CPU/메모리 사용률
- 실행 중인 Job 수
- Job 상태 (Active/Completed/Failed)
- Stage 상태
- Executor 상태
- Executor 메모리 사용량
- I/O 처리량 (읽기/쓰기)
- Job 실행 시간 (p50, p95, p99)
- Pod별 CPU 사용률

### 8. Kubernetes 클러스터 리소스 대시보드 (`kubernetes-resources-dashboard.json`)
Kubernetes 클러스터의 하드웨어 리소스 사용량을 상세히 모니터링합니다.

**주요 패널:**
- 노드 수, Pod 수
- 요청된 CPU/메모리
- Pod별 CPU/메모리 사용률
- 클러스터 CPU/메모리 사용률 (전체 대비)
- Pod별 네트워크 수신/송신량
- Pod별 디스크 사용량 및 읽기 처리량

## 🚀 배포 방법

### 사전 요구사항

1. **Kubernetes 클러스터** 실행 중
2. **Prometheus** 설치 및 실행 중
3. **Grafana** 설치 및 실행 중
4. **Pod Monitor** 및 **Service Monitor** 설정 완료
5. 각 서비스의 메트릭 엔드포인트가 Prometheus에 의해 스크랩되고 있어야 함

### 1. Prometheus Data Source 설정

Grafana에서 Prometheus를 데이터 소스로 추가합니다:

1. Grafana UI 접속
2. Configuration → Data Sources → Add data source
3. Prometheus 선택
4. Prometheus 서버 URL 입력 (예: `http://prometheus:9090`)
5. Save & Test

### 2. 대시보드 임포트

각 대시보드를 Grafana에 임포트합니다:

#### 방법 1: Grafana UI를 통한 임포트

1. Grafana UI 접속
2. Dashboards → Import
3. 각 JSON 파일의 내용을 복사하여 붙여넣기
4. 또는 "Upload JSON file"을 통해 파일 업로드
5. Prometheus 데이터 소스 선택
6. Import 클릭

#### 방법 2: Grafana API를 통한 임포트

```bash
# Grafana API 토큰 설정
export GRAFANA_TOKEN="your-api-token"
export GRAFANA_URL="http://grafana:3000"

# 각 대시보드 임포트
for dashboard in *.json; do
  curl -X POST \
    -H "Authorization: Bearer $GRAFANA_TOKEN" \
    -H "Content-Type: application/json" \
    -d @$dashboard \
    "$GRAFANA_URL/api/dashboards/db"
done
```

#### 방법 3: ConfigMap을 통한 배포 (권장)

Kubernetes ConfigMap으로 대시보드를 배포하면 Grafana가 자동으로 로드합니다:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: grafana-dashboards
  namespace: monitoring
  labels:
    grafana_dashboard: "1"
data:
  overview-dashboard.json: |
    # overview-dashboard.json 내용
  kafka-dashboard.json: |
    # kafka-dashboard.json 내용
  # ... 나머지 대시보드들
```

Grafana가 ConfigMap을 자동으로 감지하도록 설정되어 있어야 합니다.

### 3. 변수 설정 확인

모든 대시보드는 다음 변수를 사용합니다:

- **`$namespace`**: 서비스가 실행되는 Kubernetes 네임스페이스 (자동 감지)
- **`$DS_PROMETHEUS`**: Prometheus 데이터 소스 (자동 감지)

대시보드 임포트 시 이 변수들이 자동으로 설정됩니다.

## 📊 메트릭 수집 설정

### Pod Monitor 예시

각 서비스에 대해 PodMonitor를 생성하여 메트릭을 수집합니다:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PodMonitor
metadata:
  name: kafka-podmonitor
  namespace: default
spec:
  selector:
    matchLabels:
      app: kafka
  podMetricsEndpoints:
  - port: metrics
    path: /metrics
    interval: 30s
```

### Service Monitor 예시

ServiceMonitor를 사용하는 경우:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: kafka-servicemonitor
  namespace: default
spec:
  selector:
    matchLabels:
      app: kafka
  endpoints:
  - port: metrics
    path: /metrics
    interval: 30s
```

## 🔧 커스터마이징

### 메트릭 이름 수정

대시보드의 메트릭 쿼리는 Prometheus에서 실제로 수집되는 메트릭 이름에 맞춰 조정해야 할 수 있습니다. 각 서비스의 메트릭 엔드포인트에서 제공하는 실제 메트릭 이름을 확인하고 필요시 수정하세요.

### 임계값 조정

대시보드의 Gauge 패널에는 임계값이 설정되어 있습니다. 환경에 맞게 조정하려면:

1. 대시보드 편집 모드 진입
2. 해당 패널 클릭
3. Panel options → Thresholds에서 값 수정

### 새 패널 추가

대시보드에 새로운 메트릭 패널을 추가하려면:

1. 대시보드 편집 모드 진입
2. Add panel 클릭
3. Prometheus 쿼리 작성
4. 패널 타입 선택 및 설정
5. Save

## 📝 참고사항

### 메트릭 이름 가정

대시보드는 다음과 같은 메트릭 이름을 가정합니다:

- **Kafka**: `kafka_*` 접두사
- **Trino**: `trino_*` 접두사
- **Airflow**: `airflow_*` 접두사
- **MinIO**: `minio_*` 접두사
- **HMS**: `hive_metastore_*` 또는 `hms_*` 접두사
- **Spark**: `spark_*` 접두사
- **Kubernetes**: `kube_*`, `container_*` 접두사

실제 환경의 메트릭 이름이 다를 수 있으므로, Prometheus에서 확인 후 필요시 수정하세요.

### 네임스페이스 필터링

모든 대시보드는 `$namespace` 변수를 사용하여 특정 네임스페이스의 메트릭만 표시합니다. 여러 네임스페이스에 걸쳐 있는 경우 변수를 "All"로 설정하거나 쿼리를 수정하세요.

## 🐛 문제 해결

### 메트릭이 표시되지 않는 경우

1. **Prometheus에서 메트릭 확인**
   ```bash
   curl http://prometheus:9090/api/v1/query?query=kafka_server_brokertopicmetrics_messagesin_total
   ```

2. **Pod/Service Monitor 확인**
   ```bash
   kubectl get podmonitors -n default
   kubectl get servicemonitors -n default
   ```

3. **Grafana 데이터 소스 연결 확인**
   - Configuration → Data Sources에서 Prometheus 연결 테스트

4. **대시보드 변수 확인**
   - 대시보드 상단의 변수 드롭다운에서 올바른 값 선택

### CPU/메모리 메트릭이 0으로 표시되는 경우

- `cAdvisor` 또는 `node-exporter`가 클러스터에 설치되어 있는지 확인
- Pod에 `resources` 설정이 올바르게 되어 있는지 확인

## 📚 추가 리소스

- [Grafana 공식 문서](https://grafana.com/docs/grafana/latest/)
- [Prometheus 공식 문서](https://prometheus.io/docs/)
- [Kubernetes 모니터링 가이드](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/)

## 📄 라이선스

이 대시보드 패키지는 학습 및 프로덕션 환경에서 자유롭게 사용할 수 있습니다.

