# 選修附錄 A：Gin 快速上手與 `net/http` 原理對照

> **定位**：本附錄是 32 週「Go × Kubernetes × Cloud-Native AI Platform」學習計畫的選修內容。  
> **建議時數**：10–14 小時。  
> **適用對象**：具備 Go 基礎與 HTTP 基本概念，但尚未使用過 Gin，或過去只會照範例撰寫 Gin CRUD API 的學習者。  
> **建議順序**：先備知識與前導指南 → 本附錄 → 32 週正式計畫第 1 週 `net/http`。

---

# 1. 附錄目標

32 週正式計畫假設學習者已具備一般 Go Web Backend 經驗，並會使用 Gin 建立 REST API。正式計畫第 1 週則刻意要求暫時不使用 Gin，改以純 `net/http` 理解 HTTP Server 的底層模型。

本附錄用來補足這個前置能力，但不以「學完 Gin 所有功能」為目標，而是透過並排比較建立以下理解：

```text
Gin 核心用法
    ↓
它替開發者封裝了什麼
    ↓
對應哪個 net/http 概念
    ↓
抽象在哪些情況會洩漏
    ↓
如何維持 framework boundary
```

完成後，學習者應能：

- 使用 Gin 建立 REST API。
- 建立 routing、route group 與 middleware。
- 使用 binding 與 validation。
- 建立統一 JSON response 與 API error。
- 區分 `*gin.Context` 與 `context.Context`。
- 使用自訂 `http.Server`、timeout 與 graceful shutdown。
- 使用 `httptest` 測試 Gin router。
- 將 Gin 限制在 transport layer。
- 將同一個 application service 接到 `net/http` handler。

---

# 2. 學習範圍

本附錄聚焦：

```text
Routing
Binding / Validation
Response / Error
Middleware
Context
Server Lifecycle
Testing
Architecture Boundary
```

第一版不深入：

- GORM 完整教學
- Schema migration
- Swagger／OpenAPI
- Session
- OAuth／OIDC
- Refresh token
- WebSocket
- HTML template
- 檔案上傳
- Gin 效能 benchmark

---

# 3. 環境準備

## 3.1 建立專案

```bash
mkdir gin-quickstart
cd gin-quickstart

go mod init example.com/gin-quickstart
go get github.com/gin-gonic/gin
```

確認環境：

```bash
go version
go env GOMOD
```

## 3.2 建議目錄

```text
gin-quickstart/
├── cmd/
│   └── api/
│       └── main.go
├── internal/
│   ├── application/
│   ├── domain/
│   └── transport/
│       └── http/
│           └── gin/
├── go.mod
└── README.md
```

初期可先集中在 `main.go`，完成核心功能後再拆分。

---

# 4. Gin 與 `net/http` 的關係

## 4.1 第一個 Gin Server

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	router := gin.Default()

	router.GET("/ping", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"message": "pong",
		})
	})

	if err := router.Run(":8080"); err != nil {
		panic(err)
	}
}
```

執行：

```bash
go run ./cmd/api
```

測試：

```bash
curl http://localhost:8080/ping
```

結果：

```json
{
  "message": "pong"
}
```

## 4.2 對應的 `net/http`

```go
mux := http.NewServeMux()

mux.HandleFunc("GET /ping", func(
	w http.ResponseWriter,
	r *http.Request,
) {
	w.Header().Set("Content-Type", "application/json")
	w.WriteHeader(http.StatusOK)

	_ = json.NewEncoder(w).Encode(map[string]string{
		"message": "pong",
	})
})

