> 本文件是《Go_Kubernetes_Cloud_Native_AI_Platform_32週完整學習計畫》的入口指南。
>
> 建議先完成本文件中的自我檢核，再決定要直接進入 32 週主計畫，或先執行 3～8 週的前導學習。

---

# 目錄

1. [本文件的目的](#1-本文件的目的)
2. [這份 32 週計畫適合誰](#2-這份-32-週計畫適合誰)
3. [開始前的最低必備能力](#3-開始前的最低必備能力)
4. [強烈建議具備的能力](#4-強烈建議具備的能力)
5. [不需要事先具備的能力](#5-不需要事先具備的能力)
6. [AI 與機器學習需要懂多少](#6-ai-與機器學習需要懂多少)
7. [設備與開發環境需求](#7-設備與開發環境需求)
8. [開始前自我檢核](#8-開始前自我檢核)
9. [依檢核結果選擇起點](#9-依檢核結果選擇起點)
10. [8 週前導學習計畫](#10-8-週前導學習計畫)
11. [3～4 週快速補強版](#11-34-週快速補強版)
12. [正式進入 32 週計畫前的驗收專案](#12-正式進入-32-週計畫前的驗收專案)
13. [建議安裝的工具](#13-建議安裝的工具)
14. [常見錯誤與進入門檻判斷](#14-常見錯誤與進入門檻判斷)
15. [最終適用對象說明](#15-最終適用對象說明)

---

# 1. 本文件的目的

《Go_Kubernetes_Cloud_Native_AI_Platform_32週完整學習計畫》不是零基礎程式設計課程，而是一條從一般 Go Backend 進階到下列能力的中進階工程路線：

- Production-grade Go Service
- ConnectRPC、gRPC、Protocol Buffers
- Container 與 Kubernetes
- Kubernetes API、client-go、Informer、Workqueue
- CRD、Controller、Operator、Kubebuilder
- OIDC、RBAC、mTLS、多租戶隔離
- Gateway API、Istio、Rook／Ceph
- Multi-cluster control plane 與 cluster agent
- KServe、GPU scheduling、LLM inference infrastructure

因此，正式進入 32 週主計畫前，學習者應已經具備基本程式設計、Web Backend、資料庫、Git、Linux CLI 與 Docker 能力。

本文件的目標不是把入場門檻設得很高，而是避免學習者在主計畫前半段同時卡在：

```text
Go 語法不熟
HTTP 不熟
SQL 不熟
Docker 不熟
Linux CLI 不熟
Kubernetes 新概念大量湧入
```

如果基礎不足，問題通常不是不夠努力，而是同時承擔太多認知負荷。

---

# 2. 這份 32 週計畫適合誰

## 2.1 最適合的學習者

最適合的起點是：

> 能獨立完成一個具備資料庫、身分驗證、錯誤處理、測試與 Docker 部署能力的 Go REST API，但尚未深入 Kubernetes Controller、Operator、多叢集與 AI inference platform。

具體而言，學習者最好曾經完成過下列其中一種專案：

- 會員與登入系統
- Todo／Task Management API
- 部落格 API
- 訂單或庫存 API
- 小型 SaaS Backend
- IoT Device Management API
- Model Registry API

專案不需要很大，但應至少包含：

- REST API
- JSON request／response
- SQL database
- Authentication
- Middleware
- Configuration
- Error handling
- Git
- Docker

## 2.2 不太適合直接開始的情況

若符合以下多數情況，建議先執行本文件的前導學習：

- 尚未學過一門程式語言
- Go 語法仍需要逐行照抄
- 不理解 HTTP request／response
- 尚未做過資料庫 CRUD
- 不會使用 Git branch 與 commit
- 幾乎沒有命令列操作經驗
- 尚未 build 過 Docker image
- 不知道 process、port、environment variable 的用途

---

# 3. 開始前的最低必備能力

以下能力屬於正式進入 32 週計畫的最低基礎。

---

## 3.1 Go 程式設計基礎

至少應熟悉：

### 語言與資料結構

- 變數與常數
- 基本型別
- 條件判斷
- 迴圈
- Array
- Slice
- Map
- Struct
- Pointer 基礎
- Method
- Interface

### 程式結構

- Function
- Package
- Exported／unexported identifier
- `go.mod`
- 第三方套件管理
- 專案目錄基礎

### 錯誤與資源管理

- Go 的多回傳值
- `error` 型別
- `if err != nil`
- `defer`
- 基本 error wrapping

### 基礎併發

- 知道 goroutine 是什麼
- 知道 channel 的用途
- 知道 concurrency 不等於 parallelism
- 知道 goroutine 需要生命週期管理

### 基礎 Context

- 知道 `context.Context` 用來傳遞 cancellation、deadline 與 request-scoped information
- 能看懂 `context.WithTimeout`
- 不把 Context 當成任意資料容器

### JSON

- Struct tag
- `json.Marshal`
- `json.Unmarshal`
- `json.NewEncoder`
- `json.NewDecoder`

### 最低閱讀能力範例

學習者應可理解下列程式：

```go
package main

import (
    "encoding/json"
    "net/http"
)

type User struct {
    ID    int64  `json:"id"`
    Name  string `json:"name"`
    Email string `json:"email"`
}

func getUser(w http.ResponseWriter, r *http.Request) {
    user := User{
        ID:    1,
        Name:  "Alice",
        Email: "alice@example.com",
    }

    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusOK)

    if err := json.NewEncoder(w).Encode(user); err != nil {
        http.Error(w, "failed to encode response", http.StatusInternalServerError)
    }
}
```

### 開始前不要求精通

以下內容會在主計畫中補強，不是入場條件：

- Go runtime
- scheduler internals
- memory model 深入細節
- reflection
- generics 進階抽象
- lock-free data structure
- GC tuning

---

## 3.2 Web Backend 與 HTTP 基礎

這是最重要的前置能力之一。

至少應理解：

### HTTP 基本模型

- Client／Server
- Request／Response
- URL
- HTTP method
- Header
- Request body
- Response body
- Status code

### 常用 HTTP Method

- `GET`
- `POST`
- `PUT`
- `PATCH`
- `DELETE`

### 常用 Status Code

- `200 OK`
- `201 Created`
- `204 No Content`
- `400 Bad Request`
- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`
- `409 Conflict`
- `422 Unprocessable Entity`
- `500 Internal Server Error`

### API 設計基礎

- REST API 基本概念
- Path parameter
- Query parameter
- JSON body
- Pagination 基礎
- Request validation
- 統一錯誤格式
- API versioning 基礎

### Middleware

應知道 Middleware 常用於：

- Authentication
- Authorization
- Logging
- Request ID
- CORS
- Recovery
- Metrics
- Timeout

### Authentication 與 Authorization

應能區分：

```text
Authentication
確認「你是誰」

Authorization
確認「你可以做什麼」
```

### JWT 基礎

至少知道：

- JWT 常用於傳遞身分資訊
- JWT 包含 header、payload、signature
- JWT payload 並非加密內容
- Server 需要驗證 signature、issuer、audience、expiration
- 不應把密碼與敏感資料放進 token payload

### 建議已完成的 API

```text
POST   /users
GET    /users/{id}
PATCH  /users/{id}
DELETE /users/{id}

POST   /auth/register
POST   /auth/login
GET    /healthz
```

---

## 3.3 SQL 與資料庫基礎

主計畫雖然不是資料庫專精路線，但平台 API、Tenant、Cluster Registry、Audit Log 與操作紀錄都會接觸持久化資料。

至少應理解：

### 資料模型

- Database
- Schema
- Table
- Row
- Column
- Primary key
- Foreign key
- Unique constraint
- Nullability

### SQL 基礎

- `SELECT`
- `INSERT`
- `UPDATE`
- `DELETE`
- `WHERE`
- `ORDER BY`
- `LIMIT`
- `JOIN`
- Aggregate function 基礎

### Database Engineering 基礎

- Index 的用途
- Transaction 的概念
- Connection pool
- Schema migration
- ORM 與 SQL 的關係
- N+1 query 是什麼

### 可使用的技術組合

熟悉其中一種即可：

- PostgreSQL + `database/sql`
- PostgreSQL + pgx
- PostgreSQL + GORM
- MySQL + GORM
- MySQL + `database/sql`

### 開始前不要求精通

- Query planner 深入分析
- MVCC 內部實作
- Replication
- Sharding
- Distributed transaction
- Consensus protocol

---

## 3.4 Git 與版本控制

整份計畫會持續產出：

- Source code
- CRD
- Kubernetes manifest
- Helm chart
- CI workflow
- Architecture Decision Record
- Technical note
- Demo script

因此 Git 是必要能力。

至少應會：

```bash
git clone
git status
git add
git commit
git pull
git push
git branch
git switch
git merge
git log
git diff
```

並理解：

- Repository
- Commit
- Branch
- Merge
- Merge conflict
- Pull request
- `.gitignore`
- Commit message
- Tag 與 release 基礎

### 安全要求

必須知道下列內容不可 commit：

- Password
- API token
- Cloud credential
- kubeconfig
- Private key
- Database connection secret
- `.env` 敏感內容

---

## 3.5 Linux 與命令列基礎

Kubernetes 與平台工程高度依賴 CLI。若命令列不熟，後續除錯成本會非常高。

至少應會：

```bash
pwd
ls
cd
mkdir
cp
mv
rm
cat
less
head
tail
grep
find
curl
ps
kill
chmod
export
env
which
```

並理解：

- Absolute path／relative path
- Working directory
- Environment variable
- Process
- Signal
- Port
- File permission
- Standard input
- Standard output
- Standard error
- Exit code
- Pipe
- Redirect

### 基本操作範例

```bash
curl -X POST http://localhost:8080/jobs \
  -H "Content-Type: application/json" \
  -d '{"name":"demo"}'
```

```bash
tail -f app.log | grep "error"
```

### Windows 學習者

建議使用：

- Windows 11
- WSL2
- Ubuntu
- Docker Desktop 或相容容器環境

雖然可以使用 PowerShell，但大量雲原生文件、腳本與工具以 Bash／Linux 為預設環境。

---

## 3.6 基礎網路知識

不需要先成為 Network Engineer，但必須理解服務如何互相連線。

至少應理解：

- IP address
- Port
- TCP／UDP
- `localhost`
- DNS
- Domain name
- HTTP／HTTPS
- TLS 的用途
- Client 如何連線到 Server
- Reverse proxy
- Public network／private network
- Firewall 基礎
- Connection timeout
- DNS resolution

### 最低理解範例

對下列位址：

```text
http://localhost:8080/api/v1/users
```

應能辨識：

- `http`：protocol
- `localhost`：host
- `8080`：port
- `/api/v1/users`：path

### 為什麼重要

後續會接觸：

- Kubernetes Service
- ClusterIP
- NodePort
- LoadBalancer
- Ingress
- Gateway API
- Service Mesh
- mTLS
- Multi-cluster networking

若前置網路概念不足，容易只記 YAML，不理解流量實際如何移動。

---

## 3.7 Docker 基礎

主計畫會深入 production container，但不會從「Container 是什麼」開始。

至少應理解：

- Image 與 Container 的差異
- Dockerfile
- Build context
- Layer
- Port mapping
- Volume
- Environment variable
- Container log
- Container network 基礎
- Container lifecycle

至少應會：

```bash
docker build -t demo-api .
docker run --rm -p 8080:8080 demo-api
docker ps
docker images
docker logs <container>
docker exec -it <container> sh
docker stop <container>
```

最好也使用過：

```bash
docker compose up -d
docker compose ps
docker compose logs
docker compose down
```

### 開始前不要求精通

- Multi-stage build
- Distroless image
- Rootless container
- Image signing
- SBOM
- Supply-chain security
- Registry architecture

---

# 4. 強烈建議具備的能力

以下不是絕對入場條件，但能顯著降低主計畫前半段負擔。

---

## 4.1 基本分層架構

最好理解：

```text
HTTP Handler
    ↓
Application Service
    ↓
Repository Interface
    ↓
Database Implementation
```

各層責任大致如下：

| 層級 | 主要責任 |
|---|---|
| Handler／Transport | HTTP、RPC、輸入輸出轉換 |
| Application Service | Use case 與流程編排 |
| Domain | 核心模型與商業規則 |
| Repository | 資料存取抽象 |
| Infrastructure | Database、cache、external API |

### 必須建立的觀念

Framework-specific context 不應擴散到整個系統。

建議：

```go
func (s *UserService) Create(
    ctx context.Context,
    input CreateUserInput,
) (*User, error)
```

不建議：

```go
func (s *UserService) Create(c *gin.Context) error
```

原因是 Service 應依賴標準 Go abstraction，而不是依賴特定 HTTP framework。

---

## 4.2 基本測試經驗

最好曾寫過：

- Unit test
- Table-driven test
- HTTP handler test
- Repository integration test

至少應看得懂：

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name string
        a    int
        b    int
        want int
    }{
        {
            name: "positive numbers",
            a:    1,
            b:    2,
            want: 3,
        },
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Add(tt.a, tt.b)
            if got != tt.want {
                t.Fatalf("got %d, want %d", got, tt.want)
            }
        })
    }
}
```

若完全沒有測試經驗，仍可開始，但建議先完成前導第 6 週。

---

## 4.3 YAML 基礎

Kubernetes 大量使用 YAML，至少要看得懂：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-api
spec:
  replicas: 2
```

應理解：

- Key／value
- List
- Nested object
- Indentation
- String
- Number
- Boolean
- Multiline text

不需要事先背 Kubernetes manifest 欄位。

---

## 4.4 基礎 Kubernetes 操作

這不是硬性門檻，主計畫會系統性學習。

但若已接觸過以下操作，會更順利：

```bash
kubectl get pods
kubectl get deployments
kubectl describe pod <name>
kubectl logs <pod>
kubectl apply -f deployment.yaml
kubectl delete -f deployment.yaml
```

並知道以下名詞即可：

- Pod
- Deployment
- Service
- Namespace
- ConfigMap
- Secret

---

## 4.5 英文技術文件閱讀能力

不要求英文流利，但需要能透過翻譯工具閱讀：

- Go 官方文件
- Kubernetes 官方文件
- Kubebuilder Book
- ConnectRPC 文件
- Istio 文件
- KServe 文件
- GitHub issue 與 release note

平台工程變化快，中文教學常落後於官方版本，因此不能只依賴單一中文課程。

---

# 5. 不需要事先具備的能力

以下是 32 週主計畫的核心學習內容，不應被當成入場條件：

## Go 與 Service Engineering

- 深入 `net/http`
- Middleware chaining 設計
- Graceful shutdown
- Context cancellation 深入運用
- Worker pool
- Fan-out／fan-in
- `errgroup`
- Race detector
- Benchmark
- pprof
- Structured logging
- Metrics
- Tracing

## RPC 與 API Contract

- Protocol Buffers
- ConnectRPC
- gRPC
- Streaming RPC
- Interceptor
- API backward compatibility
- RPC status code

## Kubernetes Platform Engineering

- Kubernetes API machinery
- `client-go`
- Typed client
- Dynamic client
- Discovery client
- Informer
- Lister
- Workqueue
- ResourceVersion
- Optimistic concurrency
- Server-side apply

## Operator Development

- CRD
- Custom Resource
- Kubebuilder
- `controller-runtime`
- Reconcile loop
- OwnerReference
- Finalizer
- Status subresource
- Conditions
- Admission webhook
- Conversion webhook

## Security 與 Multi-tenancy

- OIDC integration
- JWT claims mapping
- Kubernetes RBAC 進階設計
- Namespace isolation
- NetworkPolicy
- mTLS
- Certificate rotation

## Networking、Storage 與 Multi-cluster

- Gateway API
- Istio
- Envoy
- Rook／Ceph
- Cluster registration
- Cluster agent
- Reverse tunnel
- Multi-cluster reconciliation

## AI Inference Infrastructure

- KServe
- InferenceService
- vLLM
- GPU resource scheduling
- Model lifecycle
- Scale-to-zero
- Canary rollout
- Inference routing
- Gateway API Inference Extension

若要求學習者事先熟悉以上內容，等於要求先完成整份主計畫才能開始主計畫。

---

# 6. AI 與機器學習需要懂多少

本計畫偏向：

> AI Infrastructure、Model Serving、Inference Platform

而不是：

> 模型研究、模型訓練、演算法推導

## 6.1 開始主計畫前不要求

- 線性代數深入推導
- 微積分
- Backpropagation
- Transformer architecture 深入數學
- PyTorch 訓練流程
- Fine-tuning
- Dataset preprocessing
- Experiment tracking

## 6.2 進入 KServe 階段前應補齊

- Training 與 inference 的差異
- Model artifact
- Online inference
- Batch inference
- Model runtime
- CPU inference
- GPU inference
- Latency
- Throughput
- Token
- Context length
- Model loading
- Autoscaling
- Cold start

## 6.3 建議最低理解

學習者應能說明：

```text
模型訓練
使用資料更新模型參數

模型推論
使用已訓練完成的模型產生預測結果

模型服務
把模型包裝成可透過網路呼叫的長期執行服務
```

---

# 7. 設備與開發環境需求

---

## 7.1 最低配置

- 4 核心 CPU
- 16 GB RAM
- 80～100 GB 可用 SSD 空間
- 支援硬體虛擬化
- 穩定網路
- Windows 11 + WSL2、Linux 或 macOS

此配置可完成大部分前半段內容，但同時啟動下列元件時可能吃緊：

- kind cluster
- Istio
- KServe
- Rook／Ceph
- Observability stack
- 多個 Go service

建議按階段啟動，不要一次常駐全部元件。

## 7.2 推薦配置

- 8 核心以上 CPU
- 32 GB RAM
- 150 GB 以上 SSD 空間
- 支援硬體虛擬化

此配置較適合：

- 多節點 kind
- Istio
- Prometheus／Grafana
- KServe
- 多叢集模擬
- 本機 LLM runtime

## 7.3 GPU

GPU 不是開始計畫的必要條件。

前期可採用：

- CPU runtime
- 小型模型
- Mock model server
- 模擬 GPU resource request
- 雲端短期 GPU instance

只有在後期進行真實 LLM serving、GPU scheduling 與效能測試時，才需要實體 GPU 或雲端 GPU。

---

# 8. 開始前自我檢核

請針對每一項回答「是」或「否」。

---

## 8.1 Go 與 Backend

- [ ] 能自行建立 Go module
- [ ] 能使用 struct、method、interface
- [ ] 能正確處理 `error`
- [ ] 能閱讀 goroutine 與 channel 基礎程式
- [ ] 能建立 JSON REST API
- [ ] 能將 request body 轉為 struct
- [ ] 能回傳合理的 HTTP status code
- [ ] 能連接 MySQL 或 PostgreSQL
- [ ] 能完成基本 CRUD
- [ ] 知道 Middleware 的用途
- [ ] 知道 JWT 的基本運作方式
- [ ] 能區分 Authentication 與 Authorization

## 8.2 工具與環境

- [ ] 能使用 Git commit、branch、merge、push
- [ ] 能處理基本 merge conflict
- [ ] 能在命令列啟動程式與查看 log
- [ ] 能使用 `curl` 測試 API
- [ ] 能使用 environment variable
- [ ] 能閱讀基本 Shell command
- [ ] 能寫基本 Dockerfile
- [ ] 能 build 並執行 Container
- [ ] 能閱讀 Docker log
- [ ] 能閱讀基本 YAML

## 8.3 基礎觀念

- [ ] 知道 process 與 container 的差異
- [ ] 知道 image 與 container 的差異
- [ ] 知道 host、port、path 的意義
- [ ] 知道 HTTP 與 HTTPS 的差異
- [ ] 知道 DNS 的用途
- [ ] 知道 index 與 transaction 的基本用途
- [ ] 知道手動測試不能取代自動測試
- [ ] 能透過翻譯工具閱讀英文技術文件

## 8.4 加分項目

- [ ] 寫過 Unit Test
- [ ] 寫過 Handler Test
- [ ] 使用過 Docker Compose
- [ ] 使用過 GitHub Actions
- [ ] 使用過 `kubectl`
- [ ] 在本機建立過 kind 或 k3d cluster

---

# 9. 依檢核結果選擇起點

以上核心項目共 30 題，不含加分項目。

| 答「是」數量 | 建議起點 |
|---:|---|
| 25～30 | 可直接開始 32 週主計畫 |
| 19～24 | 建議先執行 3～4 週快速補強 |
| 11～18 | 建議完成完整 8 週前導計畫 |
| 0～10 | 建議先完成 Go 與 Web Backend 基礎課程，再執行前導計畫 |

## 9.1 不只看分數

即使總分夠高，若下列任一項完全不會，仍建議先補強：

- 無法獨立寫 Go CRUD API
- 不會使用 Git
- 不會基本 SQL
- 不會使用命令列
- 無法 build Docker image

這五項是硬性基礎。

---

# 10. 8 週前導學習計畫

此階段適合：

- 已有基本程式設計概念
- 但 Go／Backend／Docker 能力尚未形成完整專案經驗

建議每週投入 8～12 小時。

---

## 前導第 1 週：Go 語法與工具鏈

### 學習目標

- 能獨立建立 Go 專案
- 熟悉基本語法與資料結構
- 理解 package 與 module

### 學習內容

- Go 安裝與環境
- `go run`
- `go build`
- `go test`
- `go fmt`
- `go vet`
- package
- module
- variable
- constant
- condition
- loop
- function
- struct
- pointer
- slice
- map

### 實作任務

建立 CLI Todo Application：

```text
add
list
complete
remove
```

資料可以先儲存在 JSON file。

### 驗收標準

- 能拆分至少兩個 package
- 不依賴全域變數管理主要狀態
- 能正確處理檔案錯誤
- 通過 `go fmt` 與 `go vet`

---

## 前導第 2 週：Interface、Error、Concurrency 與 Context

### 學習目標

- 理解 Go 的 composition
- 建立基本 error handling 習慣
- 初步理解 cancellation

### 學習內容

- Interface
- Method set 基礎
- Error wrapping
- `errors.Is`
- `errors.As`
- `defer`
- goroutine
- channel
- WaitGroup
- Mutex 基礎
- `context.Background`
- `context.WithCancel`
- `context.WithTimeout`

### 實作任務

建立可取消的 background job runner：

```text
提交工作
→ Worker 執行
→ 支援 Timeout
→ 支援 Cancel
→ 正確關閉 Goroutine
```

### 驗收標準

- 不產生明顯 goroutine leak
- cancellation 能傳遞至工作函式
- 發生錯誤時能停止相關工作
- 通過 `go test -race ./...`

---

## 前導第 3 週：HTTP 與 REST API

### 學習目標

- 理解 HTTP request／response
- 能獨立建立 JSON REST API

### 學習內容

- `net/http` 基礎
- Router
- Handler
- Path parameter
- Query parameter
- JSON decode／encode
- Status code
- Middleware
- Validation
- Error response
- Health check

### 實作任務

建立記憶體版 Todo REST API：

```text
POST   /todos
GET    /todos
GET    /todos/{id}
PATCH  /todos/{id}
DELETE /todos/{id}
GET    /healthz
```

### 驗收標準

- 錯誤輸入不造成 panic
- 回傳合理 status code
- 統一 JSON error schema
- 支援 request ID
- 能使用 `curl` 完整測試

---

## 前導第 4 週：SQL、Migration 與 Repository

### 學習目標

- 將 API 從記憶體儲存改為資料庫
- 理解 Repository 與 transaction

### 學習內容

- PostgreSQL 或 MySQL
- Schema
- Migration
- CRUD SQL
- Foreign key
- Index
- Transaction
- Connection pool
- Repository interface
- Database implementation

### 實作任務

將 Todo API 改為資料庫版本。

新增：

- User
- Todo ownership
- Due date
- Status

### 驗收標準

- 使用 migration 建立 schema
- Repository 支援 context
- 查無資料能回傳 domain error
- 重要欄位建立適當 index
- 不將 SQL error 直接回傳給 Client

---

## 前導第 5 週：Authentication 與分層架構

### 學習目標

- 建立完整會員登入流程
- 把 HTTP、Use case 與 Database 分離

### 學習內容

- Password hashing
- JWT
- Authentication Middleware
- Authorization
- Handler
- Service
- Repository
- Domain model
- DTO
- Configuration
- Environment variable
- Secret handling

### 實作任務

新增：

```text
POST /auth/register
POST /auth/login
GET  /me
```

每個使用者只能操作自己的 Todo。

### 驗收標準

- 密碼不得以明文儲存
- JWT 驗證 expiration
- Service 不依賴 Gin／Echo Context
- 未授權與無權限使用不同錯誤
- Secret 不 commit 進 Git

---

## 前導第 6 週：Testing、Git 與 CI

### 學習目標

- 建立自動測試與基本 CI
- 形成可維護的 Git workflow

### 學習內容

- Unit test
- Table-driven test
- Fake／Mock 基礎
- Handler test
- Repository integration test
- Test database
- Git branch
- Pull request
- GitHub Actions
- Lint

### 實作任務

替 Todo API 建立：

- Service unit test
- Handler test
- Repository integration test
- GitHub Actions workflow

### 驗收標準

CI 至少執行：

```bash
go test ./...
go test -race ./...
go vet ./...
```

重要 Use case 需有成功與失敗測試。

---

## 前導第 7 週：Docker、Linux 與可部署服務

### 學習目標

- 將 API 轉為可重複部署的 Container
- 能處理基本程序與容器問題

### 學習內容

- Dockerfile
- Image layer
- Container lifecycle
- Port mapping
- Volume
- Network
- Docker Compose
- PID 1
- Signal
- Graceful shutdown 基礎
- Container logging
- Environment configuration

### 實作任務

建立 Docker Compose：

```text
API
+ PostgreSQL
+ Migration
```

### 驗收標準

- 新環境可由單一指令啟動
- API 不以 root 身分執行較佳
- 支援 graceful shutdown
- Secret 不寫死在 image
- Log 輸出至 stdout／stderr

---

## 前導第 8 週：Kubernetes 入門

### 學習目標

- 將既有 API 部署至本機 Kubernetes
- 建立 Kubernetes declarative mindset

### 學習內容

- kind 或 k3d
- Pod
- Deployment
- Service
- Namespace
- ConfigMap
- Secret
- Liveness probe
- Readiness probe
- Resource request／limit
- `kubectl apply`
- `kubectl get`
- `kubectl describe`
- `kubectl logs`

### 實作任務

將 Todo API 部署到 kind：

```text
Namespace
Deployment
Service
ConfigMap
Secret
Readiness Probe
Liveness Probe
```

### 驗收標準

- Pod 能成功啟動
- Service 能被存取
- 設定由 ConfigMap／Secret 注入
- 刪除 Pod 後 Deployment 能自動補回
- 能透過 describe 與 logs 找出失敗原因

---

# 11. 3～4 週快速補強版

此版本適合：

- 已能完成 Go CRUD API
- 但測試、Docker、Linux、Kubernetes 基礎較弱

---

## 快速補強第 1 週：Go Service Quality

- Error wrapping
- Context
- Graceful shutdown
- Middleware
- Table-driven test
- Handler test
- Race detector

產出：具備測試與 graceful shutdown 的 Job API。

## 快速補強第 2 週：Database、Architecture 與 Authentication

- Repository
- Transaction
- JWT
- Config
- Secret handling
- Layered architecture

產出：具備 PostgreSQL 與 JWT 的 Job API。

## 快速補強第 3 週：Docker、Linux 與 CI

- Dockerfile
- Docker Compose
- Signal
- Logging
- GitHub Actions
- Integration test

產出：可由 CI 測試、可透過 Compose 部署的 Service。

## 快速補強第 4 週：Kubernetes Fundamentals

- kind
- Deployment
- Service
- ConfigMap
- Secret
- Probe
- Resource request／limit
- RBAC 入門

產出：部署至 kind 並完成基本故障排查。

---

# 12. 正式進入 32 週計畫前的驗收專案

建議所有學習者先完成一個小型 **Preflight Project**。

## 12.1 專案題目

```text
Job Management API
```

## 12.2 最低功能

```text
POST   /jobs
GET    /jobs
GET    /jobs/{id}
DELETE /jobs/{id}
POST   /auth/register
POST   /auth/login
GET    /healthz
GET    /readyz
```

## 12.3 技術要求

- Go
- `net/http`、chi、Gin 或 Echo 任一
- PostgreSQL 或 MySQL
- Migration
- JWT Authentication
- Layered architecture
- Environment configuration
- Structured logging
- Unit test
- Handler test
- Dockerfile
- Docker Compose
- GitHub Actions
- Kubernetes Deployment／Service

## 12.4 架構邊界

```text
HTTP Handler
    ↓
Application Service
    ↓
Repository Interface
    ↓
Database Repository
```

Service 與 Repository 不可依賴特定 Web Framework Context。

## 12.5 驗收清單

- [ ] 可註冊與登入
- [ ] 可建立與查詢 Job
- [ ] 未登入不可操作受保護 API
- [ ] 使用者不能操作其他人的 Job
- [ ] API 正確處理 invalid input
- [ ] Database schema 可由 migration 建立
- [ ] `go test ./...` 通過
- [ ] `go test -race ./...` 通過
- [ ] Docker Compose 可完整啟動
- [ ] Kubernetes Pod 可正常啟動
- [ ] Readiness／Liveness probe 正常
- [ ] Secret 未提交至 Git
- [ ] README 包含啟動、測試與 API 範例

## 12.6 通過標準

不要求專案完美，但學習者應能：

1. 不依賴逐行教學完成主要功能。
2. 遇到錯誤時能自行閱讀 log。
3. 能透過文件與錯誤訊息找到原因。
4. 能說明每一層的責任。
5. 能說明 request 從 Client 到 Database 的完整路徑。
6. 能說明 Container 與 Kubernetes Deployment 的差異。

通過後，即可開始 32 週主計畫。

---

# 13. 建議安裝的工具

以下工具可在開始前完成安裝。

## 13.1 基礎工具

- Git
- Go
- Make
- curl
- jq
- OpenSSL
- Docker
- Docker Compose

## 13.2 Kubernetes 工具

- kubectl
- kind 或 k3d
- Helm
- k9s，可選

## 13.3 開發工具

- Visual Studio Code 或 GoLand
- Go extension
- YAML extension
- Docker extension
- Kubernetes extension，可選

## 13.4 建議檢查指令

```bash
go version
git --version
docker version
docker compose version
kubectl version --client
kind version
helm version
curl --version
jq --version
```

---

# 14. 常見錯誤與進入門檻判斷

---

## 14.1 錯誤：以為會 Gin 就等於熟悉 Go Backend

會使用：

```go
c.JSON(...)
c.ShouldBindJSON(...)
c.Param(...)
```

不代表已理解：

- `http.Handler`
- Context cancellation
- Header write timing
- Server timeout
- Middleware ordering
- Client disconnect
- Graceful shutdown

因此只做過 Gin CRUD 的人仍可以開始，但前幾週必須認真補 Go 原生 HTTP 能力。

---

## 14.2 錯誤：會下 kubectl 指令就等於理解 Kubernetes

能執行：

```bash
kubectl apply -f app.yaml
```

不代表理解：

- Desired state
- Controller loop
- Reconciliation
- Resource ownership
- Status
- Eventual consistency

主計畫真正要建立的是 Kubernetes-native thinking，而不是 YAML 記憶能力。

---

## 14.3 錯誤：一次補所有基礎

不建議同時學：

```text
Go
+ React
+ Kubernetes
+ Terraform
+ Istio
+ LLM training
+ LeetCode
```

正式主線應維持：

```text
Go Service
→ RPC
→ Container
→ Kubernetes
→ Controller
→ Platform Security
→ Multi-cluster
→ AI Inference
```

---

## 14.4 錯誤：因為尚未全部準備好而一直延後

不需要達到「精通」才開始。

適當的進入標準是：

- 能獨立完成基本 Go Backend
- 能 Dockerize
- 能使用 Git 與 CLI
- 願意閱讀官方文件

主計畫中的進階內容本來就會邊做邊學。

---

## 14.5 錯誤：只看影片，不建立可驗收產出

每個前導階段都應留下：

- Git commit
- README
- Test
- Demo command
- Architecture diagram
- Troubleshooting note

沒有輸出的學習很難判斷是否真正理解。

---

# 15. 最終適用對象說明

可將本計畫的適用對象正式描述為：

> 本計畫適合已具備 Go 基礎語法、REST API、SQL、Git、Linux CLI 與 Docker 基礎，並曾獨立完成至少一個具備資料庫與身分驗證功能之 Go Web Backend 專案的學習者。學習者不需要預先具備 Kubernetes Operator、client-go、CRD、Istio、多叢集或 KServe 經驗，相關能力將於 32 週主計畫中逐步建立。

## 最簡單的判斷方式

> 能自行完成並 Dockerize 一個 Go CRUD API，就大致具備進入 32 週計畫的最低條件。

若仍需要逐行照抄才能完成 CRUD API，建議先執行本文件的 8 週前導計畫。

---

# 下一步

完成自我檢核與必要前導訓練後，接續執行：

```text
Go_Kubernetes_Cloud_Native_AI_Platform_32週完整學習計畫.md
```

建議將兩份文件放在同一個專案目錄：

```text
learning-roadmap/
├── Go_Kubernetes_Cloud_Native_AI_Platform_先備知識與前導學習指南.md
└── Go_Kubernetes_Cloud_Native_AI_Platform_32週完整學習計畫.md
```

推薦執行順序：

```text
先備知識自我檢核
    ↓
必要時完成 3～8 週前導學習
    ↓
完成 Preflight Project
    ↓
進入 32 週完整學習計畫
```