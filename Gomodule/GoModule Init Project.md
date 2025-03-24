- go env -w GO111MODULE=on // default is on
- cd to project catalog
- go mod init "module name"
	- 初始化 GoModule 
- go get "module name" // 下載該需要的資源庫

- Don't create project in $GOPATH/src
- Create  go.mod file and make a project name

```go
module "project name"

go "version"
```
- go.mod will add mod when programer get new mod or command go mod tidy
- //indirect 表示間接依賴
- go.sum list direct and indirect mod version, make sure mod version not be change
- h1:hash 表示整體的校驗，如果不存在可能沒有用上該mod