_ = http.ListenAndServe(":8080", mux)
```

## 4.3 抽象對照

| Gin | `net/http` |
|---|---|
| `gin.Engine` | Router + `http.Handler` |
| `gin.Context` | `http.ResponseWriter` + `*http.Request` + Gin 狀態 |
| `router.GET()` | `mux.HandleFunc("GET /...", ...)` |
| `c.JSON()` | Header + status + JSON encode |
| `router.Run()` | `http.ListenAndServe()` |
| `gin.Default()` | Router + Logger + Recovery |

Gin 不是另一套網路協定。它是在 Go HTTP 生態上提供較高階的 routing、binding、middleware 與 response abstraction。

---

# 5. `gin.Default()` 與 `gin.New()`

## `gin.Default()`

```go
router := gin.Default()
```

自動包含：

- Logger middleware
- Recovery middleware

適合教學、prototype 與快速開發。

## `gin.New()`

```go
router := gin.New()

router.Use(gin.Logger())
router.Use(gin.Recovery())
```

`gin.New()` 不包含預設 middleware，適合需要明確控制 logging、recovery 與 middleware 順序的服務。

---

# 6. Routing

## 6.1 HTTP Methods

```go
router.GET("/users", listUsers)
router.POST("/users", createUser)
router.PATCH("/users/:id", updateUser)
router.DELETE("/users/:id", deleteUser)
```

## 6.2 Path Parameter

### Gin

```go
router.GET("/users/:id", func(c *gin.Context) {
	id := c.Param("id")

	c.JSON(http.StatusOK, gin.H{
		"id": id,
	})
})
```

### `net/http`

```go
mux.HandleFunc("GET /users/{id}", func(
	w http.ResponseWriter,
	r *http.Request,
) {
	id := r.PathValue("id")

	writeJSON(w, http.StatusOK, map[string]string{
		"id": id,
	})
})
```

| Gin | `net/http` |
|---|---|
| `:id` | `{id}` |
| `c.Param("id")` | `r.PathValue("id")` |

## 6.3 Query Parameter

### Gin

```go
router.GET("/users", func(c *gin.Context) {
	page := c.DefaultQuery("page", "1")
	keyword := c.Query("keyword")

	c.JSON(http.StatusOK, gin.H{
		"page":    page,
		"keyword": keyword,
	})
})
```

### `net/http`

```go
page := r.URL.Query().Get("page")
if page == "" {
	page = "1"
}

keyword := r.URL.Query().Get("keyword")
```

## 6.4 Route Group

```go
api := router.Group("/api/v1")

users := api.Group("/users")
{
	users.GET("", listUsers)
	users.GET("/:id", getUser)
	users.POST("", createUser)
	users.PATCH("/:id", updateUser)
	users.DELETE("/:id", deleteUser)
}
```

Route group 常用來管理：

- API version
- 功能領域
- Middleware scope
- Tenant route
- Admin route

例如：

```go
api := router.Group("/api/v1")
api.Use(AuthMiddleware())

admin := api.Group("/admin")
admin.Use(RequireRole("admin"))
```

## 6.5 Routing 練習

完成：

```text
GET    /api/v1/jobs
GET    /api/v1/jobs/:id
POST   /api/v1/jobs
DELETE /api/v1/jobs/:id
GET    /healthz
```

要求：

- `/healthz` 不需要 authentication。
- `/api/v1/jobs` 經過 request ID middleware。
- 刪除成功回傳 `204 No Content`。
- 找不到 Job 回傳 `404 Not Found`。

---

# 7. Binding 與 Validation

## 7.1 Request DTO

```go
type CreateJobRequest struct {
	Name string `json:"name" binding:"required,min=2,max=100"`

	Type string `json:"type" binding:"required,oneof=batch realtime"`
}
```

## 7.2 `ShouldBindJSON`

```go
func createJob(c *gin.Context) {
	var request CreateJobRequest

	if err := c.ShouldBindJSON(&request); err != nil {
		c.JSON(http.StatusBadRequest, gin.H{
			"code":    "INVALID_REQUEST",
			"message": "request validation failed",
		})

		return
	}

	c.JSON(http.StatusCreated, gin.H{
		"name": request.Name,
		"type": request.Type,
	})
}
```

測試：

```bash
curl -X POST http://localhost:8080/api/v1/jobs \
  -H "Content-Type: application/json" \
  -d '{
    "name": "nightly-report",
    "type": "batch"
  }'
