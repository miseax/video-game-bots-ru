# Доступ к памяти процесса

Мы научились вручную искать переменную в памяти процесса. Пришло время написать код, для автоматизации этого поиска. Вутриигровые боты не могут использовать программу-отладчик (например OllyDbg). Вместо этого они должны сами реализовывать необходимые функции отладчика.

## Открытие процесса

Как вы помните, перед началом работы с памятью процесса к нему нужно подключить отладчик. После этого он получает полный доступ к адресному пространства процесса. Мы выполняли это действие через диалог интерфейса пользователя. То же самое должен уметь выполнять внутриигровой бот. Рассмотрим, какими WinAPI функциями он может воспользоваться для этого.

Практически все объекты и ресурсы Windows доступны через их дескрипторы. WinAPI функция `OpenProcess` возвращает дескриптор указнного процесса. У каждого процесса, запущенного в системе, есть уникальный номер или идентификатор (process identifier или PID). Его мы должны передать в `OpenProcess` входным параметром. Получив дескриптор, мы можем обращаться к памяти процесса с помощью других WinAPI функций.