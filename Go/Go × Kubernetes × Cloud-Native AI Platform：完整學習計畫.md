> 適用對象：已具備 Go Web 後端、Gin、GORM、MySQL、Swagger、Docker 與基本分層架構經驗，希望進一步轉向 Kubernetes 平台工程、Operator、Multi-Cluster、平台安全與 AI Inference Infrastructure。
>
> 建議期間：32 週主線，每週約 10–12 小時。若每週可投入 15–20 小時，可壓縮至約 22–24 週；若仍需兼顧論文、工作或求職，則可延長至 36–40 週。

---

# 1. 學習定位與最終目標

你目前並不是 Go 初學者，也不需要重新從變數、迴圈、slice、map 開始。你已經具備一般 Web Backend 的基礎，後續真正需要補強的是：

1. Go 原生 HTTP 與系統程式設計能力。
2. Production-grade service 的可靠性、測試、效能分析與可觀測性。
3. Protocol Buffers、ConnectRPC、gRPC 與服務間通訊。
4. Kubernetes API object、controller、client-go 與 reconciliation model。
5. CRD、Kubebuilder、controller-runtime 與 Operator lifecycle。
6. 多租戶、OIDC、RBAC、mTLS、NetworkPolicy 與安全邊界。
7. Gateway API、Istio、儲存與平台資料面。
8. Multi-Cluster control plane 與 cluster agent。
9. KServe、GPU scheduling、模型生命週期與 inference routing。
10. 將上述能力整合成可展示、可部署、可測試的完整平台作品。

## 1.1 能力差距定位

| 面向 | 現有基礎 | 本計畫完成後應達程度 |
|---|---|---|
| Go | Gin/GORM、REST API、分層架構 | 熟悉 `net/http`、concurrency、testing、pprof、race detector、可靠服務設計 |
| API | REST、Swagger | ConnectRPC、gRPC、Protobuf、streaming、版本演進、錯誤模型 |
| Docker | 容器操作與 GPU container 經驗 | multi-stage build、non-root、image security、container debugging |
| Kubernetes | 基本概念或操作經驗 | 能除錯 workload、network、storage、RBAC，並理解 API machinery |
| Controller | 尚未系統化 | client-go、informer、workqueue、controller-runtime、reconcile |
| Platform Security | JWT 基礎 | OIDC、RBAC、多租戶、mTLS、NetworkPolicy、Secret 管理 |
| Multi-Cluster | 尚未建立完整模型 | cluster registry、agent、heartbeat、remote apply、斷線處理 |
| AI Infra | ML/資料分析背景 | KServe、模型部署、GPU scheduling、inference gateway、LLM serving |
| 系統設計 | 一般後端架構能力 | API layer、control plane、data plane、multi-tenant、eventual consistency |

---

# 2. 最終作品：Mini Cloud-Native AI Platform

整份學習計畫以一個逐步演化的主專案為核心，而不是每學一個工具就做一個互不相關的小專案。

## 2.1 平台願景

建立一套以 Go 開發的輕量雲原生 AI 平台，具備：

- ConnectRPC / Protobuf 平台 API。
- Tenant、Cluster、ModelService、InferenceRoute 等 Custom Resources。
- Kubernetes Operator 與 reconciliation control loop。
- Namespace、ResourceQuota、LimitRange、RBAC 與 NetworkPolicy 多租戶隔離。
- OIDC 登入與 tenant-aware authorization。
- Gateway API 對外路由。
- Istio mTLS 與 service-to-service authorization。
- Multi-Cluster registration、agent heartbeat 與 remote deployment。
- KServe 模型部署與模型狀態管理。
- GPU request、runtime 選擇與 inference endpoint 管理。
- Prometheus metrics、structured logs、tracing 與 audit trail。

## 2.2 整體架構

```text
                           ┌──────────────────────────────┐
                           │ Web UI / CLI / External API  │
                           └──────────────┬───────────────┘
                                          │ Connect / gRPC-Web
                           ┌──────────────▼───────────────┐
                           │ Platform API Server          │
                           │ Go + ConnectRPC + OIDC       │
                           └──────────────┬───────────────┘
                                          │ Create / Read CRs
                  ┌───────────────────────▼───────────────────────┐
                  │ Kubernetes Management Cluster                │
                  │                                               │
                  │ Tenant CRD          Cluster CRD               │
                  │ ModelService CRD    InferenceRoute CRD        │
                  │                                               │
                  │ Controllers / Operators                       │
                  └───────────────┬───────────────────┬───────────┘
                                  │                   │
                         Local resources         Remote agent / API
                                  │                   │
              ┌───────────────────▼──────┐  ┌────────▼──────────────┐
              │ Workload / Data Plane    │  │ Member Clusters       │
              │ KServe / Gateway / Istio │  │ Agent + Workloads     │
              │ Storage / GPU / Metrics  │  │ KServe / GPU          │
              └──────────────────────────┘  └───────────────────────┘
```

## 2.3 平台核心資源

### Tenant

表達租戶、namespace、quota、角色與隔離政策。

### Cluster

表達成員叢集註冊資訊、連線狀態、版本、容量與健康狀態。

### ModelService

表達模型來源、runtime、資源需求、伸縮策略與 exposure 設定。

### InferenceRoute

表達模型流量入口、path、權重、版本與 tenant routing。

---

# 3. 學習原則

## 3.1 主線優先，工具從屬

學習順序應由底層模型逐步往上：

```text
Go runtime / context / HTTP
→ RPC contract
→ Container
→ Kubernetes object model
→ client-go control loop
→ CRD / Operator
→ Security / Networking / Storage
→ Multi-Cluster
→ KServe / Inference Platform
```

不要反過來先背 Helm 指令、安裝 Istio、抄 Kubebuilder 教學，再試圖補理解。

## 3.2 每週投入比例

```text
20% 官方文件與概念閱讀
60% 實作、測試、除錯
20% README、架構圖、技術筆記與回顧
```

## 3.3 每週固定產出

每週至少留下：

- 一組可追蹤的 Git commit。
- 一份技術筆記。
- 一個測試或驗收腳本。
- 一段可重現的 demo 指令。
- 一項踩坑紀錄與根因分析。
- 一個「這項技術解決什麼問題」的口頭說明。

## 3.4 Definition of Done

一個主題不以「看完文章」算完成，而需至少符合：

1. 能自行實作最小版本。
2. 能寫測試或驗證腳本。
3. 能說明設計取捨。
4. 能刻意製造錯誤並完成除錯。
5. 能把功能整合進主專案。
6. 能寫出 README 使用方式與已知限制。

