---
### TRANSLATION INSTRUCTIONS FOR THIS SECTION:
### TRANSLATE THE VALUE OF THE title ATTRIBUTE AND UPDATE THE VALUE OF THE lang ATTRIBUTE.
### DO NOT CHANGE ANY OTHER TEXT.
layout: page
title: Диспетчеры процессов для приложений Express
menu: advanced
lang: ru
### END HEADER BLOCK - BEGIN GENERAL TRANSLATION
---

# Диспетчеры процессов для приложений Express

При запуске приложения Express для рабочей среды использование *диспетчера процессов* позволяет выполнять следующие задачи:

- Автоматически перезапускать приложение в случае сбоя.
- Получать аналитическую информацию о производительности среды выполнения и потреблении ресурсов.
- Изменять параметры в динамическом режиме в целях повышения производительности.
- Управлять кластеризацией.

Диспетчер процессов можно сравнить с сервером приложений: это "контейнер" для приложений, обеспечивающий развертывание и высокую готовность и позволяющий управлять приложением в среде выполнения.

Наиболее популярные диспетчеры процессов для Express и других приложений Node.js перечислены ниже:

- [StrongLoop Process Manager](#sl)
- [PM2](#pm2)
- [Forever](#forever)


Использование любого из этих трех инструментов может быть исключительно полезным, тем не менее StrongLoop Process Manager является единственным инструментом, предоставляющим универсальное решение для среды выполнения и развертывания в течение всего жизненного цикла приложения Node.js и включающим в себя, в виде единого интерфейса, все средства для каждого этапа, как до перехода в рабочий режим, так и в рабочей среде.

Ниже приводится краткий обзор каждого из этих инструментов.
Подробное сравнение можно найти в разделе [http://strong-pm.io/compare/](http://strong-pm.io/compare/).

## <a id="sl">StrongLoop Process Manager</a>

StrongLoop Process Manager (StrongLoop PM) - это рабочий диспетчер процессов для приложений Node.js. В StrongLoop PM предусмотрено встроенное распределение нагрузки, мониторинг, развертывание на нескольких хостах и графическая консоль.
С помощью StrongLoop PM можно выполнять следующие задачи:

- Компоновать, объединять в пакет и развертывать приложение Node.js в локальной или удаленной системе.
- Просматривать профайлы CPU и моментальные снимки кучи в целях оптимизации производительности и диагностирования утечек памяти.
- Поддерживать постоянную активность процессов и кластеров.
- Просматривать показатели производительности приложения.
- Без затруднений управлять развертываниями на нескольких хостах с интеграцией Nginx.
- Объединять несколько диспетчеров StrongLoop PM в распределенную среду выполнения микрослужб, управляемую из Arc.

Работать с StrongLoop PM можно посредством многофункционального интерфейса командной строки, называемого `slc`, или графического инструмента Arc. Arc имеет открытый исходный код и обеспечен экспертной поддержкой StrongLoop.

Дополнительная информация содержится на странице [http://strong-pm.io/](http://strong-pm.io/).

Полная версия документации:

- [Управляющие приложения Node (Документация StrongLoop)](http://docs.strongloop.com/display/SLC)
- [Использование StrongLoop Process Manager](http://docs.strongloop.com/display/SLC/Using+Process+Manager).

### Установка
<pre>
<code class="language-sh" translate="no">
$ [sudo] npm install -g strongloop
</code>
</pre>

### Основы использования
<pre>
<code class="language-sh" translate="no">
$ cd my-app
$ slc start
</code>
</pre>

Просмотр состояния диспетчера процессов и всех развернутых приложений:

<pre>
<code class="language-sh" translate="no">
$ slc ctl
Service ID: 1
Service Name: my-app
Environment variables:
  No environment variables defined
Instances:
    Version  Agent version  Cluster size
     4.1.13      1.5.14           4
Processes:
        ID      PID   WID  Listening Ports  Tracking objects?  CPU profiling?
    1.1.57692  57692   0
    1.1.57693  57693   1     0.0.0.0:3001
    1.1.57694  57694   2     0.0.0.0:3001
    1.1.57695  57695   3     0.0.0.0:3001
    1.1.57696  57696   4     0.0.0.0:3001
</code>
</pre>

Вывод списка всех приложений (служб), подлежащих управлению:

<pre>
<code class="language-sh" translate="no">
$ slc ctl ls
Id          Name         Scale
 1          my-app       1
</code>
</pre>

Остановка приложения:

<pre>
<code class="language-sh" translate="no">
$ slc ctl stop my-app
</code>
</pre>

Перезапуск приложения:

<pre>
<code class="language-sh" translate="no">
$ slc ctl restart my-app
</code>
</pre>

Также можно выполнить "мягкий перезапуск", при котором рабочим процессам выделяется определенное время на закрытие существующих соединений, после чего текущее приложение перезапускается:

<pre>
<code class="language-sh" translate="no">
$ slc ctl soft-restart my-app
</code>
</pre>

Удаление приложения из числа подлежащих управлению:

<pre>
<code class="language-sh" translate="no">
$ slc ctl remove my-app
</code>
</pre>

## <a id="pm2">PM2</a>

PM2 - это диспетчер процессов рабочей среды для приложений Node.js, в котором предусмотрен встроенный инструмент распределения нагрузки. PM2 позволяет всегда поддерживать приложения в рабочем состоянии и перезагружать их без простоев, а также обеспечивает выполнение общих задач системного администрирования.  PM2 также позволяет управлять ведением протоколов, мониторингом и кластеризацией приложений.

Дополнительная информация содержится на странице [https://github.com/Unitech/pm2/](https://github.com/Unitech/pm2).

### Установка

<pre>
<code class="language-sh" translate="no">
$ [sudo] npm install pm2 -g
</code>
</pre>

### Основы использования

При запуске приложения с помощью команды `pm2` необходимо указать путь к приложению. Тем не менее, при остановке, перезапуске или удалении приложения можно указать только имя или ИД приложения.

<pre>
<code class="language-sh" translate="no">
$ pm2 start app.js
[PM2] restartProcessId process id 0
┌──────────┬────┬──────┬───────┬────────┬─────────┬────────┬─────────────┬──────────┐
│ App name │ id │ mode │ pid   │ status │ restart │ uptime │ memory      │ watching │
├──────────┼────┼──────┼───────┼────────┼─────────┼────────┼─────────────┼──────────┤
│ my-app   │ 0  │ fork │ 64029 │ online │ 1       │ 0s     │ 17.816 MB   │ disabled │
└──────────┴────┴──────┴───────┴────────┴─────────┴────────┴─────────────┴──────────┘
 Use the `pm2 show <id|name>` command to get more details about an app.
</code>
</pre>

При запуске приложения с помощью команды `pm2` оно немедленно переводится в фоновый режим. Фоновым приложением можно управлять из командной строки с помощью различных команд `pm2`.

После запуска приложения с помощью команды `pm2` оно регистрируется в списке процессов PM2 с указанием ИД. Затем можно управлять приложениями с тем же именем из различных каталогов в системе, используя только их ИД.

Обратите внимание на то, что если запущено несколько приложений с одинаковым именем, команды `pm2` применяются ко всем таким приложениям. Поэтому для управления отдельными приложениями используйте не имена, а ИД.

Список всех запущенных процессов:

<pre>
<code class="language-sh" translate="no">
$ pm2 list
</code>
</pre>

Остановка приложения:

<pre>
<code class="language-sh" translate="no">
$ pm2 stop 0
</code>
</pre>

Перезапуск приложения:

<pre>
<code class="language-sh" translate="no">
$ pm2 restart 0
</code>
</pre>

Просмотр подробной информации о приложении:

<pre>
<code class="language-sh" translate="no">
$ pm2 show 0
</code>
</pre>

Удаление приложения из реестра PM2:

<pre>
<code class="language-sh" translate="no">
$ pm2 delete 0
</code>
</pre>


## <a id="forever">Forever</a>

Forever представляет собой простой инструмент интерфейса командной строки, обеспечивающий непрерывное (бесконечное) выполнение определенного сценария. Благодаря простому интерфейсу Forever является идеальным инструментом для выполнения не столь масштабных развертываний приложений и сценариев Node.js.

Дополнительная информация содержится на странице [https://github.com/foreverjs/forever/](https://github.com/foreverjs/forever).

### Установка

<pre>
<code class="language-sh" translate="no">
$ [sudo] npm install forever -g
</code>
</pre>

### Основы использования

Для запуска сценария воспользуйтесь командой `forever start` и укажите путь к сценарию:

<pre>
<code class="language-sh" translate="no">
$ forever start script.js
</code>
</pre>

Эта команда запустит сценарий в режиме демона (в фоновом режиме).

Для запуска сценария таким образом, чтобы он был соединен с терминалом, не указывайте `start`:

<pre>
<code class="language-sh" translate="no">
$ forever script.js
</code>
</pre>

Целесообразно регистрировать вывод инструмента Forever и сценария в протоколе, используя для этого опции ведения протоколов `-l`, `-o` и `-e`, как показано в примере ниже:

<pre>
<code class="language-sh" translate="no">
$ forever start -l forever.log -o out.log -e err.log script.js
</code>
</pre>

Для просмотра списка сценариев, запущенных с помощью Forever:

<pre>
<code class="language-sh" translate="no">
$ forever list
</code>
</pre>

Для остановки сценария, запущенного с помощью Forever, воспользуйтесь командой `forever stop` и укажите индекс процесса (согласно списку, выведенному с помощью команды `forever list`).

<pre>
<code class="language-sh" translate="no">
$ forever stop 1
</code>
</pre>

В качестве альтернативы, укажите путь к файлу:

<pre>
<code class="language-sh" translate="no">
$ forever stop script.js
</code>
</pre>

Для остановки всех сценариев, запущенных с помощью Forever:

<pre>
<code class="language-sh" translate="no">
$ forever stopall
</code>
</pre>

В Forever предусмотрено множество других опций, и, кроме того, данным инструментом предоставляется программный API.