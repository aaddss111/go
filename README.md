package main

import (
	"fmt"
	"strconv"
)

func main() {
	numChannel := make(chan int)
	strChannel := make(chan string)

	go func() {
		for i := 0; i < 10; i++ {
			numChannel <- i
		}
		close(numChannel)
	}()

	for i := 0; i < 10; i++ {
		go func() {
			for num := range numChannel {
				strChannel <- strconv.Itoa(num)
			}
		}()
	}

	for i := 0; i < 10; i++ {
		str := <-strChannel
		fmt.Println(str)
	}
}