---

# 4. 32 週完整學習路線

---

## Phase 0：基線盤點與開發環境（第 0 週）

### 目標

建立統一開發環境、主專案骨架、版本策略與學習追蹤方式，避免後續被環境問題反覆中斷。

### 必學與設定

- Go module、workspace 與 toolchain 版本管理。
- `golangci-lint`、`go vet`、`staticcheck`。
- Makefile 或 Taskfile。
- Git branch、commit convention、tag 與 release note。
- Docker、Docker Compose。
- `kind` 或 `k3d`。
- `kubectl`、`helm`、`kustomize`。
- `buf`、`protoc`、ConnectRPC code generation。
- Kubebuilder CLI。
- 可選：Tilt、Skaffold 或 DevSpace 做本機迭代。

### 主專案初始結構

```text
mini-ai-platform/
├── api/
│   ├── proto/
│   └── generated/
├── cmd/
│   ├── platform-api/
│   ├── cluster-agent/
│   └── controller-manager/
├── internal/
│   ├── application/
│   ├── domain/
│   ├── transport/
│   ├── infrastructure/
│   ├── observability/
│   └── security/
├── operator/
│   ├── api/
│   ├── controllers/
│   └── webhooks/
├── deploy/
│   ├── base/
│   ├── overlays/
│   └── helm/
├── test/
│   ├── integration/
│   └── e2e/
├── docs/
│   ├── architecture/
│   ├── adr/
│   └── notes/
├── Makefile
└── README.md
```

### 驗收標準

- `make lint`、`make test`、`make build` 可執行。
- 本機可建立 kind cluster。
- GitHub Actions 可跑 lint 與 unit test。
- README 清楚記載需求版本與啟動方式。

---

## Phase 1：Go 原生模型與系統程式設計（第 1–4 週）

### Phase 目標

從「會使用 Gin 寫 API」進一步提升到「理解 Go server、context、concurrency 與 runtime 行為」。

---

### 第 1 週：`net/http`、Handler 與 Server Lifecycle

#### 必學內容

- `http.Handler` 與 `http.HandlerFunc`。
- `ServeHTTP` 呼叫模型。
- `http.ServeMux` 的 method/path routing。
- request、response、header、status code。
- JSON encode/decode。
- `http.Server`。
- read timeout、write timeout、idle timeout、header timeout。
- graceful shutdown。
- signal handling。
- `http.MaxBytesReader` 與 request body 限制。
- header already written 問題。
- keep-alive 與 connection reuse 基本概念。

#### 實作任務

建立 `job-api`：

```text
POST   /jobs
GET    /jobs/{id}
DELETE /jobs/{id}
GET    /healthz
GET    /readyz
```

限制：

- 使用純 `net/http` 或標準 `ServeMux`。
- 暫不使用 Gin。
- 統一 JSON response。
- 實作 request body size limit。
- 正確處理 malformed JSON 與 unknown field。
- 加入 graceful shutdown。

#### 驗收標準

- SIGTERM 後不再接受新請求，並等待既有請求結束。
- timeout 能實際中止慢請求。
- 所有 handler 有 `httptest` 測試。
- 能口頭說明 Gin `Engine` 與 `http.Handler` 的關係。

---

### 第 2 週：Context、Cancellation 與 Concurrency

#### 必學內容

- `context.Background`、`TODO`、`WithCancel`、`WithTimeout`、`WithDeadline`。
- request context lifecycle。
- context value 的適用與不適用情境。
- goroutine lifecycle。
- channel、buffered channel、select。
- worker pool。
- fan-out / fan-in。
- goroutine leak。
- `sync.Mutex`、`RWMutex`、`WaitGroup`、`Once`。
- `errgroup.Group`。
- backpressure。
- shutdown sequence。

#### 實作任務

擴充 Job API：

- Job 由 worker pool 非同步執行。
- 支援取消 Job。
- API request 中斷時，可依需求決定是否取消下游工作。
- 服務關閉時停止接收新 Job，等待或取消未完成 Job。
- 為 worker queue 加入容量限制。

#### 故障實驗

- 故意製造 goroutine leak。
- 故意關閉錯誤的 channel。
- 故意產生 data race。
- 使用 `go test -race ./...` 驗證修復。

#### 驗收標準

- race detector 通過。
- goroutine profile 無持續增長。
- 能說明 cancellation propagation 與 backpressure。

---

### 第 3 週：錯誤設計、Logging 與 Configuration

#### 必學內容

- sentinel error。
- typed error。
- error wrapping。
- `errors.Is`、`errors.As`。
- domain error 與 transport error mapping。
- retryable 與 non-retryable error。
- `log/slog` structured logging。
- request ID、trace ID、tenant ID。
- 避免記錄 secret、token 與敏感 payload。
- environment-based configuration。
- config validation。
- immutable configuration。
- startup fail-fast。
- secrets 與普通 configuration 的區別。

#### 實作任務

- 定義 `ErrJobNotFound`、`ErrQueueFull`、`ErrConflict`。
- 統一 API error schema。
- 日誌包含 request ID、method、path、duration、status。
- config 缺漏時拒絕啟動。
- 加入 log level 與 JSON log。

#### 驗收標準

- Repository error 不直接洩漏到 HTTP response。
- 可從一筆錯誤日誌追蹤完整 request。
- 測試驗證錯誤映射與 log field。

---

### 第 4 週：Testing、Benchmark、pprof 與品質工具

#### 必學內容

- table-driven test。
- subtest。
- interface boundary。
- fake、stub、mock、spy 的差異。
- `httptest.Server`。
- integration test。
- benchmark。
- allocation。
- CPU、heap、goroutine、mutex profile。
- fuzz test 基礎。
- coverage 的正確解讀。
- deterministic test。
- flaky test 常見原因。

#### 實作任務

- Service unit tests。
- Handler tests。
- Repository fake。
- benchmark JSON encoding 與 queue operation。
- 使用 pprof 找出一個刻意製造的 allocation hotspot。
- 寫一個 fuzz test 測 JSON input parser。

#### Phase 1 產出

- Production-ready Job API。
- 一份 `net/http` 與 Gin 抽象比較筆記。
- 一份 concurrency 與 cancellation 架構圖。
- 一份效能分析紀錄。

---

## Phase 2：Production HTTP、Chi、Gin 邊界與 RPC（第 5–7 週）

### Phase 目標

理解框架取捨，並建立對外 REST / Connect 與內部 RPC 契約。

---

### 第 5 週：Chi、Middleware Composition 與 Clean Boundary

#### 必學內容