```

## 7.3 `Bind*` 與 `ShouldBind*`

常見 API：

```text
BindJSON
ShouldBindJSON
BindQuery
ShouldBindQuery
BindUri
ShouldBindUri
BindHeader
ShouldBindHeader
```

建議優先使用 `ShouldBind*`，因為：

- 錯誤會回傳給 handler。
- Handler 可自行決定 status code。
- 容易建立統一 API error。
- 避免 framework 過早寫入 response。

## 7.4 對應的 `net/http`

```go
decoder := json.NewDecoder(r.Body)
decoder.DisallowUnknownFields()

if err := decoder.Decode(&request); err != nil {
	// invalid JSON
}

if err := validate.Struct(request); err != nil {
	// validation failure
}
```

Gin 的 binding 大致封裝：

```text
讀取 request body
    ↓
判斷 Content-Type
    ↓
選擇 decoder
    ↓
綁定到 struct
    ↓
執行 validator
    ↓
回傳 binding / validation error
```

## 7.5 不要直接回傳 `err.Error()`

不建議：

```go
c.JSON(http.StatusBadRequest, gin.H{
	"error": err.Error(),
})
```

建議建立穩定格式：

```go
type APIErrorResponse struct {
	Code    string `json:"code"`
	Message string `json:"message"`

	Fields map[string]string `json:"fields,omitempty"`
}
```

例如：

```json
{
  "code": "VALIDATION_FAILED",
  "message": "request validation failed",
  "fields": {
    "name": "must contain at least 2 characters",
    "type": "must be batch or realtime"
  }
}
```

---

# 8. Response 與 API Error

## 8.1 使用 Response Struct

Prototype 可以使用 `gin.H`，正式 API 建議使用具名型別：

```go
type JobResponse struct {
	ID     string `json:"id"`
	Name   string `json:"name"`
	Type   string `json:"type"`
	Status string `json:"status"`
}
```

```go
c.JSON(http.StatusOK, JobResponse{
	ID:     "job-001",
	Name:   "nightly-report",
	Type:   "batch",
	Status: "pending",
})
```

具名型別的優勢：

- 編譯期檢查。
- API schema 清楚。
- 容易測試。
- 有利於 OpenAPI。
- 能分離 DTO 與 domain model。

## 8.2 統一 Error Helper

```go
type APIErrorResponse struct {
	Code      string `json:"code"`
	Message   string `json:"message"`
	RequestID string `json:"requestId,omitempty"`
}

func writeError(
	c *gin.Context,
	status int,
	code string,
	message string,
) {
	requestID, _ := c.Get("requestID")

	c.JSON(status, APIErrorResponse{
		Code:      code,
		Message:   message,
		RequestID: fmt.Sprint(requestID),
	})
}
```

## 8.3 `c.JSON()` 的底層意義

```text
c.JSON(status, body)
    ↓
設定 Content-Type
    ↓
設定 HTTP status
    ↓
JSON encode
    ↓
寫入 ResponseWriter
```

因此使用 Gin 後，以下問題仍然存在：

- Header already written
- Client disconnect
- Write timeout
- Middleware ordering
- Streaming lifecycle
- JSON encode failure

---

# 9. Middleware

## 9.1 適合放在 Middleware 的功能

- Request ID
- Logging
- Recovery
- Authentication
- Authorization
- CORS
- Metrics
- Tracing
- Rate limiting
- Audit metadata
- Tenant context

不適合：

- 核心商業邏輯
- 大量 DB 操作
- Domain validation
- 長時間背景工作
- 無限制 goroutine

## 9.2 Request ID Middleware

```go
func RequestIDMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		requestID := c.GetHeader("X-Request-ID")

		if requestID == "" {
			requestID = uuid.NewString()
		}

		c.Set("requestID", requestID)
		c.Header("X-Request-ID", requestID)

		c.Next()
	}
}
```

註冊：

```go
router.Use(RequestIDMiddleware())
```

## 9.3 `c.Next()`

```go
func LoggingMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		start := time.Now()

		c.Next()

		slog.Info(
			"request completed",
			"method", c.Request.Method,
			"path", c.Request.URL.Path,
			"status", c.Writer.Status(),
			"duration", time.Since(start),
		)
	}
}
```

執行順序：

```text
Middleware A：before
    ↓
