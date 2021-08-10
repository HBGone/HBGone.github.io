---
title: "Golang Echo Server/Client #1"
categories:
  - Golang
toc: true
tags:
  - Golang
  - network
last_modified_at: 2021-06-17T20:00:00-05:00
---

# 개요
golang에서 제공하는 network 함수들을 이용해서 기본적인 Echo Servre/Client를 작성하고 관련된 개념 및 함수 동작 방식을 기술 합니다.

---
# Network 함수들 동작 방식

## Server Side
- Server는 Client의 요청을 받아들여야 합니니다. 때문에 Server는 Client가 접속할 수 있는 정보를 제공 해야 하며 해당되는 정보를 이용해 구현되어야 합니다. 때문에 다음과 같은 작업이 수행되어야 합니다.
	1. __Binding__ : IP / Port 할당
	2. __Listening__ : 클라이언트 요청 대기
	3. __Accept__ : 클라이언트 요청 확인 및 연결 소켓 생성

## Client Side
- Client는 Server로 접속해야 한다. 때문에 Client는 Server에 대한 정보를 알아야 하며 해당 정보를 이용해 접속해야 합니다. 이때 기본적으로 필요한 정보는 IP / Port입니다. Server가 Listening 상태일 때 Client는 다음과 같은 작업을 수행하여 Server로 접속 가능합니다.
	1. __Connect__ : IP / Port 를 이용하여 Server에 접속

위의 과정을 도식화 하면 다음과 같습니다. 왼쪽은 리눅스 기준 오른쪽은 golang 기준입니다.

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/golang/socket_flowchart.png)

# 코드

## Echo Server

```golang
package main

import (
	"bufio"
	"fmt"
	"net"
)

func main() {
	fmt.Println("Start Socket Practice Server")

	addr := "0.0.0.0:18080"
	tcpAddr, err := net.ResolveTCPAddr("tcp4", addr)
	if err != nil {
		fmt.Println("Error occurred : ", err.Error())
	}

	listener, err := net.ListenTCP("tcp", tcpAddr)
	if err != nil {
		fmt.Println("Error occurred : s%", err.Error())
	}

	conn, err := listener.AcceptTCP()
	if err != nil {
		fmt.Println("Error occurred : s%", err.Error())
	}
	fmt.Printf("Client %s Connected\n", conn.RemoteAddr().String())

	handleEchoClient(conn)
}

func handleEchoClient(conn net.Conn)  {
	buf, err := bufio.NewReader(conn).ReadBytes('\n')

	if (err != nil) {
		fmt.Println("Error occrured : s%", err.Error())
		conn.Close()
		return
	}

	fmt.Println("Receive Msg : ", string(buf[:]))
	conn.Write(buf)
	conn.Close()
}
```

## Echo Client

```golang
package main

import (
	"bufio"
	"fmt"
	"net"
	"os"
)

func main()  {
	fmt.Println("Start Socket Practice Client")

	addr := "localhost:18080"

	conn, err := net.Dial("tcp", addr)
	if err != nil {
		fmt.Println("Error occurred : s%", err.Error())
	}

	fmt.Println("Input Msg : ")
	data, err := bufio.NewReader(os.Stdin).ReadString('\n')
	if err != nil {
		fmt.Println("Error occurred : s%", err.Error())
	}

	// write the data to the connection
	fmt.Fprintf(conn, data)

	recv, err := bufio.NewReader(conn).ReadString('\n')
	if err != nil {
		fmt.Println("Error occurred : s%", err.Error())
	}

	fmt.Println("Receive Echo : {{recv}}", recv)

	conn.Close()
}
```

## 보완할 점
1. Connection 유지
		- 현재 한번 Echo 작업 수행후 Connection이 끊어짐
2. Single Connection
		- 현재 한번에 하나의 커넥션만 가능하기 때문에 한번에 여러 커넥션이 가능하도록 변경