- chi router 與標準 `http.Handler` 相容性。
- route group。
- middleware chaining。
- authentication middleware。
- authorization middleware。
- panic recovery。
- CORS。
- rate limiting 基礎。
- idempotency key。
- HTTP adapter 與 application service 分離。

#### 架構原則

```text
HTTP / Gin / Chi Context
           ↓
       Handler DTO
           ↓
   Application Service
           ↓
    Domain / Repository
```

- `*gin.Context` 不進入 application service。
- HTTP status code 不出現在 domain layer。
- DB model 不直接作為 API response。

#### 實作任務

- 將 Job API routing 改為 chi。
- 或保留 Gin，但確保 Gin 僅存在 transport layer。
- 實作 request ID、auth、recovery、timeout middleware。
- 撰寫 Architecture Decision Record：為何選 chi、Gin 或 `net/http`。

---

### 第 6 週：Protocol Buffers 與 API Contract

#### 必學內容

- message、field、enum、service。
- field number 與 wire compatibility。
- optional、repeated、map。
- timestamp、duration、well-known types。
- package 與 version 命名。
- breaking change。
- backward / forward compatibility。
- `buf lint`、`buf breaking`。
- generated code 與手寫 code 的邊界。
- API pagination、filter、sort、resource name。

#### 實作任務

定義：

```proto
service JobService {
  rpc CreateJob(CreateJobRequest) returns (CreateJobResponse);
  rpc GetJob(GetJobRequest) returns (GetJobResponse);
  rpc CancelJob(CancelJobRequest) returns (CancelJobResponse);
  rpc WatchJob(WatchJobRequest) returns (stream JobEvent);
}
```

#### 驗收標準

- `buf lint` 通過。
- 建立 breaking change check。
- 能解釋為何不能重複使用已刪除 field number。

---

### 第 7 週：ConnectRPC、gRPC Semantics 與 Resilience

#### 必學內容

- Connect protocol、gRPC、gRPC-Web。
- unary RPC。
- server streaming。
- client streaming 與 bidirectional streaming 的概念。
- interceptor。
- metadata / header。
- deadline 與 cancellation。
- RPC status code。
- retry policy。
- exponential backoff。
- jitter。
- idempotency。
- circuit breaker 概念。
- request hedging 的風險。

#### 實作任務

- 將 Job Service 提供 ConnectRPC endpoint。
- REST endpoint 可作為外部相容層。
- `WatchJob` 透過 streaming 推送狀態。
- 建立 auth interceptor。
- 建立 logging、metrics interceptor。
- 模擬 timeout、retry 與 duplicate request。

#### Phase 2 產出

- ConnectRPC Job Service。
- `.proto` contract 與 API versioning 文件。
- REST 與 RPC 選擇比較。
- 一組 resilience test。

---

## Phase 3：Container 與 Kubernetes 核心（第 8–10 週）

### Phase 目標

把服務可靠地容器化、部署到 Kubernetes，並具備基礎除錯能力。

---

### 第 8 週：Production Container Image

#### 必學內容

- multi-stage build。
- distroless / scratch / alpine 取捨。
- CGO 與 static binary。
- non-root user。
- read-only filesystem。
- signal handling。
- image layer cache。
- `.dockerignore`。
- architecture / platform build。
- SBOM 基礎。
- image vulnerability scanning。
- OCI image、registry、tag 與 digest。

#### 實作任務

- 建立小型、安全的 Job API image。
- 使用 non-root。
- 設定 read-only root filesystem。
- 建立 healthcheck。
- 使用 image digest 部署。
- 產生 SBOM，執行 vulnerability scan。

---

### 第 9 週：Kubernetes Workload、Config 與 Resource Management

#### 必學資源

- Pod。
- ReplicaSet。
- Deployment。
- StatefulSet。
- Job / CronJob。
- Service。
- ConfigMap。
- Secret。
- Namespace。
- requests / limits。
- liveness、readiness、startup probe。
- rolling update。
- rollback。
- affinity、anti-affinity。
- taint、toleration。
- topology spread constraints。
- priority class 基礎。

#### 實作任務

- 將 Job API 部署到 kind。
- 加入 probes。
- 設定 requests / limits。
- 執行 rolling update 與 rollback。
- 刻意製造 OOMKilled、CrashLoopBackOff、ImagePullBackOff。

#### 驗收標準

能使用：

```bash
kubectl get
kubectl describe
kubectl logs
kubectl exec
kubectl top
kubectl events
```

完成基本根因分析。

---

### 第 10 週：Kubernetes Network、Storage 與 RBAC 基礎

#### 必學內容

- ClusterIP、NodePort、LoadBalancer。
- DNS 與 service discovery。
- Ingress 與 Gateway API 的角色差異。
- CNI 基本概念。
- NetworkPolicy。
- PV、PVC、StorageClass。
- volume lifecycle。
- ServiceAccount。
- Role、ClusterRole。
- RoleBinding、ClusterRoleBinding。
- least privilege。
- `kubectl auth can-i`。

#### 實作任務

- 建立 `tenant-a` namespace。
- 建立 ServiceAccount。
- Role 只允許讀 Pod。
- 使用 NetworkPolicy 限制服務流量。
- 建立 PVC 並驗證 Pod 重建後資料仍存在。
- 建立 Gateway / HTTPRoute 的最小範例。

#### Phase 3 產出

- Kubernetes manifests 或 Kustomize overlays。
- Kubernetes troubleshooting runbook。
- RBAC 權限矩陣。
- Container security checklist。

---

## Phase 4：Kubernetes API Machinery、client-go 與自製 Controller（第 11–13 週）

### Phase 目標

先理解 Operator 底層 control loop，再使用高階框架。

---

### 第 11 週：Kubernetes API Object 與 Client

#### 必學內容

- Group、Version、Kind。
- resource 與 subresource。
- metadata、spec、status。
- labels、annotations、selectors。
- UID、generation、resourceVersion。
- optimistic concurrency。
- watch。
- list-watch。
- API discovery。
- typed client。
- dynamic client。
- RESTMapper。
- unstructured object。
- server-side apply。
- patch：merge、strategic merge、JSON patch。

#### 實作任務

建立 CLI：

- 列出 namespace 內 Deployment。
- 顯示 desired / available replicas。
- 使用 label selector 篩選。
- 使用 patch 修改 annotation。
- 處理 resourceVersion conflict。

---

### 第 12 週：Informer、Cache、Workqueue

#### 必學內容

- SharedInformer。
- local cache。
- lister。
- event handler。
- workqueue。
- rate-limiting queue。
- key-based processing。
- resync。
- at-least-once delivery。
- event coalescing。
- controller 不應依賴事件順序。
- cache stale read。