Middleware B：before
    ↓
Handler
    ↓
Middleware B：after
    ↓
Middleware A：after
```

## 9.4 `c.Abort()`

```go
func AuthMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		token := c.GetHeader("Authorization")

		if token == "" {
			c.AbortWithStatusJSON(
				http.StatusUnauthorized,
				APIErrorResponse{
					Code:    "UNAUTHENTICATED",
					Message: "authentication is required",
				},
			)

			return
		}

		c.Next()
	}
}
```

`c.Abort()` 會阻止後續 Gin handlers 執行，但不會自動結束目前 Go function，因此仍需 `return`。

## 9.5 Scope

### Global

```go
router.Use(RequestIDMiddleware())
```

### Group

```go
api := router.Group("/api/v1")
api.Use(AuthMiddleware())
```

### Route

```go
router.DELETE(
	"/api/v1/jobs/:id",
	RequireRole("admin"),
	deleteJob,
)
```

## 9.6 與 `net/http` 對照

### Gin

```go
func Middleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		// before
		c.Next()
		// after
	}
}
```

### `net/http`

```go
func Middleware(
	next http.Handler,
) http.Handler {
	return http.HandlerFunc(func(
		w http.ResponseWriter,
		r *http.Request,
	) {
		// before
		next.ServeHTTP(w, r)
		// after
	})
}
```

核心關係：

```text
Gin:
HandlerFunc → c.Next() → 後續 HandlerFunc

net/http:
Middleware(next) → next.ServeHTTP()
```

---

# 10. `gin.Context` 與 `context.Context`

## 10.1 `*gin.Context`

主要負責：

- Path/query/header
- Binding
- Response
- Gin middleware state
- Gin handler chain

## 10.2 `context.Context`

主要負責：

- Cancellation
- Deadline
- Timeout
- Request-scoped metadata
- 跨 service／repository／RPC／DB 傳遞 lifecycle

## 10.3 正確邊界

```text
Gin Handler
    ↓
c.Request.Context()
    ↓
Application Service
    ↓
Repository / RPC / DB
```

Handler：

```go
func (h *JobHandler) Create(c *gin.Context) {
	var request CreateJobRequest

	if err := c.ShouldBindJSON(&request); err != nil {
		writeError(
			c,
			http.StatusBadRequest,
			"INVALID_REQUEST",
			"invalid request",
		)
		return
	}

	job, err := h.service.Create(
		c.Request.Context(),
		CreateJobInput{
			Name: request.Name,
			Type: request.Type,
		},
	)

	_ = job
	_ = err
}
```

Service：

```go
func (s *JobService) Create(
	ctx context.Context,
	input CreateJobInput,
) (*Job, error) {
	return s.repository.Create(ctx, input)
}
```

## 10.4 不建議

```go
func (s *JobService) Create(
	c *gin.Context,
	request CreateJobRequest,
) error {
	return nil
}
```

後果：

```text
Application Logic
    ↓
依賴 Gin
    ↓
無法被 ConnectRPC 重用
    ↓
測試困難
    ↓
Framework lock-in 擴散
```

## 10.5 Goroutine 注意事項

不要在 goroutine 中繼續操作原始 `*gin.Context`。

應先抽取必要資料：

```go
requestID, _ := c.Get("requestID")
ctx := c.Request.Context()

