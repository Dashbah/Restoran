﻿**Readme для ИДЗ№3 по КПО.**

**ИДЗ выполнили Баханкова Дарья Сергеевна и Морин Илья Олегович.**

**БПИ №215**



Для корректного выполнения данного объемного ДЗ было принято решение построить следующую схему зависимостей агентов и объектов:

![image](https://user-images.githubusercontent.com/90968766/228000107-c30d244a-84dd-427d-ad07-1648f50bf7ca.png)









Далее были написаны модели по JSON файлам. Десириализатор был написан с использованием библиотеки GSON (от Google), но так как исходные JSONы были не очень корректны, поэтому их пришлось немного отредактировать. Ниже приведен пример исходного JSON файла и отредактированного.





**Исходный JSON:**


```
{

`   `"dish\_cards":[

`      `{

`         `"card\_id":518,

`         `"dish\_name":"Princess Nuri tea bag in a paper cup",

`         `"card\_descr":"pouring boiled water into a paper cup + 2 bags of sugar",

`         `"card\_time":0.15,

`         `"equip\_type":25,

`         `"operations":[

`            `{

`               `"oper\_type":17,

`               `"oper\_time":0.15,

`               `"oper\_async\_point":0,

`               `"oper\_products":[

`                  `{

`                     `"prod\_type":18,

`                     `"prod\_quantity":1

`                  `},

`                  `{

`                     `"prod\_type":23,

`                     `"prod\_quantity":2

`                  `},

`                  `{

`                     `"prod\_type":24,

`                     `"prod\_quantity":0.2

`                  `},

`                  `{

`                     `"prod\_type":12,

`                     `"prod\_quantity":1

`                  `},

`                  `{

`                     `"prod\_type":19,

`                     `"prod\_quantity":1

`                  `}

`               `]

`            `}

`         `]

`      `}

`   `]

}
```




**Отредактированный JSON:**


[

`  `{

`    `"card\_id": 518,

`    `"dish\_name": "Princess Nuri tea bag in a paper cup",

`    `"card\_descr": "pouring boiled water into a paper cup + 2 bags of sugar",

`    `"card\_time": 0.15,

`    `"equip\_type": 25,

`    `"operations": [

`      `{

`        `"oper\_type": 17,

`        `"oper\_time": 0.15,

`        `"oper\_async\_point": 0,

`        `"oper\_products": [

`          `{

`            `"prod\_type": 18,

`            `"prod\_quantity": 1

`          `},

`          `{

`            `"prod\_type": 23,

`            `"prod\_quantity": 2

`          `},

`          `{

`            `"prod\_type": 24,

`            `"prod\_quantity": 0.2

`          `},

`          `{

`            `"prod\_type": 12,

`            `"prod\_quantity": 1

`          `},

`          `{

`            `"prod\_type": 19,

`            `"prod\_quantity": 1

`          `}

`        `]

`      `}

`    `]

`  `}

]




**Используемые архитектуры.**

Данный проект написан с использованием архитектуры MVC с использованием MAS (Multi Agent System), а также многопоточности, семафоры.

Проект был написан с использованием Gradle, с помощью его и был собран Jar-ник.


**Схема работы программы.**

На вход поступает список заказов посетителей. Каждый в свой поток. 

Далее формируется список рецептов блюд в заказе. 

Создается агент заказа. 

Резервируются продукты для заказа. На данном этапе разработки уменьшается количество продуктов на складе. В дальнейшем при нехватке продуктов будет корректироваться меню и отменяться блюдо в заказе/заказ. 

Высчитывается минимальное время ожидания заказа и выводится на табло (как в ресторанах самообслуживания)

Дальше проходимся по каждому блюду заказа и передаем его в отдельный поток, где выбираем первого свободного повара. 

Выполняем операции

Возвращаем заказ

Итого каким образом работают процессы:

Заказы - параллельно. При большом потоке посетителей такая реализация может навредить, потому что тогда мы отправляем работать 100 потоков. Это не эффективно. Лучше сделать через семафор (ограничить места в ресторане)

Блюда в заказе - параллельно через семафор. Заказ ищет свободного повара. 

Операции последовательно. Оборудование блокируется при использовании



**Результат работы.**