#### 實作任務

建立 Deployment Health Controller：

```text
Watch Deployment
→ 將 namespace/name 放入 queue
→ worker 讀取 key
→ 從 cache 取得 object
→ 比較 desired 與 available replicas
→ 輸出 metric / Event
→ 失敗時 rate-limited retry
```

#### 驗收標準

- 支援多 worker。
- shutdown 時正確關閉 queue。
- transient error 會重試。
- permanent error 不無限重試。

---

### 第 13 週：Controller Reliability

#### 必學內容

- idempotency。
- level-triggered 與 edge-triggered。
- reconcile convergence。
- eventual consistency。
- rate limiting。
- exponential backoff。
- conflict retry。
- duplicate event。
- cache delay。
- leader election。
- controller metrics。
- Kubernetes Event。

#### 實作任務

- 對自製 controller 加入 metrics。
- 模擬 API server 暫時失敗。
- 模擬 watch restart。
- 模擬 object 快速連續更新。
- 寫 controller unit test。

#### Phase 4 產出

- client-go Deployment Health Controller。
- control loop 架構圖。
- 一份「事件不等於狀態」的技術筆記。
- controller reliability 測試報告。

---

## Phase 5：Kubebuilder、CRD 與 Tenant Operator（第 14–18 週）

### Phase 目標

掌握正式 Operator 開發流程與 Kubernetes-native API 設計。

---

### 第 14 週：Kubebuilder 與 CRD API Design

#### 必學內容

- Kubebuilder project layout。
- API group / version。
- markers。
- deepcopy。
- CRD schema。
- structural schema。
- OpenAPI validation。
- defaulting。
- enum、pattern、minimum、maximum。
- printer columns。
- status subresource。
- short name、category。

#### 第一個 CRD：Tenant

```yaml
apiVersion: platform.example.com/v1alpha1
kind: Tenant
metadata:
  name: team-a
spec:
  displayName: Team A
  namespaces:
    - team-a-dev
    - team-a-prod
  quota:
    cpu: "4"
    memory: "8Gi"
    pods: 20
  defaultLimits:
    cpu: "1"
    memory: "1Gi"
status:
  observedGeneration: 1
  phase: Ready
  conditions: []
```

#### 驗收標準

- 無效 quota 無法通過 API validation。
- `kubectl get tenant` 顯示 phase。
- spec 與 status 責任清楚。

---

### 第 15 週：Reconcile Loop 與 Child Resources

#### Controller 自動建立

- Namespace。
- ResourceQuota。
- LimitRange。
- ServiceAccount。
- Role。
- RoleBinding。
- NetworkPolicy。

#### 必學內容

- `controller-runtime` client。
- `CreateOrUpdate` pattern。
- controller reference。
- owner reference。
- `Owns` watch。
- desired resource builder。
- immutable field 處理。
- status patch。

#### 驗收標準

- 重複 reconcile 不會建立重複資源。
- 手動刪除 child resource 後可自動補回。
- 修改 Tenant spec 後 child resource 會收斂。

---

### 第 16 週：Conditions、Status 與 ObservedGeneration

#### 必學內容

- Ready、Progressing、Degraded conditions。
- `status.observedGeneration`。
- reason、message、lastTransitionTime。
- phase 與 conditions 的角色。
- status update conflict。
- user-facing status。

#### 實作任務

狀態範例：

```yaml
status:
  observedGeneration: 3
  conditions:
    - type: Ready
      status: "False"
      reason: RBACProvisioningFailed
      message: failed to create RoleBinding
```

#### 驗收標準

- 使用者可從 status 判斷目前發生什麼事。
- spec 新版本尚未處理時可辨識。
- condition transition 正確。

---

### 第 17 週：Finalizer、Deletion 與 External Resource Cleanup

#### 必學內容

- deletionTimestamp。
- finalizer lifecycle。
- external resource cleanup。
- stuck terminating。
- cleanup idempotency。
- timeout 與 force delete 的風險。

#### 實作任務

- Tenant 刪除前清理模擬外部 registry record。
- cleanup 失敗時保留 finalizer 並重試。
- cleanup 成功後移除 finalizer。
- 提供 runbook 處理 stuck finalizer。

---

### 第 18 週：Webhook、版本演進與 Operator Testing

#### 必學內容

- validating webhook。
- mutating webhook。
- defaulting webhook。
- conversion webhook。
- `v1alpha1`、`v1beta1`、`v1`。
- hub / spoke conversion。
- envtest。
- fake client 限制。
- controller integration test。
- end-to-end test。

#### 實作任務

- Tenant default quota webhook。
- 驗證 namespace 命名規則。
- envtest 驗證 reconcile。
- kind e2e 驗證完整 child resource。

#### Phase 5 產出

- Tenant Operator。
- CRD API 文件。
- condition 與 error catalog。
- operator e2e test。
- upgrade / deletion runbook。

---

## Phase 6：平台 API、OIDC、多租戶安全與可觀測性（第 19–21 週）

### Phase 目標

建立使用者入口、雙層授權模型與完整可觀測性。

---

### 第 19 週：Platform API Server 與 TenantService

#### 必學內容

- API layer 與 Kubernetes client boundary。
- ConnectRPC service implementation。
- DTO / CR mapping。
- pagination。
- field mask 基礎。
- request validation。
- idempotency key。
- audit metadata。
- long-running operation pattern。

#### 實作任務

```proto
service TenantService {
  rpc CreateTenant(CreateTenantRequest) returns (Tenant);
  rpc GetTenant(GetTenantRequest) returns (Tenant);
  rpc ListTenants(ListTenantsRequest) returns (ListTenantsResponse);
  rpc DeleteTenant(DeleteTenantRequest) returns (DeleteTenantResponse);
  rpc WatchTenant(WatchTenantRequest) returns (stream TenantEvent);
}
```

後端流程：

```text
API Request
→ AuthN / AuthZ
→ Validate
→ Create Tenant CR
→ Operator Reconcile
→ Stream / Poll Status
```

---

### 第 20 週：OIDC、JWT 與 Authorization

#### 必學內容

- OAuth 2.0 與 OIDC 差異。
- Authorization Code flow。
- ID token 與 access token。
- issuer、audience、subject。
- JWKS 與 key rotation。
- token expiry。
- refresh token 基礎。
- role / group claims。
- tenant-aware authorization。
- confused deputy problem。
- RBAC 與 ABAC 概念。

#### 實作任務

- 使用 Dex 或 Keycloak。
- API server 驗證 issuer、audience、signature、expiry。
- claims：