go func(ctx context.Context, requestID string) {
	// 使用抽取後的資料
}(ctx, fmt.Sprint(requestID))
```

若工作需要在 request 結束後繼續，應建立獨立 background job lifecycle，而不是直接沿用 request context。

---

# 11. 自訂 `http.Server`

## 11.1 Production-oriented Server

不要只停留在：

```go
router.Run(":8080")
```

改用：

```go
server := &http.Server{
	Addr:              ":8080",
	Handler:           router,
	ReadHeaderTimeout: 5 * time.Second,
	ReadTimeout:       10 * time.Second,
	WriteTimeout:      15 * time.Second,
	IdleTimeout:       60 * time.Second,
}
```

啟動：

```go
go func() {
	if err := server.ListenAndServe(); err != nil &&
		!errors.Is(err, http.ErrServerClosed) {
		slog.Error("server failed", "error", err)
		os.Exit(1)
	}
}()
```

## 11.2 Graceful Shutdown

```go
signalContext, stop := signal.NotifyContext(
	context.Background(),
	os.Interrupt,
	syscall.SIGTERM,
)
defer stop()

<-signalContext.Done()

shutdownContext, cancel := context.WithTimeout(
	context.Background(),
	10*time.Second,
)
defer cancel()

if err := server.Shutdown(shutdownContext); err != nil {
	slog.Error("graceful shutdown failed", "error", err)
}
```

流程：

```text
收到 SIGTERM
    ↓
停止接受新 connection
    ↓
等待既有 request 完成
    ↓
超過 deadline 則結束
    ↓
關閉 process
```

Gin Engine 可以放進 `http.Server.Handler`，因為它可作為標準 HTTP handler 使用。

---

# 12. Testing Gin Handler

## 12.1 基本測試

```go
func TestPing(t *testing.T) {
	gin.SetMode(gin.TestMode)

	router := gin.New()

	router.GET("/ping", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"message": "pong",
		})
	})

	request := httptest.NewRequest(
		http.MethodGet,
		"/ping",
		nil,
	)

	recorder := httptest.NewRecorder()

	router.ServeHTTP(recorder, request)

	if recorder.Code != http.StatusOK {
		t.Fatalf(
			"got status %d, want %d",
			recorder.Code,
			http.StatusOK,
		)
	}
}
```

## 12.2 JSON Body

```go
type PingResponse struct {
	Message string `json:"message"`
}

var response PingResponse

if err := json.Unmarshal(
	recorder.Body.Bytes(),
	&response,
); err != nil {
	t.Fatalf("decode response: %v", err)
}

if response.Message != "pong" {
	t.Fatalf(
		"got message %q, want %q",
		response.Message,
		"pong",
	)
}
```

## 12.3 Binding Failure

```go
func TestCreateJob_InvalidBody(t *testing.T) {
	gin.SetMode(gin.TestMode)

	router := setupRouter()

	body := strings.NewReader(`{
		"name": "",
		"type": "unknown"
	}`)

	request := httptest.NewRequest(
		http.MethodPost,
		"/api/v1/jobs",
		body,
	)
	request.Header.Set("Content-Type", "application/json")

	recorder := httptest.NewRecorder()
	router.ServeHTTP(recorder, request)

	if recorder.Code != http.StatusBadRequest {
		t.Fatalf(
			"got status %d, want %d",
			recorder.Code,
			http.StatusBadRequest,
		)
	}
}
```

至少測試：

- Success
- Invalid JSON
- Validation failure
- Missing authentication
- Not found
- Conflict
- Middleware header
- Internal service error
- Panic recovery
- Timeout

Gin 測試仍然建立在：

```text
net/http/httptest
    +
gin.Engine.ServeHTTP
```

---

# 13. Architecture Boundary

## 13.1 建議架構

```text
cmd/
└── api/
    └── main.go

internal/
├── application/
│   ├── job_service.go
│   └── job_input.go
├── domain/
│   ├── job.go
│   └── error.go
├── repository/
│   └── job_repository.go
└── transport/
    └── http/
        └── gin/
            ├── router.go
            ├── job_handler.go
            ├── middleware.go
            ├── request.go
            └── response.go
