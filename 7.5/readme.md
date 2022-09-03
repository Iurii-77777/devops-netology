## **Задача 1.**
#### Установите golang. 
![Screenshot](7_5_1_.jpg)
## **Задача 2.** 
#### Знакомство с gotour.
```
Выполнено. Выглядит даже проще чем python =). 
```
## Задача 3. Написание кода.
#### Программа № 1. Напишите программу для перевода метров в футы (1 фут = 0.3048 метр).
#### Содержимое файла скрипта test.go:
```
package main
import "fmt"

 func main() {
   fmt.Print("Введите количество футов: ")
   var input float64
   fmt.Scanf("%f", &input)           
   output := input * float64(0.3048) 
   RoundUpOutput := fmt.Sprintf("( %.2f)", output)
   fmt.Println("Данное количество футов в метрах:", RoundUpOutput )
  }
```
#### Пример запуска:
```
iurii-devops@Host-SPB:~/goland$ go run test.go 
Enter value in foot: 5
Value in Meters: ( 1.52)
iurii-devops@Host-SPB:~/goland$ vim test.go 
iurii-devops@Host-SPB:~/goland$ go run test.go 
Введите количество футов: 6
Данное количество футов в метрах: ( 1.83)
```
#### Программа № 2. Напишите программу, которая найдет наименьший элемент в любом заданном списке.
#### Содержимое файла скрипта test2.go:
```
package main
import "fmt"

  func main() {
    spis := []int{48,96,86,68,57,82,63,70,37,34,51,32,83,27,19,97,17,3,7,9}
    current := 0
    fmt.Println ("Список чисел : ", spis)
    for i, value := range spis {
        if (i == 0) {
           current = value
        } else {
            if (value < current){
                current = value
            }
        }
    }
    fmt.Println("Минимальное число : ", current)
    }
```
####  Пример запуска:
```
iurii-devops@Host-SPB:~/goland$ vim test2.go
iurii-devops@Host-SPB:~/goland$ go run test2.go 
Список чисел :  [48 96 86 68 57 82 63 70 37 34 51 32 83 27 19 97 17 3 7 9]
Минимальное число :  3
```
#### Программа № 3. Напишите программу, которая выводит числа от 1 до 100, которые делятся на 3. 
#### Содержимое файла скрипта test3.go:
```
package main
import "fmt"
  func main() {
      for i := 1; i <= 100; i++ {
          if (i%3) == 0 {
              fmt.Print(i,"  ")
              }
          if (i%100) == 0 {
              fmt.Println()
          }
      }
  }

```
####  Пример запуска:
```
iurii-devops@Host-SPB:~/goland$ vim test3.go
iurii-devops@Host-SPB:~/goland$ go run test3.go 
3  6  9  12  15  18  21  24  27  30  33  36  39  42  45  48  51  54  57  60  63  66  69  72  75  78  81  84  87  90  93  96  99 
```