```json
{
  "sub": "user-a",
  "tenant": "team-a",
  "roles": ["tenant-admin"]
}
```

- tenant-admin 只能操作自己的 Tenant 與 ModelService。
- platform-admin 可跨 tenant 管理。
- Kubernetes ServiceAccount 權限再做第二層限制。

#### 安全原則

```text
UI 隱藏按鈕：不是安全控制
API authorization：第一層
Kubernetes RBAC：第二層
Namespace / NetworkPolicy / Quota：隔離層
```

---

### 第 21 週：Metrics、Tracing、Logging 與 Audit

#### 必學內容

- RED metrics：Rate、Errors、Duration。
- USE metrics：Utilization、Saturation、Errors。
- Prometheus metric types。
- cardinality 風險。
- OpenTelemetry trace。
- span propagation。
- structured log correlation。
- audit log。
- SLI、SLO、error budget 基礎。

#### 實作任務

API metrics：

- request count。
- request latency。
- error count。
- active request。

Controller metrics：

- reconcile count。
- reconcile duration。
- error count。
- queue depth。
- ready tenant count。

Tracing：

```text
Platform API span
→ Kubernetes API request span
→ Operator reconcile log correlation
```

#### Phase 6 產出

- Tenant-aware Platform API。
- OIDC login / token verification demo。
- Authorization matrix。
- Grafana dashboard。
- 基礎 SLO 文件。

---

## Phase 7：Gateway API、Istio 與 Storage（第 22–24 週）

### Phase 目標

理解平台資料面的入口、安全、流量與儲存，而不是只會安裝套件。

---

### 第 22 週：Gateway API

#### 必學內容

- GatewayClass。
- Gateway。
- HTTPRoute。
- GRPCRoute。
- ReferenceGrant。
- listener。
- hostname / path matching。
- route attachment。
- cross-namespace reference。
- TLS termination。
- controller implementation 與標準 API 的區別。

#### 實作任務

- 使用 Gateway API 暴露 Platform API。
- 為不同 tenant 建立 route。
- 配置 TLS。
- 驗證 route status conditions。
- 比較 Ingress 與 Gateway API。

---

### 第 23 週：Istio Traffic、mTLS 與 AuthorizationPolicy

#### 必學內容

- service mesh control plane / data plane。
- sidecar 或 ambient mode 基礎概念。
- PeerAuthentication。
- DestinationRule。
- VirtualService 與 Gateway API 整合概念。
- service identity。
- mTLS STRICT / PERMISSIVE。
- AuthorizationPolicy。
- retries、timeouts、circuit breaking。
- traffic shifting。
- observability。

#### 實作任務

- 兩個 Go service 透過 mesh 通訊。
- 啟用 STRICT mTLS。
- 只允許特定 ServiceAccount 呼叫 model service。
- 建立 90/10 canary routing。
- 模擬 retry storm 並調整 policy。

---

### 第 24 週：Storage、Rook/Ceph 與模型資料

#### 必學內容

- block、file、object storage。
- access mode。
- reclaim policy。
- volume expansion。
- snapshot 基礎。
- CSI。
- Rook Operator 與 Ceph resource 基礎。
- object storage / S3 compatible API。
- 模型 artifact 與 runtime image 分離。
- data locality。

#### 實作任務

- 使用簡化本機 storage class 完成 PVC 流程。
- 若環境允許，安裝 Rook/Ceph 測試環境。
- 建立模型 artifact storage。
- 驗證 Pod 重建與節點變更後資料行為。
- 寫 storage failure runbook。

#### Phase 7 產出

- Gateway / HTTPRoute manifests。
- Istio mTLS 與 authorization demo。
- canary routing demo。
- 模型 storage 設計文件。

---

## Phase 8：Multi-Cluster Control Plane（第 25–27 週）

### Phase 目標

建立可管理多個 Kubernetes cluster 的平台核心能力。

---

### 第 25 週：Cluster Registry 與 Multi-Cluster Client

#### 必學內容

- kubeconfig 與 `rest.Config`。
- per-cluster client cache。
- cluster credentials lifecycle。
- cluster identity。
- capability discovery。
- Kubernetes version compatibility。
- health check。
- heartbeat。
- credential encryption。
- agentless 與 agent-based 模式。

#### Cluster CRD 範例

```yaml
apiVersion: platform.example.com/v1alpha1
kind: Cluster
metadata:
  name: edge-a
spec:
  connectionMode: Agent
  tenant: team-a
status:
  kubernetesVersion: v1.xx.x
  nodeCount: 3
  capacity:
    gpu: 2
  lastHeartbeatTime: "..."
  phase: Ready
```

#### 實作任務

- 註冊第二個 kind cluster。
- 建立 cluster client factory。
- 顯示 version、node count、capacity。
- 定期 health check。

---

### 第 26 週：Cluster Agent 與 ConnectRPC Streaming

#### 必學內容

- outbound agent connection。
- reverse connection 概念。
- heartbeat stream。
- reconnect。
- exponential backoff + jitter。
- sequence number。
- duplicate message。
- message acknowledgement。
- offline queue。
- eventual consistency。
- certificate bootstrap 概念。

#### 實作任務

建立 `cluster-agent`：

```text
Agent 啟動
→ 讀取 cluster identity
→ ConnectRPC 連線 management plane
→ 回報 heartbeat / version / capacity
→ 接收 deployment instruction
→ apply resource
→ 回報 result
```

---

### 第 27 週：Remote Apply、Failure Handling 與 Scheduling Decision

#### 必學內容

- server-side apply。
- field manager。
- drift detection。
- desired state replication。
- cluster unavailable。
- partial failure。
- timeout。
- retry budget。
- placement policy。
- capacity-aware scheduling。
- GPU capability。
- tenant policy。

#### 實作任務

- 將 demo workload 部署到指定 member cluster。
- cluster 斷線時狀態標為 Unknown / Unavailable。
- 重連後重新同步 desired state。
- 實作簡單 placement：依 GPU 數量與 tenant allowlist 選 cluster。

#### Phase 8 產出

- Multi-Cluster Manager。
- Cluster Agent。
- Cluster CRD。
- 斷線與重連測試。
- placement decision 文件。

---

## Phase 9：KServe、ModelService 與 AI Inference Infrastructure（第 28–30 週）

### Phase 目標

把平台能力延伸到模型部署、GPU、runtime、route 與 lifecycle。

---

### 第 28 週：模型服務與 KServe 基礎

#### 必學內容

