# go-zero-box example source packet

Captured: 2026-09-24

- Project: https://github.com/prf16/go-zero-box
- Source revision: `0076124d7dab6b63a0a5677775b5ea16375d4710` (local checkout).
- Official example index: https://go-zero.dev/zh-cn/examples/
- Reference layout: `src/content/docs/zh-cn/examples/bookstore.md` and `microservice-system.md`.
- Public pages: `examples/go-zero-box.md` in English, Simplified Chinese, and Korean, plus their example indexes.

## Source snapshots

The following excerpts are captured verbatim from the project checkout. Configuration credentials are not included.

### `go.mod`

```text
module go-zero-box

go 1.23.5

require (
	github.com/Masterminds/squirrel v1.5.4
	github.com/go-playground/validator/v10 v10.27.0
	github.com/golang-jwt/jwt/v4 v4.5.2
	github.com/google/uuid v1.6.0
	github.com/google/wire v0.6.0
	github.com/hibiken/asynq v0.25.1
	github.com/prf16/go-zero-box-rpc v1.9.4-0.1.2
	github.com/spf13/cast v1.7.1
	github.com/spf13/cobra v1.10.2
	github.com/zeromicro/go-zero v1.9.4
	golang.org/x/crypto v0.34.0
)

require (
	filippo.io/edwards25519 v1.1.0 // indirect
```

### `app/app.go`

```go
		Use:                "app",
		Use:   "server:api",
		Use:   "server:queue",
		Use:   "server:scheduler",
		Use:   "server:all",
```

### `pkg/rpc/client.go`

```go
package rpc

import (
	"github.com/prf16/go-zero-box-rpc/api/user"
)

type User struct {
	user.UserClient
}

func NewUser(config *Config) *User {
	//var i int
	//for {
	//	client, err := zrpc.NewClient(zrpc.RpcClientConf{
	//		Target: config.Target,
	//	})
	//	if err == nil {
	//		return &User{user.NewUserClient(client.Conn())}
	//	}
	//
	//	log.Printf("rpc.NewUser zrpc.NewClient failed retry:%d, err: %v", i, err)
	//	time.Sleep(2 * time.Second) // 两秒后重试
	//	i++
	//	if i > 3 {
	//		panic(err)
	//	}
	//}

	return &User{}
}
```

### `api/hello/hello.api`

```text
import "../base/base.api"

type (
    HelloReq {
    }
    HelloResp {
        Response
        Data HelloData `json:"data"`
    }
    HelloData {
    }
)

type (
    HelloUserReq {
    }
    HelloUserResp {
        Response
        Data HelloUserRespData `json:"data"`
    }
    HelloUserRespData {
        Id uint64 `json:"id"`
        Nickname string `json:"nickname"`
    }
)

@server(
    middleware: LogMiddleware
    group: hello
    prefix: /api
    tags: "Hello world"
)
service app {
    @doc "Hello world"
    @handler Hello
    get /hello (HelloReq) returns (HelloResp)

    @doc "Hello user（rpc）"
    @handler HelloUser
    get /hello/user (HelloUserReq) returns (HelloUserResp)
}
```

## Documentation decisions

- Identify this as a community project template, with a concise description beside the index link.
- Keep API, queues, scheduler, and CLI distinct from the companion RPC service.
- The current RPC client returns an empty wrapper; document the missing initialization rather than claiming RPC works out of the box.
- Use the actual Go minimum from go.mod and commands from app/app.go.
- Verify documentation structure, links, and site build; runtime examples require local MySQL and Redis and were source-checked only.
