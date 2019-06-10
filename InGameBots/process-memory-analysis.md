# Анализ памяти процесса

Адресное пространство процеесов рассмотрено во многих книгах и статьях. Мы изучим только те аспекты этого вопроса, которые имеют отношение к поиску, чтению и записи в память.

## Адресное пространство процесса

Начнём с того, что ещё раз подчеркнём отличие исполняемого файла и запущенного процесса ОС. Файл - это некоторые данные, записанные на устройство хранения информации (например жесткий диск). Исполняемый файл содержит инструкции (или машинный код) для процессора.

Когда вы запускаете исполняемый файл, ОС выполняет несколько последовательных действий. Первое - чтение содержимого файла с устройства хранения и запись его в оперативную память (random-access memory или RAM). Благодаря этому процессор получает намного более быстрый доступ к инструкциям, поскольку скорость его интерфейса с оперативной памятью на несколько порядков выше чем с любым диском.

После загрузки исполняемого файла в оперативную память, ОС загружает все необходимые для его работы динамические библиотеки. После этого шага, процесс готов к выполнению. Для этого он должен получить процессорное время, т.е. время в течении которого CPU будет выполнять его инструкции. В многозадачных ОС распределением этого времени занимается программа **планировщик** (или scheduler). Каждый процесс получает единицы времени (тики или секунды) в зависимости от своего приоритета.

Чем занимается процесс? Чтобы ответить на этот вопрос, заглянем в типичный исполняемый файл. В основном он содержит алгоритмы обработки и интерпретации каких-то данных. Мы можем прийтий к выводу, что основная работа типичного процесса заключается в манипуляции данными.

Следующий вопрос: где процесс хранит свои данные? Мы уже знаем, что ОС всегда хранит исполняемые инструкции процессора в оперативной памяти. В случае процесса, он имеет право свободно выбирать место хранения данных. Это может быть жесткий диск, оперативная память или даже удалённый компьютер (например игровой сервер подключённый по сети). Большая часть данных, необходимых во время работы процесса копируются в оперативную память для ускорения доступа к ней. Поэтому, именно в RAM мы можем прочитать состояния всех игровых объектов. Они будут доступны на протяжении всего времени выполнения (или runtime) процесса.

Иллюстрация 3-2 демонстрирует элементы типичного процесса. Как правило, он состоит из нескольких модулей. Обязательный из них - это EXE, который содержит все инструкции и данные, загруженные из исполняемого файла. Другие модули (обозначенные DLL\_1 и DLL\_2) соответствуют библиотекам, подпрограммы которых вызываются из главного.

![Схема процесса](process-scheme.png)

**Иллюстрация 3-2.** *Элементы типичного процесса Windows*

Все Windows приложения используют как минимум одну системную библиотеку, которая предоставляет доступ к WinAPI функциям. Даже если вы не пользуетесь WinAPI явно в своей программе, компилятор вставляет вызовы `ExitProcess` и `VirtualQuery` автоматически в ходе компиляции. Они отвечают за корректное завершение процесса и управление его памятью.

Мы рассмотрели исполняемый файл. Теперь изучим библиотеки с подпрограммами. Они деляться на два типа: **динамически подключаемые** (dynamic-link libraries или DLL) и **статически подключаемые** (static libraries). Главное различие между ними - это время разрешения зависимостей. Когда исполняемый файл использует подпрограмму библиотеки, говорят, что он от неё зависит.

Статически подключаемые библиотеки должны быть доступны в момент компиляции. Программа компоновщик собирает их и исполняемый файл в единый выходной файл. Таким образом EXE модуль на иллюстрации 3-2 содержит машинный код и статических библиотек и исполняемого файла.

Динамически подключаемые библиотеки также должны быть доступны в момент компиляции. Однако, результирующий файл на выходе компоновщика не содержит их машинный код. Вместо этого ОС ищет и загружает эти DLL библиотеки в момент старта приложения. Если их не удаётся найти, приложение завершает свою работу с ошибкой. На иллюстрации 3-2 представлены два DLL модуля.

