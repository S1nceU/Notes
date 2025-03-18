## go mod commands
- go mod init
  - initialize a new module, creating a new go.mod file
- go mod download
  - download modules to local cache by reading go.mod
- go mod tidy
  - add missing and remove unused modules
- go mod graph
  - print the module requirement graph
- go mod edit
  - edit go.mod from tools or scripts
- go mod vendor 
  - export dependencies to vendor file
- go mod verify
  - verify dependencies have expected content
- go mod why
  - explain why packages or modules are needed

## go mod env variables
```bash
$ go env
GO111MODULE=on
GOPROXY=https://proxy.golang.org,direct
GOSUMDB=sum.golang.org
GONOSUMDB=
GOPRIVATE=
```
### GO111MODULE
- GO111MODULE 是一個開關，使 go mod 啟用或是關閉
- 三種情況：
	- on: go mod is enabled
	- off: go mod is disabled
	- auto: go mod is enabled if current directory is inside GOPATH/src, otherwise disabled
```bash
go env -w GO111MODULE=on
```
### GOPROXY
- 下載 mod 的代理平台
```bash
go env -w GOPROXY="URI"
```
### GOSUMDB
- 確保下載的 mod 是一個完整的資源，所以他會透過 GOSUMDB 檢查 SUM
### GONOSUMDB, GOPRIVATE, GONOPROXY
- 如果今天是私有或是公司用的資源包，則可設定不經過 Go 官網的檢查
```bash
go env -w GORPIVATE="git.example1.com, git.example2.com"
go env -w GORPIVATE="*.example1.com"
```
