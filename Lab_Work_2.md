# Сбор, обработка и визуализация тестового набора данных
Отчет по лабораторной работе #2 выполнил(а):
- Сафаргалеев Никита Олегович
- РИ210914
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

## Цель работы
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity.
## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity.
- Шаг 1. В консоли Google Cloud подключил необходимые API сервисов (Drive & Sheets), создал сервисный аккаунт, создал API-ключ, выгрузил ключ сервисного аккаунта
![](https://github.com/Little-hot-dog/RTF_Work_2/blob/main/%D1%81%D0%BA%D1%80%D0%B8%D0%BD_1.png)

- Шаг 2. Реализовать запись данных из скрипта на Python в Google Spreadsheets. Данные описывают изменение темпа инфляции на протяжении 11 отсчётных периодов, с учётом стоимости игрового объекта в каждый период.
![](https://github.com/Little-hot-dog/RTF_Work_2/blob/main/%D1%81%D0%BA%D1%80%D0%B8%D0%BD_2.png)

- Шаг 3-4. Создать новый проект на Unity, который будет получать данные из Google Spreadsheets, в которую были записаны данные в предыдущем пункте. Написать функционал на Unity, в котором будет воспризводиться аудиофайл в зависимости от значения данных из таблицы.
![](https://github.com/Little-hot-dog/RTF_Work_2/blob/main/%D1%81%D0%BA%D1%80%D0%B8%D0%BD_3.png)

## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1
Код реализации собственной загрузки данных с модели на Google Spreadsheets:
```py
import gspread
import numpy as np

x = [3, 21, 22, 34, 54, 34, 55, 67, 89, 99]
x = np.array(x)
y = [2, 22, 24, 65, 79, 82, 55, 130, 150, 199]
y = np.array(y)

def model(a, b, x):
    return a * x + b

def loss_function(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)

    return (0.5 / num) * (np.square(prediction - y)).sum()


def optimize(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)

    da = (1.0 / num) * ((prediction - y) * x).sum()
    db = (1.0 / num) * ((prediction - y).sum())
    a = a - Lr * da
    b = b - Lr * db

    return a, b


def iterate(a, b, x, y, times):
    for i in range(times):
        a, b = optimize(a, b, x, y)
    return a, b


a = np.random.rand(1)
b = np.random.rand(1)

Lr = 0.000001

gc = gspread.service_account(filename = 'unitydatasciense-364910-be2ee0d23c91.json')
sh = gc.open('UnitySheets')

old_loss = 0

for i in range(10):
    a, b = iterate(a, b, x, y, 100 * (i + 1))

    prediction = model(a, b, x)
    loss = loss_function(a, b, x, y)

    diff_loss = abs(loss - old_loss)
    old_loss = loss

    sh.sheet1.update(('A' + str(i + 1)), str(i + 1))
    sh.sheet1.update(('B' + str(i + 1)), str(loss))
    sh.sheet1.update(('C' + str(i + 1)), str(diff_loss))
```
![](https://github.com/Little-hot-dog/RTF_Work_2/blob/main/%D1%81%D0%BA%D1%80%D0%B8%D0%BD_4.png)

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2
Задание мне показалось легким, но Unity начал выдавать ошибку, как только я начал менять условия:
```NullReferenceException: Object reference not set to an instance of an object```
Даже когда я все вернул в исхожный вариант, ошибка не пропала. 


## Выводы

Узнал как работать с Google Spreadsheets API на Python с помощью библиотеки ```gspread```, узнал как получать данные в реальном времени из Google Spreadsheets в Unity.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
