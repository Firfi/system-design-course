# 1. Laba1

## Event Storming

https://miro.com/app/board/uXjVMJZiOTQ=/?share_link_id=263702489982

### Логика группировки в контексты:

- Связи - контексты это натурально кластеры с бОльшим количеством внутренних связей, чем внешних
- специальные требования - актуально для лендоса Кандидатов с их ддосом

## 2. Данные

https://miro.com/app/board/uXjVMJfuw-c=/?share_link_id=744965036979

замечания и вопросы
 
- стрелочки пошли вразнос, miro не умеет в их наложение
- я чувствую, что можно обойтись без стрелочек просто цветами; чтение/запись, с таким же правилом овнершипа как в Расте
- связи данных будто бы нормально уместились в ES (первую часть)
- я пока не понял, зачем мне это нужно и чем мне это помогло:
- в первую очередь, прописывание связей данных (многие-ко-многим итд)
- во-вторых, мне неочевидно, что нового привносит модель данных в уже получившийся ES, если в него включать вью и связи межлу вью 

## 3. Общая модель

https://miro.com/app/board/uXjVMJcG_DA=/?share_link_id=781651861440

## 4. Реализация

Выбираю монолиты под монорепо:

### 1. Лендос Кандидатов

- Требование - точно будет дудос, обезопасим себя заранее
- Почти нет связей с другими частями.

### 2. Казино

- Для логики пользовательского продукта это отвлекающий треш
- Сильно иные требования - например не меняется часто

### 3. Основной монолит

При условии что мы сохраняем модульность - типа как у Ибрагима в рейлс /modules/ папке

- Машина состояний тасков
- Склад
- Айчар и эвалюация
- Биллинг и инвойсы

#### Таски

Сразу готовим Таски как event sourcing; дофига модулей читает их ивенты и последнее состояние; и будет часто хотеться посмотреть историю таска 

Даём доступ к Ивентам для Казино через rabbitmq или SQS; в монолите используем какой-нибудь локальный ивент эмиттер. С оглядкой чтобы пустить это в кафку если/когда захотим.

## 5. Спорные места

1. У меня странное ощущение от HR + Evaluation; они как будто оба трогают Воркера и им владеют. 

При этом, очень много кто читает Воркера. Им негоже знать об HR и Evaluation. Возможно Воркеру нужен свой хранилище/модуль, но возможно это оверкилл.

2. Таск читают вообще все. Интересно было бы узнать правильную реализацию этого. Как будто бы это стрим ивентов + каждый сервис хранит свои аггрегаты, но я в этом пока не силён.

3. Не включена служба нотификаций. Если её добавлять в схему там очень много повторяющихся стрелок получается. Не уверен, что с этим делать. 