- model artifact。
- inference runtime。
- predictor / transformer / explainer 概念。
- KServe InferenceService。
- storage URI。
- runtime image。
- readiness / model load。
- autoscaling。
- scale-to-zero。
- cold start。
- protocol：V1 / V2 inference protocol 基礎。
- predictive AI 與 generative AI serving 差異。

#### 實作任務

- 部署一個小型 CPU model。
- 呼叫 inference endpoint。
- 觀察 InferenceService status。
- 模擬 model URI 錯誤。
- 比較 raw Deployment 與 KServe 的抽象價值。

---

### 第 29 週：ModelService Operator

#### CRD 範例

```yaml
apiVersion: platform.example.com/v1alpha1
kind: ModelService
metadata:
  name: demo-model
spec:
  tenantRef: team-a
  clusterRef: edge-a
  model:
    uri: s3://models/demo-model
    format: sklearn
  runtime:
    name: sklearn
  resources:
    requests:
      cpu: "1"
      memory: "2Gi"
  autoscaling:
    minReplicas: 0
    maxReplicas: 3
  exposure:
    type: Private
    path: /v1/models/demo-model
status:
  observedGeneration: 1
  phase: Ready
  endpoint: https://example.com/v1/models/demo-model
  conditions: []
```

#### Controller 負責

- 驗證 tenant 與 cluster。
- 建立 KServe InferenceService。
- 建立 ServiceAccount / RBAC。
- 建立 Gateway API route。
- 建立 NetworkPolicy。
- 監看 KServe readiness。
- 回寫 endpoint。
- 回寫 ModelLoadFailed、RoutingFailed、InsufficientGPU 等 conditions。

#### 驗收標準

- 重複 reconcile idempotent。
- 刪除 ModelService 可清理 child resources。
- runtime / resource 更新能正確 rollout。

---

### 第 30 週：GPU Scheduling、LLM Runtime 與 Inference Routing

#### 必學內容

- GPU device plugin。
- resource request。
- node label。
- taint / toleration。
- node affinity。
- GPU fragmentation 概念。
- MIG 基礎。
- batch、latency、throughput 取捨。
- vLLM / TGI / Triton 等 runtime 的定位。
- model parallelism 基礎概念。
- KV cache 概念。
- canary model version。
- weighted routing。
- inference-specific routing。
- Gateway API Inference Extension 的定位。

#### 實作任務

若有 GPU：

- 部署小型 vLLM 或其他 runtime。
- 設定 GPU request。
- 觀察 scheduling 與 utilization。

若沒有 GPU：

- 使用 CPU 小模型或 mock inference server。
- 以 node label / extended resource 模擬 GPU placement。
- 專注完成 control plane 與 routing。

#### Phase 9 產出

- ModelService Operator。
- KServe demo。
- model lifecycle 狀態機。
- GPU scheduling 筆記。
- inference routing demo。

---

## Phase 10：Capstone 整合、可靠性與求職化（第 31–32 週）

### Phase 目標

把所有模組整合成可展示、可部署、可說明設計取捨的作品。

---

### 第 31 週：整合測試、Failure Injection 與 Security Hardening

#### 測試層級

- Unit test。
- Integration test。
- envtest。
- kind e2e。
- multi-cluster e2e。
- API contract test。
- authorization test。
- failure injection。

#### 故障情境

- API server restart。
- operator restart。
- Kubernetes API timeout。
- member cluster offline。
- duplicate request。
- token expired。
- RBAC denied。
- model load failed。
- gateway route failed。
- insufficient GPU。
- storage unavailable。
- stuck finalizer。

#### Security checklist

- non-root container。
- read-only root filesystem。
- least privilege RBAC。
- no hardcoded secret。
- token validation complete。
- NetworkPolicy default deny。
- mTLS。
- audit log。
- dependency scan。
- image scan。
- secret redaction。

---

### 第 32 週：文件、Demo、履歷與面試準備

#### 必備文件

- 根目錄 README。
- 一鍵啟動流程。
- architecture diagram。
- sequence diagram。
- CRD reference。
- API reference。
- security model。
- multi-cluster design。
- failure handling。
- testing strategy。
- known limitations。
- roadmap。
- ADR。

#### Demo 流程

1. 使用 OIDC 登入。
2. 建立 Tenant。
3. Tenant Operator 建立 namespace、quota、RBAC。
4. 註冊 member cluster。
5. 建立 ModelService。
6. placement 選擇 cluster。
7. Operator 建立 KServe resource。
8. Gateway 建立 endpoint。
9. 呼叫模型推論。
10. 顯示 metrics、trace 與 status。
11. 模擬 cluster offline。
12. 顯示平台錯誤狀態與恢復過程。

#### 履歷可寫成果

- Built a Go-based Kubernetes control plane using ConnectRPC, CRDs and controller-runtime.
- Designed tenant isolation with OIDC, RBAC, Namespace, ResourceQuota and NetworkPolicy.
- Implemented a multi-cluster agent with streaming heartbeat, reconnect and remote resource application.
- Developed a ModelService Operator integrating KServe, Gateway API and GPU-aware placement.
- Added production observability, end-to-end testing, failure handling and security hardening.

---

# 5. 每週總表

| 週次 | 主題 | 核心產出 |
|---:|---|---|
| 0 | 環境、Repo、CI | 主專案骨架、lint/test/build pipeline |
| 1 | `net/http` | Job API、graceful shutdown、timeout |
| 2 | Concurrency / Context | worker pool、cancellation、race-free |
| 3 | Error / Logging / Config | 統一錯誤、structured logs、config validation |
| 4 | Test / Benchmark / pprof | 測試、效能分析、fuzz |
| 5 | Chi / Middleware / Boundary | framework adapter 與 clean boundary |
| 6 | Protobuf | API contract、buf lint/breaking |
| 7 | ConnectRPC / gRPC | RPC server、streaming、interceptors |
| 8 | Container | secure multi-stage image、SBOM |
| 9 | Kubernetes Workload | Deployment、probes、resources、debug |
| 10 | Network / Storage / RBAC | Service、Gateway、PVC、RBAC、NetworkPolicy |
| 11 | Kubernetes API Client | typed/dynamic client、patch、SSA |
| 12 | Informer / Queue | 自製 controller |
| 13 | Controller Reliability | idempotency、retry、metrics、leader election |
| 14 | CRD Design | Tenant CRD |
| 15 | Reconcile / Child Resource | Namespace、Quota、RBAC 自動化 |
| 16 | Status / Conditions | observedGeneration、condition model |
| 17 | Finalizer | external cleanup lifecycle |
| 18 | Webhook / Testing | validation、defaulting、envtest、e2e |
| 19 | Platform API | TenantService、CR mapping |
| 20 | OIDC / Authorization | tenant-aware access control |
| 21 | Observability | metrics、traces、logs、SLO |
| 22 | Gateway API | Gateway、HTTPRoute、TLS |
| 23 | Istio | mTLS、AuthorizationPolicy、canary |
| 24 | Storage | PVC、CSI、Rook/Ceph 基礎 |
| 25 | Cluster Registry | Cluster CRD、multi-client |
| 26 | Cluster Agent | heartbeat、stream、reconnect |
| 27 | Remote Apply | drift、placement、offline recovery |
| 28 | KServe | InferenceService、model lifecycle |
| 29 | ModelService Operator | KServe + Gateway + status |
| 30 | GPU / LLM Routing | GPU placement、runtime、inference routing |
| 31 | Hardening | e2e、failure injection、security |
| 32 | Portfolio | demo、docs、履歷、面試 |