```

## 13.2 原則

- Gin 只存在 `transport/http/gin`。
- Service 接受 `context.Context`。
- Domain 不知道 HTTP status。
- Repository 不知道 Gin。
- Request DTO 不等於 domain entity。
- DB model 不直接作為 API response。
- Handler 負責 transport error mapping。
- Service 負責 use case。
- Repository 負責持久化。

## 13.3 Error Mapping

Domain：

```go
var ErrJobNotFound = errors.New("job not found")
```

Service：

```go
job, err := s.repository.FindByID(ctx, id)

if err != nil {
	return nil, fmt.Errorf("find job: %w", err)
}
```

Handler：

```go
switch {
case errors.Is(err, ErrJobNotFound):
	writeError(
		c,
		http.StatusNotFound,
		"JOB_NOT_FOUND",
		"job does not exist",
	)

default:
	writeError(
		c,
		http.StatusInternalServerError,
		"INTERNAL_ERROR",
		"internal server error",
	)
}
```

---

# 14. 實作專案：Gin Job API

## 14.1 API

```text
POST   /api/v1/jobs
GET    /api/v1/jobs
GET    /api/v1/jobs/:id
DELETE /api/v1/jobs/:id
GET    /healthz
GET    /readyz
```

## 14.2 Model

```go
type Job struct {
	ID        string
	Name      string
	Type      string
	Status    string
	CreatedAt time.Time
}
```

## 14.3 Request

```go
type CreateJobRequest struct {
	Name string `json:"name" binding:"required,min=2,max=100"`

	Type string `json:"type" binding:"required,oneof=batch realtime"`
}
```

## 14.4 Response

```go
type JobResponse struct {
	ID        string    `json:"id"`
	Name      string    `json:"name"`
	Type      string    `json:"type"`
	Status    string    `json:"status"`
	CreatedAt time.Time `json:"createdAt"`
}
```

## 14.5 必做項目

### Routing

- API version group
- Job resource group
- Health endpoints
- 正確 status code

### Binding

- JSON binding
- Required validation
- Enum validation
- 統一 validation error

### Middleware

- Request ID
- Structured logging
- Recovery
- Authentication mock
- Timeout 基礎

### Architecture

- Handler
- Service
- Repository interface
- In-memory repository
- Domain error
- Error mapping

### Server

- 自訂 `http.Server`
- Read/write/idle timeout
- Signal handling
- Graceful shutdown

### Testing

- Handler success
- Binding failure
- Not found
- Middleware
- Service error

---

# 15. 與正式第 1 週銜接

完成 Gin 版本後，進入正式計畫第 1 週時，將 HTTP transport 改成純 `net/http`。

以下保持不變：

```text
Application Service
Domain
Repository Interface
In-memory Repository
Use Case Test
```

只替換：

```text
Gin Router / Handler / Middleware
                ↓
