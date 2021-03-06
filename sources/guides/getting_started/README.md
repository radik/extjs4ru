# Введение

## 1. Требования

### 1.1 Web Browsers

Ext JS 4 поддерживает все популярные браузеры, начиная с Internet Explorer 6 до последней версии Google Chrome. Но все же, для удобства отладки при разработке рекомендуется использовать один из следующих браузеров:

- [Google Chrome](http://www.google.com/chrome/) 10+
- [Apple Safari](http://www.apple.com/safari/download/) 5+
- [Mozilla Firefox](http://www.mozilla.com/en-US/firefox/fx/) 4+ with the [Firebug](http://getfirebug.com/) Web Development Plugin

Это руководство предполагает, что вы используете последнюю версию Google Chrome. Если у вас до сих пор нет Chrome, выделите немного времени для установки и ознакомьтесь с [Chrome Developer Tools](http://code.google.com/chrome/devtools/docs/overview.html).

### 1.2 Web Server

Хотя локальный web-сервер и не является обязательным для использования Ext JS 4, настоятельно советуем для разработки установить хотя бы один, так как [XHR-запросы](http://en.wikipedia.org/wiki/XHR) по локальному [file:// протоколу](http://en.wikipedia.org/wiki/File_URI_scheme) для большинства браузеров попадают под [правила ограничения домена](http://en.wikipedia.org/wiki/Same_origin_policy).
Если у вас еще не установлен локальный веб-сервер, рекомендуется установить Apache HTTP Server.

- [Instructions for installing Apache on Windows](http://httpd.apache.org/docs/current/platform/windows.html)
- [Instructions for installing Apache on Linux](http://httpd.apache.org/docs/current/install.html)
- Mac OS X поставляется с предустановленным apache, который можно активировать перейдя в "System Preferences > Sharing" и поставив галочку напротив "Web Sharing".

После установки или активации Apache вы можете проверить, что он запущен открыв в своем браузере [localhost](http://localhost/).  Вы должны увидеть стартовую страницу с информацией, что Apache HTTP Server установлен и запущен.

### 1.3. Ext JS 4 SDK

Скачайте [Ext JS 4 SDK][sdk-download]. Распакуйте архив в папку "extjs" в корневой директории вышего веб-сервера.  Если вы не уверены, где находиться корневая директория, обратитесь к документации.
Корневая директория вашего веб-сервера может меняться в зависимости от вашей операционной системы, но если вы используете Apache, то она должна быть:

- для Windows - "C:\Program Files\Apache Software Foundation\Apache2.2\htdocs"
- Linux - "/var/www/"
- Mac OS X - "/Library/WebServer/Documents/"

После того, как все это проделаете, перейдите по адресу [http://localhost/extjs/index.html](http://localhost/extjs/index.html) в вашем браузере. Если увидете страницу приветствия Ext JS 4 то все установлено верно.

## 2. Структура приложения

### 2.1 Базовая структура

Хотя все описанные ниже советы не являются обязательными, но они являются наилучшими практиками, придерживаясь которых, вы сможете сделать свое приложение удобным для расширения и поддержки.
Структуру Ext JS приложения рекомендуется организовать следующим образом:

    - appname
        - app
            - namespace
                - Class1.js
                - Class2.js
                - ...
        - extjs
        - resources
            - css
            - images
            - ...
        - app.js
        - index.html

- `appname` это директория, которая содержит все ресурсы вашего приложения
- `app` содержит все ваши классы, имена которых должны соответствовать рекомендациям, описанным в руководстве [Class System](#/guide/class_system)
- `extjs` содержит файлы Ext JS 4 SDK
- `resources` содержит дополнительные CSS, файлы с изображениями, которые отвечают за внешний вид приложения, и другие статичные ресурсы (XML, JSON, etc.)
- `index.html` HTML документ, являющийся входной точкой вашего приложения
- `app.js` содержит логику вашего приложения.

Пока не торопитесь создавать все эти файлы. Для начала сконцентрируемся на минимальном количестве кода необходимом для запуска Ext JS приложения.
Для этого мы создадим простое Ext JS приложение типа "hello world", которое назовем "Hello Ext".  Прежде всего создайте следующие папку и файлы в корневой директории вашего веб-браузера.

    - helloext
        - app.js
        - index.html

Далее распакуйте архив с Ext JS 4 SDK в папку `extjs` внутри `helloext`.

Типичное Ext JS приложение содержится в единственном HTML файле - `index.html`.  Откройте `index.html` и добавьте следующий html код:

    <html>
    <head>
        <title>Hello Ext</title>

        <link rel="stylesheet" type="text/css" href="extjs/resources/css/ext-all.css">
        <script type="text/javascript" src="extjs/ext-debug.js"></script>
        <script type="text/javascript" src="app.js"></script>
    </head>
    <body></body>
    </html>

- `extjs/resources/css/ext-all.css` содержит всю информацию о стилях для всего framework
- `extjs/ext-debug.js` содержит минимальное множество классов ядра Ext JS 4
- `app.js` будет содержать код вашего приложения

Теперь вы готовы написать код вашего приложения. Откройте `app.js` и вставьте следующий JavaScript код:

    Ext.application({
        name: 'HelloExt',
        launch: function() {
            Ext.create('Ext.container.Viewport', {
                layout: 'fit',
                items: [
                    {
                        title: 'Hello Ext',
                        html : 'Hello! Welcome to Ext JS.'
                    }
                ]
            });
        }
    });

Теперь откройте ваш браузер и перейдите по адресу [http://localhost/helloext/index.html](http://localhost/helloext/index.html). Вы должны увидеть панель с заголовком, содержащим текст "Hello Ext", и привествие в рабочей области панели.

### 2.2 Динамическая загрузка

Откройте Chrome Developer Tools и переключитесь на вкладку Console. Теперь перезагрузите страницу с приложением Hello Ext. Вы должны увидеть в консоли следующее предупреждение:

{@img loader-warning-viewport.png testing}

Ext JS 4 содержит систему для динамической подгрузки JavaScript-ресурсов, которая загружает файлы приложения по мере необходимости.
В нашем примере `Ext.create` создает экземпляр `Ext.container.Viewport`. При вызове `Ext.create` загрузчик  сначала проверит определен ли `Ext.container.Viewport`. Если он неопределен, то загрузчик попытается загрузить JavaScript-файл, который содержит код для `Ext.container.Viewport`, перед тем как создать экземпляр объекта viewport. В нашем примере файл `Viewport.js` будет загружен, но приложение определит, что происходит это не оптимальным способом. Так как мы загружаем файл `Viewport.js` только после того как экземляр `Ext.container.Viewport` будет запрошен, выполнение нашего скрипта остановится до тех пор, пока файл не загрузится, что приведет к некоторой задержке по времени. Эта задержка увеличится, если мы будем многократно вызывать Ext.create, так как приложению придется ждать пока загрузятся все необходимые файлы один за другим.

Чтобы исправить это, необходимо добавить следующую строку в вызов `Ext.application`:

`Ext.require('Ext.container.Viewport');`

Это обеспечит загрузку файла, содержащего код для `Ext.container.Viewport`, до того как приложение запустится. Вы не должны больше видеть предупреждения от `Ext.Loader` после перезагрузки страницы.

### 2.3 Способы подключения библиотек

Когда вы распакуете скачанный архив с Ext JS 4, вы увидете следующие файлы:

1. `ext-debug.js` - Этот файл используется только во время разработки. В нем содержится минимальное количество классов Ext JS необходимых для запуска. Другие дополнительные файлы должны быть подгружены динамически в виде отдельных файлов, как показано выше.

2. `ext.js` - то же самое что и `ext-debug.js`, но минифицировано для использования в production. Это файл должен быть ииспользован вместе с `app-all.js` файлом вашего приложения. (см. раздел *3*)

3. `ext-all-debug.js` - Этот файл содержит всю библиотеку Ext JS. Этот файл может помочь когда вы начинаете изучение Ext JS, но `ext-debug.js` в большенстве случаев все же предпочительней для разработки реальных приложений.

4. `ext-all.js` - Это минифицированная версия `ext-all-debug.js`, которая может быть использована в production'е. Однако не рекомендуется этого делать, так как большинство приложений не использует все классы, которые в ней содержатся. Вместо это рекомендуется использовать свою собственную сборку. Как ее можно сделать описано в разделе *3*.

## 3. Публикация

Недавно представленный инструмент Sencha SDK Tools ([скачать][sdk-download]) делает публикацию любого Ext JS 4 приложение проще чем когда-либо. Он позволяет генерировать манифест файл, описывающий все JavaScript зависимости в формате JSB3 (JSBuilder file format), и создавать собственную сборку, которая содержит только необходимый для вашего приложения код.

После того как вы установили SDK Tools откройте окно терминала и перейдите в директорию вашего приложения.

    cd path/to/web/root/helloext

После этого вам прийдется лишь запустить пару простых команд. Первая сгенерирует JSB3 файл:

    sencha create jsb -a index.html -p app.jsb3

Для приложений, построенных в сочетании с серверным языком, такими как PHP, Ruby, ASP и др., просто замените `index.html` на реальный URL вашего приложения:

    sencha create jsb -a http://localhost/helloext/index.html -p app.jsb3

Эта команда сканирует ваш `index.html` файл на наличие всех реально используемых файлов приложения и фреймворка, а потом создает файл `app.jsb3`. Генерирование сначала JSB3 файла позволяет нам при необходимости редактировать `app.jsb3` прежде чем делать build - это может быть удобно, если у вас есть дополнительные ресурсы, которые должны быть скопированы. В большинстве случаев мы можем сразу перейти к build'у нашего приложения при помощи следующей команды:

    sencha build -p app.jsb3 -d .

Будут созданы два файла, основанные на JSB3 файле:

1. `all-classes.js` - Этот файл содержит все классы вашего приложения. Он не минифицирован, поэтому его удобно использовать для отладки. В нашем случае этот файл пустой, так как приложение "Hello Ext" не содержит классов.

2. `app-all.js` - Этот файл содержит ваше приложение и все необходимые для него классы Ext JS. Он минифицирован и представляет собой готовую для публикации версию `all-classes.js + app.js`.

Для production версии Ext JS приложения понадобится собственный `index.html`. Обычно вы будете создавать его при помощи серверной части вашего приложения, а пока создадим новый файл `index-prod.html` в директории `helloext`:

    <html>
    <head>
        <title>Hello Ext</title>

        <link rel="stylesheet" type="text/css" href="extjs/resources/css/ext-all.css">
        <script type="text/javascript" src="extjs/ext.js"></script>
        <script type="text/javascript" src="app-all.js"></script>
    </head>
    <body></body>
    </html>

Обратите внимание, что `ext-debug.js` был заменен на `ext.js`, а `app.js` на `app-all.js`. Если вы откроете [http://localhost/helloext/index-prod.html](http://localhost/helloext/index-prod.html) в вашем браузере, вы должны увидеть production версию приложения "Hello Ext".

## 4. Для дальнейшего чтения

1. [Система классов](#/guide/class_system)
2. [Архитектура MVC приложения](#/guide/application_architecture)
3. [Layouts и Containsers](#/guide/layouts_and_containers)
4. [Работа с данными](#/guide/data)

[sdk-download]: http://www.sencha.com/products/extjs/
