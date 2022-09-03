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

```
#### Программа № 3. Напишите программу, которая выводит числа от 1 до 100, которые делятся на 3. 
#### Содержимое файла скрипта test3.go:
```

```