net/http ServeMux / Handler / Middleware
```

## 對照表

| 功能 | Gin | `net/http` |
|---|---|---|
| Router | `gin.Engine` | `http.ServeMux` |
| Path param | `c.Param()` | `r.PathValue()` |
| Query param | `c.Query()` | `r.URL.Query().Get()` |
| JSON body | `ShouldBindJSON()` | `json.Decoder` |
| Validation | Binding integration | 自行呼叫 validator |
| JSON response | `c.JSON()` | Header + status + encoder |
| Middleware | `gin.HandlerFunc` | `func(http.Handler) http.Handler` |
| Context | `c.Request.Context()` | `r.Context()` |
| Server | `router.Run()`／`http.Server` | `http.Server` |
| Test | `router.ServeHTTP()` | `handler.ServeHTTP()` |

---

# 16. 驗收問題

## Routing

1. `c.Param("id")` 對應 `net/http` 的哪個操作？
2. `c.Query("page")` 從 request 哪個位置讀值？
3. Route group 的主要用途是什麼？
4. Authentication middleware 應放在哪一層？

## Binding

5. `ShouldBindJSON()` 完成哪些工作？
6. 為什麼通常優先使用 `ShouldBind*`？
7. 為什麼不應直接回傳 `err.Error()`？
8. Request DTO 為什麼不應直接等於 DB model？

## Response

9. `c.JSON()` 底層做了哪些事情？
10. 什麼是 Header already written？
11. 為什麼正式 API 不應全部使用 `gin.H`？

## Middleware

12. `c.Next()` 與 `next.ServeHTTP()` 有何關係？
13. `c.Abort()` 是否會結束目前 Go function？
14. Middleware 為什麼呈現洋蔥式順序？
15. 哪些邏輯不適合放在 middleware？

## Context

16. `*gin.Context` 與 `context.Context` 有何差異？
17. 為什麼 service 不應接受 `*gin.Context`？
18. 如何將 request cancellation 傳入 repository？
19. 為什麼不應在 goroutine 中繼續使用原始 Gin Context？

## Server

20. 為什麼 `gin.Engine` 可以放進 `http.Server.Handler`？
21. `router.Run()` 與自訂 `http.Server` 有何差異？
22. Read、write、idle timeout 分別解決什麼問題？
23. Graceful shutdown 的正確順序是什麼？

## Architecture

24. 換掉 Gin 時，哪些程式碼不應修改？
25. HTTP status 為什麼不應存在 domain layer？
26. Tenant ID 應如何從 middleware 傳入 application service？

---

# 17. 完成標準

- [ ] 能建立 Gin server。
- [ ] 能完成 REST routes。
- [ ] 能處理 path 與 query parameters。
- [ ] 能使用 `ShouldBindJSON`。
- [ ] 能建立 stable API error schema。
- [ ] 能實作 request ID middleware。
- [ ] 能說明 `c.Next()` 與 middleware ordering。
- [ ] 能使用 `c.Request.Context()`。
- [ ] 沒有將 `*gin.Context` 傳入 service。
- [ ] 能使用自訂 `http.Server`。
- [ ] 能實作 graceful shutdown。
- [ ] 能以 `httptest` 測試 Gin router。
- [ ] Gin 僅存在 transport layer。
- [ ] 能將相同 service 接到 `net/http` handler。
- [ ] 能回答至少 80% 驗收問題。

---

# 18. 建議五日安排

## 第 1 天：Routing

- 建立專案。
- 第一個 Gin server。
- HTTP methods。
- Path/query parameter。
- Route group。
- Job routes。

產出：

- 可啟動 Server。
- 完整 Job routes。
- Routing 對照筆記。

## 第 2 天：Binding 與 Response

- Request DTO。
- `ShouldBindJSON`。
- Validation tag。
- API error schema。
- Response struct。
- Error mapping。

產出：

- `POST /jobs`。
- Validation tests。
- API error 文件。

## 第 3 天：Middleware 與 Context

- Request ID。
- Logging。
- Authentication mock。
- `c.Next()`。
- `c.Abort()`。
- Gin context vs Go context。
- Service boundary。

產出：

- Middleware chain。
- Context boundary 圖。
- Service tests。

## 第 4 天：Server 與 Testing

- 自訂 `http.Server`。
- Timeout。
- Signal handling。
- Graceful shutdown。
- `httptest`。
- Handler tests。

產出：

- Server bootstrap。
- Handler tests。
- README demo 指令。

## 第 5 天：架構整理

- 拆分 transport/application/domain。
- In-memory repository。
- Domain error。
- ADR。
- 規劃 `net/http` 改寫。

產出：

- 完整 Gin Job API。
- 架構圖。
- Gin vs `net/http` 對照表。
- 正式第 1 週改寫清單。

---

# 19. 常見錯誤

## 把 Gin Context 傳入 Service

錯誤：

```go
service.Create(c, request)
```

正確：

```go
service.Create(
	c.Request.Context(),
	input,
)
```

## 全部使用 `gin.H`

Prototype 可以，正式 API 應逐步改用具名 response struct。

## 直接回傳 `err.Error()`

可能暴露：

- Validation internals
- SQL error
- File path
- Internal service name
- Implementation detail

## 呼叫 `c.Abort()` 後忘記 `return`

```go
if err != nil {
	c.AbortWithStatusJSON(...)
	return
}
```

## 只會 `router.Run()`

還需理解：

- `http.Server`
- Timeout
- TLS
- Shutdown
- Signal
- Request lifecycle

## 把商業邏輯放進 Middleware

Middleware 處理橫切關注點，不負責完整 use case。

## 把 Request DTO 當 Domain Entity

Transport、domain 與 persistence model 應依責任分離。

## 只測 Service，不測 Handler

Handler 仍需測試 routing、binding、status、schema、middleware 與 error mapping。

---

# 20. 與 32 週主線的關係

```text
Gin 快速上手
    ↓
