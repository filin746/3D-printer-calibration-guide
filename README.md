
* [роздільна здатність екструдера (E-Step)](#калібрування-e-steps-екструдера)
  * [Bowden](#для-bowden-екструдера)
  * [Direct](#для-direct-екструдера)
* Потік ()
* Відкат (Retract)
* Температура друку
* [Датчик температури (PID)](#калібрування-pid)
  * [Для Marlin через gcode](#калібрування-через-com-порт-для-marlin)

# Калібрування E-steps екструдера

## Для bowden екструдера

Відкрутіть тефлонову трубочку від екструдера

[фото]

Вставляємо філамент, відрізаємо рівненько щоб не торяав з екструдера

[фото]

Перед тим як давити філамент потрібно або розігріти хот-енд або вимкнути перевірку температури за допомогою gcode (для Marlin)

```gcode
M302 P1
```

Це потрібно тому що принтер не дозволить давити пруток на холодну.

Через екран видавлюємо 100-300мм (чим більше тим вище точність калібрування) Або давимо за допомогою команди

E - кількість в міліметрах<br>
F - швидкість в міліметрах на хвилину

```gcode
M83
G1 E100 F100
M82
```

Міряємо і запамятовуємо скільки видавилось по факту.

Заходимо через екран в налаштування принтеру, шукаємо параметр E-Steps.

Підставляємо наші данні в формулу ([або користуємось онлайн калькулятором](todo))

```
N = C * E / D
```

Де:<br>
N - Нове значення E-Steps<br>
D - Скільки міліметрів прутка видавилось<br>
E - Поточне значення E-Steps<br>
C - Скільки міліметрів прутка видавлювали

Вводимо розраховане значення та зберігаємо в EPROMM

Якщо вимикали перевірку температури через gcode то не забудьте увімкнути назад

```gcode
M302 P0
```

## Для direct екструдера

// todo

# Калібрування PID

[Що таке PID](https://ru.wikipedia.org/wiki/%D0%9F%D0%98%D0%94-%D1%80%D0%B5%D0%B3%D1%83%D0%BB%D1%8F%D1%82%D0%BE%D1%80)

## Калібрування через COM порт для Marlin

Рекомендується запускати калібрування з умовами максимально наближеними до умов реального друку, тобто філамент має бути встановленим та має бути увімкнений вентилятор. Увімкнути вентилятор можна з інтерфейсу принтера або через gcode (в даному прикладі 100% швидкість обертання вентилятора):

```gcode
M106 S255
```

Ви можете почати цей процес з хот-енду за кімнатної температури або заздалегіть розігріти. Для налаштування хот-енду введіть у терміналі:

```gcode
M303 E0 S200 U1
```

Це дозволить відкалібрувати хот-енд на 200 градусів. Значення S можна змінити відповідно до тієї температури за якої ви в більшості друкуєте. U1 означає, що результат зберігається в оперативній пам'яті, і ми можемо негайно зберегти його в EEPROM, надіславши:

```gcode
M500
```

Для стола PIDTEMPBED має бути увімкнений у прошивці, тоді команда досить схожа:

```gcode
M303 E-1 S60 U1
```

Код Е-1 замість E якраз і зазначає що ми калібруємо стіл а не хот-енд, а температура встановлюється на 60 градусів. За потреби замініть на вашу температуру. Після цього ще раз збережіть у EEPROM за допомогою:

```gcode
M500
```
