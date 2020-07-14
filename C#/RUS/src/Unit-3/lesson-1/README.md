# Урок 1: Жизненный цикл актора.

Давайте начнём изучение данного модуля, c того что рассмотрим более подробно, из каких этапов состоит жизненный цикл актора.

### Этап № 1 Starting

На этапе старта происходит инициализация актора в нашей системе акторов. То есть создаётся экземпляр нашего актора, и ссылка на него сохраняется в нашей системе акторов. 

После того как актор был создан, система акторов отправляет ему системное сообщение Started. Что приводит к вызову метода `ProcessMessageAsync()`. Что в свою очередь позволит вам обработать этап старта актора.

### Этап № 2 Receiving Messages.

На этом этапе, наш актор полностью инициализирован и ожидают входящих сообщений для обработки.

### Этап № 3 Stopping.

В какой-то момент наш актор может перейти в состояние остановки. Переход в данное состояние может произойти в нескольких случаях.

1. Если по какой то причине, вы самостоятельно решите из своей бизнес логики вызвать методы `ActorContext.Stop()` и `ActorContext.StopAsync()`
2. Из за тогу что произошла какая-то ошибка. Родительский класс из иерархии акторов может направить сообщение об остановке. 

Процесс остановки актора можно разделить на два подэтапа.

**Первый этап называется Stopping.**

На этом этапе начинается подготовка к остановке актора. Данный этап заключается в остановке дочерних акторов и вызова метода `ProcessMessageAsync()` с системным сообщением `Stop`. Благодаря этому вы сможете узнать об остановке актора и обработать это в своём коде.

**Второй этап называется Stop.**

Здесь происходит полная остановка актора и освобождения всех используемых им ресурсов.

### Этап № 4  Restarting.

Этап перегрузки актора не чем не отличается от этапа остановки за исключением того, что после остановки актор, перейдёт на этап Starting. На этом этапе ваш код получит системное сообщение `Restart`. 

### Этап № 5 Terminated.

Последний этап жизненного цикла актора называется Terminated. На этом этапе наш актор мёртв. То есть он не может больше принимать и обрабатывать сообщения, а также актор не может быть перезагружен. Перед полным отключением актора, метод `ProcessMessageAsync()` получит системное сообшение `Terminated`. 

Что бы если потребуется вы могли завершить работу вашего актора корректно и освободить используемые ресурсы

Графически жизненный цикл актора можно представить следующим образом.

![](images/3_1_1.png)

Обработку системных сообщений жизненного цикла актора, мы рассмотрим в следующем уроке.