理解 framework productivity
    ↓
正式第 1 週改寫 net/http
    ↓
理解 HTTP 原生模型
    ↓
第 5 週比較 chi / Gin / net/http
    ↓
建立 framework boundary
    ↓
第 6–7 週進入 ConnectRPC / gRPC
```

最終應理解：

> Gin、chi、`net/http` 與 ConnectRPC 都是不同 transport abstraction。Application service、domain model 與 repository contract 應維持相對穩定，使 transport framework 可以替換或並存。

---

# 21. 最終成果

完成本附錄後應交付：

1. 完整 Gin Job API。
2. Handler 與 middleware tests。
3. Gin 與 `net/http` 對照文件。
4. Request lifecycle 架構圖。
5. Gin framework boundary ADR。
6. 可改寫成 `net/http` 的乾淨專案。
7. 踩坑紀錄。

履歷描述範例：

> 使用 Gin 建立具備 request binding、validation、middleware、統一錯誤處理、graceful shutdown 與自動化測試的 REST API，並將 framework 限制於 transport layer，使 application service 可被 `net/http` 或 RPC adapter 重用。

---

# 22. 延伸選修

完成 32 週主線後，可繼續學習：

- Gin + OpenTelemetry
- Gin + Prometheus
- OIDC middleware
- Rate limiting
- OpenAPI
- Multipart upload
- Streaming response
- WebSocket
- SSE
- Load testing
- Security headers
- Request body limit
- Reverse proxy integration
- Framework migration strategy
- Gin 與 chi benchmark
- Gin 與 ConnectRPC 共存架構

---

# 23. 參考入口

- Gin Documentation: <https://gin-gonic.com/docs/>
- Gin Quickstart: <https://gin-gonic.com/en/docs/quickstart/>
- Gin Examples: <https://gin-gonic.com/en/docs/examples/>
- Binding and Validation: <https://gin-gonic.com/en/docs/examples/binding-and-validation/>
- Middleware: <https://gin-gonic.com/en/docs/examples/using-middleware/>
- Go `net/http`: <https://pkg.go.dev/net/http>
- Go `httptest`: <https://pkg.go.dev/net/http/httptest>
- Go `context`: <https://pkg.go.dev/context>

---

## 結語

Gin 的價值是降低 API 開發的決策成本與樣板程式碼，但 framework abstraction 不會消除 HTTP lifecycle、context cancellation、timeout、error handling 與 architecture boundary 等問題。

本附錄真正要建立的是：

```text
會使用 Gin
    ↓
理解 Gin 封裝了什麼
    ↓
看得見 net/http 原理
    ↓
維持 framework boundary
    ↓
能遷移、替換與整合其他 transport
```

完成本附錄後，即可進入 32 週正式計畫，並在第 1 週將 Gin Job API 改寫成純 `net/http`。
