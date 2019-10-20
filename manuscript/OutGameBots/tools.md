## Инструменты для разработки

Инструменты для разработки внутриигровых и внеигровых ботов различаются. В первом случае нам нужны эффективные средства для доступа к памяти процесса игры и манипуляции его данными. Внеигровые боты полностью замещают собой игровой клиент и дублируют его основные возможности.

### Язык программирования

Многие из существующих внеигровых ботов написаны на C++. Этот язык хорошо интегрируется с WinAPI, а кроме того для него существует много сторонних библиотек, в том числе для работы с сетью и криптографией. C++ – отличный инструмент для разработки ботов. Но в этой главе мы воспользуемся другим языком для наших примеров.

Мы будем использовать скриптовый язык **Python** по нескольким причинам. Прежде всего, он лаконичнее C++. Благодаря этому наши примеры станут короче и понятнее для чтения. Также у Python есть библиотеки (известные как **модули**) для работы с сетью и криптографией. Эти возможности очень важны для разработки внеигровых ботов.

Для работы с Python подойдёт практически любая IDE. Я предпочитаю [Notepad++](http://notepad-plus-plus.org), которым мы пользовались во второй главе.

Есть два варианта установки Python и криптографической библиотеки. Первый вариант – Python последней версии 3.6.5 и библиотека PyCryptodome. PyCryptodome – это [**ответвлённый проект**](https://ru.wikipedia.org/wiki/%D0%A4%D0%BE%D1%80%D0%BA) библиотеки PyCrypto. В нём лучше реализована поддержка ОС Windows. К сожалению, этот проект не имеет некоторых устаревших возможностей PyCrypto. Они вряд ли понадобятся при разработке реальных ботов, но могут быть полезны для учебных целей при знакомстве с криптографией. Второй вариант установки подразумевает более старую версию Python 3.3.0 и библиотеку PyCrypto.

Все примеры этой главы корректно исполняются на обеих версиях Python 3.6.5 и 3.3.0. Но если вы выберите вариант с PyCryptodome, вы не сможете запустить несколько примеров. Они не так важны, и будет достаточно просто рассмотреть их код.

Для установки Python 3.3.0 и библиотеки PyCrypto выполните следующие действия:

1. Скачайте Python 3.3.0 с [официального сайта](https://www.python.org/ftp/python/3.3.0/python-3.3.0.msi).

2. Установите Python. Выберите путь установки по умолчанию: `C:\Python33`.

3. Скачайте [неофициальную сборку](http://www.voidspace.org.uk/python/pycrypto-2.6.1/pycrypto-2.6.1.win32-py3.3.msi) библиотеки PyCrypto.

4. Установите библиотеку. В процессе установки Python будет найден автоматически.

Инструкция по установке Python 3.6.5 и библиотеки PyCryptodome:

1. Скачайте Python 3.6.5 с [официального сайта](https://www.python.org/downloads/release/python-365).

2. Установите его по пути по умолчанию: `C:\Program Files\Python36`.

3. Скачайте скрипт `get-pip.py` с [сервера bootstrap](http://bootstrap.pypa.io/get-pip.py). Этот скрипт устанавливает менеджер модулей `pip`. С его помощью вы сможете скачивать нужные вам модули Python.

4. Запустите `get-pip.py` из командной строки:
```
get-pip.py --user
```
Когда скрипт закончит свою работу, вы увидите сообщение с путём установки менеджера pip. В моём случае это `C:\Users\ilya.shpigor\AppData\Roaming\Python\Python36\Scripts`.

5. Перейдите по пути установки pip и запустите его:
```
pip install --user pycryptodome
```
По этой команде будет скачана библиотека PyCryptodome.

После установки любой версии Python нужно проверить, что путь до интерпретатора `python.exe` попал в переменную окружения `PATH`. Для этого выполните следующие действия:

1. Откройте диалог "Control Panel" ➤ "System" ➤ "Advanced system settings" ("Панель управления" ➤ "Система" ➤ "Дополнительные параметры системы"). Нажмите кнопку "Environment Variables" (переменные среды). Вы увидите диалог с двумя списками.

2. В списке "System variables" (переменные системы) найдите переменную "PATH". Выберите её левым щелчком мыши.

3. Нажмите кнопку "Edit" (Редактирование). Вы увидите текущий список путей в переменной `PATH`.

4. Добавьте в список ваш путь установки Python, если его там нет.

Теперь ваша система готова к запуску примеров этой главы.

Язык Python кросс-платформенный. Это значит, что написанные на нём скрипты можно запускать на Windows, Linux и MacOS с незначительными изменениями.

### Анализатор трафика

[**Wireshark**](http://www.wireshark.org) – один из самых известных анализаторов трафика с открытым исходным кодом. Благодаря ему вы сможете перехватывать весь входящий и исходящий трафик с указанной сетевой платы, просматривать его в удобном интерфейсе пользователя, фильтровать пакеты, выводить статистику и сохранять результат на жёстком диске. Кроме этого Wireshark имеет функции для интерпретации данных и расшифровки большинства сетевых протоколов.

### Конфигурация Windows

В этой главе мы будем работать с сетевыми приложениями. Каждое из них состоит из двух частей (клиент и сервер), запущенных на разных компьютерах, которые соединены друг с другом через сеть. Для тестирования таких приложений нужны либо два компьютера, либо специальные средства вроде виртуальной машины. В этом случае одна часть приложения запускается на хост-системе (ваша ОС), а другая часть в виртуальной машине (гостевая система). Системы подключаются друг к другу через эмулируемую локальную сеть.

К счастью, у современных ОС есть возможность запуска и отладки сетевых приложений без вспомогательных компьютеров или виртуальных машин. Для этой цели служит специальный сетевой интерфейс, известный как **loopback** (петля). Обе части сетевого приложения, запущенные на одном компьютере могут обмениваться сетевыми пакетами через loopback. При этом они ведут себя практически так же, как если бы взаимодействовали через реальную сеть.

По умолчанию loopback интерфейс отключен в Windows. Чтобы запустить наши тестовые примеры, вам потребуется его включить. Для этого выполните следующие шаги:

1. Запустите Device Manager (диспетчер устройств). Вы можете сделать это через Control Panel (панель управления) или набрав команду "Device Manager" в меню Start (пуск).

2. Выберите корневой элемент в дереве устройств окна Device Manager.

3. Выберите пункт меню "Action" ➤ "Add legacy hardware" ("Действие" ➤ "Установить старое устройство"). Откроется диалог "Add Hardware" (установить устройство).

4. Нажмите кнопку "Next" (далее) на первой странице диалога.

5. На второй странице диалога выберите пункт "Install the hardware that I manually select from a list (Advanced)" (установка оборудования, выбранного из списка вручную). Нажмите кнопку "Next".

6. В списке "Common hardware types" (стандартные типы оборудования) выберите пункт "Network adapters" (сетевые платы). Нажмите кнопку "Next".

7. Выберите производитель "Microsoft" и сетевую плату "Microsoft Loopback Adapter". Нажмите кнопку "Next" на этой и следующей страницах.

8. Когда процесс установки завершится, нажмите кнопку "Finish" (завершить).

После установки loopback интерфейса, его необходимо включить. Для этого выполните следующие действия:

1. Откройте окно "Network and Sharing Center" (центр управления сетями и общим доступом). Это можно сделать через меню "Start".

2. Щёлкните по пункту "Change adapter settings" (изменение параметров адаптера) в левой части окна. Откроется новое окно "Network Connections" (сетевые подключения).

3. Правым щелчком мыши по иконке "Microsoft Loopback Adapter" откройте всплывающее меню. В нём выберите пункт "Enable" (включить).

Теперь loopback интерфейс готов к работе.