---
title: "Golang Echo Server/Client"
categories:
  - Blog
toc: true
tags:
  - golang
  - network
---

# Project 개요

# 보완할 점

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
		fmt.Println("Error occurred : s%", err.Error())
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

	fmt.Println("Input Msg : ")
	data, err := bufio.NewReader(os.Stdin).ReadString('\n')
	if err != nil {
		fmt.Println("Error occurred : s%", err.Error())
	}

	conn, _ := net.Dial("tcp", addr)
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