# Go Tutorial Part 3

### 개요 

 지난 시간에 이어서 이번에는 Go 언어에서 조건문과 반복문에 대해서 알아보도록 하겠습니다. 



### 조건문 / 분기문

 조건문은 모든 프로그램밍 언어에서 가장 기초적이며 가장 많이 사용되는 문법입니다. Go 언어또한 조건문을 제공하며, 다음 예제를 통해 사용되는 방법에 대해 알아보도록 하겠습니다. 



#### if 사용하기

```go
package main

import (
	f "fmt" // 별칭 설정
)

func main() {

	i := 10
	if i > 5 {
		f.Println("5보다 크다")
	}

	var num int
	f.Println("정수 하나를 입력하세요.")
	f.Scan(&num)

	if num%2 == 0 {
		f.Println(num, "은 짝수")
	} else {
		f.Println(num, "은 홀수")
	}

	var word string
	f.Println("input hello or go or programs.")
	f.Scan(&word)

	if word == "hello" {
		f.Println("Hello, World")
	} else if word == "go" {
		f.Println("Start Golang")
	} else if word == "programs" {
		f.Println("learn programming")
	} else {
		f.Println("no word")
	}
}

```



#### switch 사용하기



### 반복문 



#### for 사용하기 