Рассмотрим, как именно CPU выполняет инструкции процесса. Эти инструкции - элементарные блоки более сложных высокоуровневых алгоритмов. Запущенное приложение может использовать несколько алгоритмов в процессе своей работы, и некоторые из них могут выполняться параллельно. Поток (или thread) - это часть машинного кода процесса, которая может выполняться независимо от других частей. Потоки взаимодействуют друг с другом (обмениваются информацией) через разделяемые ресурсы, например файл или область RAM. За выбор потока для исполнения отвечает уже знакомый нам планировщик ОС. Как правило, число одновременно работающих потоков определяется числом ядер процессора. Но есть технологии (например hyper-threading от Intel), позволяющие более эффективно использовать мощности процессора и исполнять сразу два потока на одном ядре.

Иллюстрация 3-2 демонстрирует, что модули могут содержать несколько потоков, а могут не содержать ни одного. EXE модуль всегда имеет главный поток (main thread), который получает управление при старте приложения.

Рассмотрим структуру памяти типичного процесса. Иллюстрация 3-3 демонстрирует адресное пространство процесса, состоящего из двух модулей: EXE и DLL библиотеки. Адресное пространство разделено на блоки памяти, называемые **сегментами**. У каждого сегмента есть **базовый адрес**, длина и набор прав доступа (на запись, чтение и исполнение). Разделение памяти на сегменты упрощает задачу контроля доступа к памяти. ОС может оперировать блоками памяти, а не отдельными адресами.

![Схема памяти процесса](process-memory-scheme.png)

**Иллюстрация 3-3.** *Адресное пространство типичного процесса*

Процесс на иллюстрации 3-3 имеет три потока (включая главный). У каждого потока есть свой **сегмента стека**. Стек - это область памяти, организованная по принципу "последним пришёл — первым вышел" ("last in — first out" или LIFO). Она инициализируется ОС при старте приложения и используется для хранения переменных и вызова подпрограмм. В стеке сохраняется адрес инструкции следующей за вызовом подпрограммы. После её завершения процесс продолжает свое выполнение с инструкции по этому адресу.

Кроме сегментов стека, у процесса есть несколько **сегментов динамической памяти** (heap). Каждый поток имеет к ним доступ.

Каждый модуль процесса имеет обязательные сегменты: `.text`, `.data` и `.bss`. Также есть дополнительные сегменты (например `.rsrc`), которые могут отсутствовать. Они не представлены на схеме 3-3. 

Таблица 3-1 кратко описывает каждый сегмент иллюстрации 3-3.

**Таблица 3-1.** *Описание сегментов*

| Сегмент | Описание |
| --- | --- |
| Стек главного потока | Содержит автоматические переменные (память под которые выделяется при входе в блок области видимости и освобождается при выходе из него), стек вызовов с адресами возврата из подпрограмм и их входные параметры. |
| Динамическая память ID 1 | Дополнительный сегмент памяти, который создаётся при переполнения сегмента динамической памяти ID 0. |
| Динамическая память ID 0 | ОС создает этот сегмент всегда при запуске процесса. Он используется по-умолчанию для хранения переменных. |
| Стек потока 2 | Выполняет те же функции, что и стек главного потока, но используется только потоком 2. |
| `.text` EXE модуля | Содержит машинный код модуля. |
| `.data` EXE модуля | Содержит статические и не константные глобальные переменные модуля EXE, которые инициализируются каким-то значением при создании. |
| `.bss` EXE модуля | Содержит статические и не константные глобальные переменные модуля EXE, которые не инициализируются при создании. |
| Стек потока 3 | То же самое, что и стек потока 2, только используется потоком 3. |
| Динамическая память ID 2 | Дополнительный сегмент памяти, расширяющий сегмент динамической памяти ID 1 при его переполнении. |
| `.text` DLL модуля | Содержит машинный код модуля. |
| `.data` DLL модуля | Содержит статические и не константные глобальные переменные модуля DLL, которые инициализируются каким-то значением при создании. |
| `.bss` DLL модуля | Содержит статические и не константные глобальные переменные модуля DLL, которые не инициализируются при создании. |
| Динамическая память ID 3 | Дополнительный сегмент памяти, расширяющий сегмент динамической памяти ID 2 при его переполнении. |

