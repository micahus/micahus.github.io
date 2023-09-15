---
title: "Go RPC相关及基础信息"
date: 2023-09-15T14:26:42+08:00
tags: ["Golang"]
categories: ["Learning", "StudyNotes"]

---

## Go 的RPC 相关的基础信息

## 1. 环境配置（MacOS）

### 1.1 protoc 安装

```bash
# 1. brew 安装
brew search protobuf
brew install protobuf # 可以指定版本protobuf@21

# 验证是否安装成功
protoc --version

# 2. 源码安装
https://github.com/protocolbuffers/protobuf/releases #下载对应的版本，放入GOPATH中的bin目录下
```

### 1.2 protoc-gen-go 安装

```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
# 安装过程中会出现无法下载，需要自行“走强”

# 验证
protoc-gen-go help
# protoc-gen-go: unknown argument "help" (this program should be run by protoc, not directly)
# ps：我觉得只要识别到，就应该安装成功了
```

### 1.3 protoc-gen-go-grpc 安装

```bash
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
# 同 protoc-gen-go

# 验证
protoc-gen-go-grpc -version
# protoc-gen-go-grpc 1.3.0
```

## 2. Proto转成桩代码

### 2.1 proto 生成

> 参考地址：https://protobuf.dev/

```protobuf
syntax = "proto3";
import public "google/protobuf/timestamp.proto";

option go_package = "./news";
option java_multiple_files = true;
option java_package = "wiki.micah.news";
option java_outer_classname = "News";

package news;

message News {
  int64 id = 1;
  
  string title = 2;

  string keyword = 3;

  string desc = 4;

  google.protobuf.Timestamp happen_time = 5;

  int32 state = 6;

  google.protobuf.Timestamp create_time = 7;

  google.protobuf.Timestamp update_time = 8;
}

message NewsRequest {
  string name = 1;
}

message NewsResponse {
  repeated News  news_list = 1;
}

service SearchService {
  rpc Search (NewsRequest) returns (NewsResponse);
}
```

### 2.2 git 仓库打分支

```bash
# 打tag
git tag -a v0.1.0-alpha -m "预发布"
# 推送tag到目标上
git push origin v0.1.0-alpha
```

## 3. 程序编写

### 3.1 go.mod编写

```go
require (
	github.com/supermicah/Protobufs v0.1.2-alpha
	google.golang.org/grpc v1.58.1
)
```

### 3.2 client 编写

根据grpc的helloworld进行编写

```go
package main

import (
	"context"
	"flag"
	"log"
	"time"

	pb "github.com/supermicah/Protobufs/news/micah/wiki/news"
	"google.golang.org/grpc"
	"google.golang.org/grpc/credentials/insecure"
)

const (
	defaultName = "world"
)

var (
	addr = flag.String("addr", "localhost:50051", "the address to connect to")
)

func main() {
	flag.Parse()
	// Set up a connection to the server.
	conn, err := grpc.Dial(*addr, grpc.WithTransportCredentials(insecure.NewCredentials()))
	if err != nil {
		log.Fatalf("did not connect: %v", err)
	}
	defer func() { _ = conn.Close() }()
	c := pb.NewSearchServiceClient(conn)

	// Contact the server and print out its response.
	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()
	r, err := c.Search(ctx, &pb.NewsRequest{Name: "你好"})
	if err != nil {
		log.Fatalf("could not greet: %v", err)
	}
	log.Printf("NewsList: %s", r)
}

```

### 3.3 server编写

```go
package main

import (
	"context"
	"flag"
	"fmt"
	"google.golang.org/protobuf/types/known/timestamppb"
	"log"
	"net"

	pb "github.com/supermicah/Protobufs/news/micah/wiki/news"
	"google.golang.org/grpc"
)

var (
	port = flag.Int("port", 50051, "The server port")
)

// server is used to implement helloworld.GreeterServer.
type server struct {
	pb.UnimplementedSearchServiceServer
}

// Search implements helloworld.GreeterServer
func (s *server) Search(ctx context.Context, in *pb.NewsRequest) (*pb.NewsResponse, error) {
	log.Printf("Received: %v", in.GetName())
	newsList := make([]*pb.News, 0)
	newsList = append(newsList, &pb.News{
		Id:         1,
		Title:      "test",
		Keyword:    "test",
		Desc:       "test",
		HappenTime: &timestamppb.Timestamp{Seconds: 1, Nanos: 1},
		State:      0,
		CreateTime: &timestamppb.Timestamp{Seconds: 2, Nanos: 2},
		UpdateTime: &timestamppb.Timestamp{Seconds: 3, Nanos: 3},
	})
	return &pb.NewsResponse{NewsList: newsList}, nil
}

func main() {
	flag.Parse()
	lis, err := net.Listen("tcp", fmt.Sprintf(":%d", *port))
	if err != nil {
		log.Fatalf("failed to listen: %v", err)
	}
	s := grpc.NewServer()
	pb.RegisterSearchServiceServer(s, &server{})
	log.Printf("server listening at %v", lis.Addr())
	if err := s.Serve(lis); err != nil {
		log.Fatalf("failed to serve: %v", err)
	}
}

```