---

# 6. 建議專案演化順序

不要把作品拆成五個完全獨立 repository。建議以同一個平台逐步演化：

```text
Job API
→ ConnectRPC Job Service
→ Kubernetes Deployment
→ client-go Controller
→ Tenant Operator
→ Platform API
→ Secure Multi-Tenant Platform
→ Multi-Cluster Manager
→ ModelService Operator
→ Cloud-Native AI Platform
```

可在 monorepo 中保留不同 milestone tag：

```text
v0.1-go-http
v0.2-connectrpc
v0.3-kubernetes
v0.4-tenant-operator
v0.5-secure-platform
v0.6-multicluster
v1.0-ai-platform
```

---

# 7. 每個階段應回答的面試問題

## Go 與服務設計

- `http.Handler` 的模型是什麼？
- Gin 為何不應進入 service layer？
- context 如何傳遞 cancellation？
- 如何避免 goroutine leak？
- Mutex、channel、atomic 如何選？
- graceful shutdown 的正確順序是什麼？
- 如何區分 retryable error？
- 如何使用 pprof 找 CPU 或 memory 問題？

## RPC

- REST、gRPC、ConnectRPC 的差異？
- Protobuf 如何維持 backward compatibility？
- deadline 與 retry 如何互動？
- 哪些操作不適合自動 retry？
- streaming 遇到斷線如何恢復？

## Kubernetes

- Deployment、StatefulSet、Job 如何選？
- readiness 與 liveness 差在哪？
- requests / limits 如何影響 scheduling 與 QoS？
- Service 如何提供服務發現？
- RBAC Role 與 ClusterRole 差異？
- NetworkPolicy 為何不是所有 CNI 都一定支援？
- PVC 與 StorageClass 的關係？

## Controller / Operator

- reconcile 為什麼要 idempotent？
- controller 為什麼不應依賴事件順序？
- informer cache 可能造成什麼問題？
- finalizer 如何避免外部資源洩漏？
- observedGeneration 解決什麼問題？
- status condition 應如何設計？
- webhook 與 CRD schema validation 有何不同？

## Security

- OIDC 與 OAuth 2.0 差異？
- 如何驗證 JWT？
- API authorization 與 Kubernetes RBAC 為何要雙層？
- tenant isolation 應包含哪些層？
- mTLS 解決的是身份、機密性還是授權？
- ServiceAccount token 應如何使用？

## Multi-Cluster

- Agent-based 與 agentless 模式如何選？
- 叢集斷線時 desired state 怎麼處理？
- 如何避免重複 apply？
- cluster credential 如何保護與輪替？
- placement policy 需要哪些輸入？

## AI Inference Platform

- KServe 解決哪些問題？
- 模型 artifact 與 runtime image 為何應分離？
- GPU request 如何影響 scheduling？
- scale-to-zero 的代價是什麼？
- LLM inference 的 latency 與 throughput 如何取捨？
- Gateway API Inference Extension 的價值在哪裡？

---

# 8. 建議技術筆記題目

每週可從下列題目選一篇撰寫：

1. Gin Context 為何不應傳入 Service？
2. `net/http` middleware 的執行順序。
3. Context cancellation 如何穿過 HTTP、RPC 與 DB。
4. Goroutine leak 的常見原因。
5. Error wrapping 與 API error mapping。
6. Protobuf field number 的相容性規則。
7. ConnectRPC 與 gRPC-Web 的差異。
8. Kubernetes probe 的錯誤使用方式。
9. requests / limits 與 OOMKilled。
10. RBAC least privilege 設計。
11. Informer cache 與 eventual consistency。
12. 為什麼 controller 要 level-triggered。
13. Reconcile idempotency 設計。
14. Finalizer 的正確生命週期。
15. Kubernetes Conditions 設計原則。
16. API layer 與 control plane layer 的責任邊界。
17. OIDC claim 到 tenant authorization 的映射。
18. Gateway API 相較 Ingress 的設計改善。
19. Istio mTLS 與 AuthorizationPolicy 的責任差異。
20. Multi-Cluster agent 的 reconnect 設計。
21. Server-side apply 與 field ownership。
22. KServe InferenceService 的生命週期。
23. GPU scheduling 與資源碎片。
24. 模型 serving 的冷啟動問題。
25. 平台 SLO 應如何定義。

---

# 9. 測試策略總覽

| 層級 | 工具或方式 | 測試重點 |
|---|---|---|
| Unit | Go testing | domain、service、mapper、validation |
| Handler / RPC | `httptest`、Connect client | status、error mapping、metadata |
| Controller Unit | fake / stub | reconcile decision、resource builder |
| Controller Integration | envtest | API server、CRD、status、webhook |
| Kubernetes E2E | kind / k3d | child resources、RBAC、deletion |
| Multi-Cluster E2E | 兩個 kind cluster | agent、heartbeat、remote apply |
| Security | token / RBAC negative test | 越權、過期 token、跨 tenant |
| Failure Injection | script / chaos-lite | timeout、offline、restart、conflict |
| Performance | benchmark / load test | API latency、queue、reconcile throughput |

---

# 10. GitHub Repository 品質標準

完成後 repository 至少應包含：

- 清楚的問題定義。
- 架構圖。
- 功能列表。
- 快速啟動。
- 本機需求。
- API / CRD 範例。
- Demo script。
- 測試方式。
- 安全模型。
- Failure handling。
- 已知限制。
- Roadmap。
- ADR。
- Release tags。
- GitHub Actions。
- issue templates。
- dependency update 設定。

推薦 badges：

- build。
- test。
- lint。
- coverage。
- release。
- container scan。

---

# 11. 暫緩與選修內容

## 11.1 主線完成前暫緩

| 項目 | 暫緩原因 |
|---|---|
| 深入 Ceph OSD / CRUSH map | 偏 storage specialist，先理解 Rook 與使用模式即可 |
| 深入前端框架 | 主線是 Go / platform backend，UI 可最後補 |
| LLM 訓練與 fine-tuning | 本計畫聚焦 serving infrastructure |
| 複雜 service mesh tuning | 先掌握 mTLS、routing、policy、telemetry |
| 過早閱讀大型專案全部原始碼 | 先建立可運作 mental model，再挑模組閱讀 |
| 自行實作完整 scheduler | 先做 placement policy，不需重寫 kube-scheduler |
| 過度追求 benchmark 數字 | 先 profiling，確認真實 bottleneck |

## 11.2 完成主線後的進階選修

### Operator 深化

- controller-runtime source code。
- multi-version CRD conversion。
- admission control。
- operator upgrade strategy。
- leader election 與 sharding。
- controller scale testing。

### Kubernetes Platform

- Argo CD / Flux GitOps。
- Crossplane。
- Cluster API。
- OPA Gatekeeper / Kyverno。
- External Secrets Operator。
- cert-manager。
- SPIFFE / SPIRE。

### Multi-Cluster

- Cluster API。
- Submariner。
- Istio multi-cluster。
- Karmada / Open Cluster Management。
- federation 與 data sovereignty。

### AI Infrastructure

- Kueue。
- Volcano scheduler。
- Ray on Kubernetes。
- NVIDIA GPU Operator。
- Triton Inference Server。
- vLLM distributed serving。
- model cache、prefetch、artifact replication。
- inference autoscaling。
- observability for tokens/sec、TTFT、TPOT。

### Reliability

- chaos engineering。
- load testing。
- rate limiting。
- bulkhead。
- disaster recovery。
- backup / restore。
- SLO / error budget policy。

---

# 12. 時間不足時的壓縮版

若求職時程較急，可採 16 週壓縮版，但仍維持同一學習順序。

| 週次 | 主題 |
|---:|---|
| 1–2 | Go `net/http`、context、concurrency、testing |
| 3 | Protobuf、ConnectRPC |
| 4–5 | Docker、Kubernetes workload、RBAC、network |
| 6 | client-go、informer、workqueue |
| 7–9 | Kubebuilder、Tenant Operator、status、finalizer |
| 10 | Platform API + OIDC |
| 11 | Observability |
| 12 | Gateway API + Istio |
| 13–14 | Multi-Cluster agent |
| 15 | KServe + ModelService |
| 16 | 整合、測試、Demo |

壓縮版的原則是刪除深度，不改變順序；不要跳過 Go 原生、client-go 或 reconciliation，直接從 Gin 跳到 KServe。

---

# 13. 第一個月的具體執行清單

## 第 1 週

- 建立 monorepo。
- 完成 `net/http` Job API。
- 加入 JSON error schema。
- 加入 graceful shutdown。
- 寫 handler test。
- 技術筆記：`http.Handler` 與 Gin Engine。

## 第 2 週

- 建立 worker pool。
- 支援 cancel Job。
- 加入 context timeout。
- 執行 race detector。
- 使用 pprof 查看 goroutine。
- 技術筆記：cancellation propagation。

## 第 3 週

- 加入 domain error。
- 使用 `log/slog`。
- 建立 config loader 與 validation。
- 加入 middleware。
- 技術筆記：transport error mapping。

## 第 4 週

- 完成 unit / integration test。
- 寫 benchmark。
- 寫 fuzz test。
- 找出並改善一個 allocation hotspot。
- 整理第一個 release tag：`v0.1-go-http`。

---

# 14. 最終能力檢核

完成本計畫後，應能獨立完成下列工作：

## Go

- 從零建立 production-grade Go service。
- 設計 context、timeout、retry、shutdown。
- 使用測試、benchmark、pprof、race detector。
- 維持 framework 與 domain 邊界。

## Kubernetes

- 部署、觀察與除錯 workload。
- 設計 RBAC、NetworkPolicy、quota 與 storage。
- 使用 typed / dynamic client 操作 API。
- 實作 informer + workqueue controller。

## Operator

- 設計 CRD。
- 實作 idempotent reconcile。
- 建立 child resources。
- 正確處理 status、condition、finalizer、webhook。
- 撰寫 envtest 與 e2e test。

## Platform

- 用 ConnectRPC 提供平台 API。
- 整合 OIDC 與 tenant authorization。
- 建立 metrics、logs、tracing 與 audit。
- 使用 Gateway API 與 Istio 控制流量與身份。

## Multi-Cluster

- 註冊叢集。
- 管理多份 client。
- 建立 agent heartbeat 與 reconnect。
- 處理離線、重試、drift 與 placement。

## AI Infrastructure

- 部署 KServe model service。
- 管理模型 artifact、runtime 與 endpoint。
- 設計 GPU-aware placement。
- 建立 ModelService Operator 與 inference route。

---

# 15. 最後的學習主線

整份計畫最重要的不是把工具清單全部勾完，而是形成下列工程思維：

```text
使用者提出意圖
→ API 驗證身份與權限
→ CRD 表達 desired state
→ Controller 持續 reconcile
→ Kubernetes 建立實際資源
→ Gateway / Istio / KServe 承載流量與模型
→ Metrics / Status 回報目前狀態
→ 故障後系統重新收斂
```

你最終要從「我會使用 Gin、Docker、Kubernetes」提升成：

> 我能設計並實作一個以 Go 與 Kubernetes 為核心的控制平面，透過宣告式 API、Operator、多租戶安全與多叢集機制，可靠地管理 AI 模型服務的部署與推論生命週期。

---

# 參考文件入口

- Go Documentation: <https://go.dev/doc/>
- `net/http`: <https://pkg.go.dev/net/http>
- Chi: <https://github.com/go-chi/chi>
- ConnectRPC: <https://connectrpc.com/>
- Buf: <https://buf.build/docs/>
- Kubernetes Documentation: <https://kubernetes.io/docs/>
- client-go: <https://github.com/kubernetes/client-go>
- Kubebuilder Book: <https://book.kubebuilder.io/>
- controller-runtime: <https://github.com/kubernetes-sigs/controller-runtime>
- Gateway API: <https://gateway-api.sigs.k8s.io/>
- Istio: <https://istio.io/latest/docs/>
- Rook: <https://rook.io/docs/rook/latest-release/>
- KServe: <https://kserve.github.io/website/>
- Gateway API Inference Extension: <https://gateway-api-inference-extension.sigs.k8s.io/>