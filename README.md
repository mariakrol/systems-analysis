# Systems Analysis. Home works of the educational course Systems Analysis

## Домашнее задание
- [ ] для каждого удалённого сервиса и связанных с ним сервисов посчитайте значение instability; 
- [ ] опишите, какие сервисы и боундед-контексты в каком месте и каким образом будут меняться; 
- [ ] спланируйте, как и в какой последовательности будет происходить работа. 
Можете выбрать одно из двух условий: нет людей, нет ресурсов.

## Анализ проблем
### Структурные
- [x] На энтити сервис воркеров завязана вся система. В случае изменений или падений, затронет все.
- [x] Большое количество связей
- [x] В подсистеме бухгалтерии объединены два не связанных между собой процесса
- [x] Не возможно обеспечить разный релизный цикл для частей системы, связанных с энтити сервисом
- [x] Т.к. заказ проходит жизненный цикл, его этапы должны быть изолированными контекстами для обеспечения
поддерживаемости
- [x] В текущей реализации сигнал о завершении задачи продюсируется контролем качества. Результат контроля качества - 
гипотезы по изменению/развитию системы, они не влияют на расчет и ставки => 
лишние связи

### Функциональные
- [x] Отсутствует функциональность редактирования типов услуг `US-021`
- [x] Невозможно разграничить бухгалтерию клиентов и воркеров и удовлетворить все требования так, чтобы сохранить 
поддерживаемость системы
    - Разный платежный цикл `[US-220]`, `[US-230]`
    - Расширяемость 
    - Клиентам доступны скидки `[US-040]`
- [x] Матчингом занимается отдельная команда с отдельными знаниями, терминологией и уровнем секретности алгоритма => 
зоны ответственности пересечены `[US-070]`
- [x] Отсутствует функциональность конфигурирования тестов `[US-110]`
- [x] Отсутствует функциональность мотивирования воркеров `[US-270]`
- [x] Система ставок - полусекретный компонент и у него не должно быть явных связей с основной системой

## Приоритезация этапов работы
При планировании работы исхожу из предположения, что есть достаночно свободных людей и ресурсов для планирования работы,
но инфраструктура не готова.

Т.к. система популярна (10 заказов в минуту), функциональность управления типами услуг. При этом, функциональность из 
supporting поддомена и подойдет для обкатки процесса и инфраструктуры.

На базе подготовленной инфраструктуры избавиться от слабого места в энтити-сервисе, что позволит улучшить стабильность
системы.

После того, как скоринг изолирован, команда этого сервиса может изолированно, без угрозы для системы, рефакторить сервис,
добавляя утерянную функциональность.

Центральный "монолитный" сервис содержит много связей и частей с разными характеристиками, в первую очередь стоит
вынести матчинг.

В текущей реализации контроль качества отвечает за оповещение системы о завершении задачи. необходимо развязать эти
понятия.

Центральный сервис должен отвечать за жизненный цикл задачи, контроль качества - побочная функциональность.

Т.к. требования к финансовой системе сопряжены с законом, откладывать их на конец не стоит. Работы будут производиться 
в 2 этапа: 1. выделение расчета затрат клиентов и заработков воркеров; 2. вынесение сервисов

Т.к. инфраструктура проверена и все события готовы, переходим к разделению биллинга.

Добавить функциональность мотивирования воркеров.

## Этапы работ
- [x] Добавление функциональности управления типами услуг.
- [x] Избавиться от энтити сервиса с воркерами.
- [x] Внедрение боундед-контекста конфигурирования тестов.
- [x] Вынести матчинг из центрального сервиса.
- [x] Выделить контекст, обрабатывающий завершение задачи и продюссирующий событие о завершении.
- [x] Вынести контроль качества.
- [x] Выделить контекст расчетов затрат клиентов и контекст подсчетов заработков worker-ов.
- [x] Вынести сервисы расчетов.
- [x] Добавить функциональность мативирования воркеров.

## ToDo
- [ ] Выделить характеристики у каждого сервиса;
- [ ] Выделить фитнесс-функции;
- [ ] ADR для сервиса управления типами услуг;
- [ ] ADR для типов коммуникаций с сервисом скоринга;
- [ ] ADR на смену архитектурного стиля в скоринге;
- [ ] ADR вынос матчинга
- [ ] ADR контекст завершения задачи

## Реализация
Все этаппы работ расположены по порядку на доске. Там же приведены расчеты нестабильности.
[Доска Miro](https://miro.com/app/board/uXjVMCbagM4=/?share_link_id=293524519172)

### Добавление функциональности управления типами услуг
Типы услуг интересны центральному сервису и сервису обеспечения расходниками. Функциональность может быть внесена в 
основной сервис, либо реализована в виде отдельного сервиса. 

**Сравнение подходов**

| Показатель    | Вносить функциональность в главный сервис                                                                                                   | Выделять отдельный сервис                                                    |
|:--------------|:--------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------|
| Instability   | Основной сервис: Instability ~ 0.6, склад = 0.25                                                                                            | Основной сервис: Instability ~ 0.43, склад = 0.25                            |
| Тип изменений | Только на горячую, держать лок на изменения основной точки взаимодействия с пользователем невозможно. При этом будет меняться схема данных. | Новый сервис может быть разработан отдельно и встроен в работу по готовности |
|               |                                                                                                                                             |                                                                              |

Т.к. внесение функциональности в центральный сервис необоснованно усложнит его (функциональность должна быть доступна 
только менеджерам, остальные акторы только потребляют результат) и внутренних связей с имеющейся функциональностью не
много принимаю решение вынести отдельный сервис. 
Сервис будет продюссировать стриминг событие, которое будет потребляться центральным сервисом и складом.
Сервис будет использователь документоориентированную базу.
Архитектура сервиса: layered monolith

**Характеристики**

Управление типами услуг - функциональность из generic поддомена. Наследует только характеристики, обозначенные для всей
системы в целом.

| Характеристика | Источник                                                                                      | Комментарий                                        |
|:---------------|:----------------------------------------------------------------------------------------------|:---------------------------------------------------|
| Deployability  | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц                      |
| Testability    | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц                      |
| Agility        | Требование низкого TTM                                                                        |                                                    |
| Security       | В системе есть разграничение ролей                                                            | Доступ только пользователям с ролью менеджер услуг |
| Evolvability   | Система работает в инновационном секторе и проверка гипотез с быстрой реакцией как требование |                                                    |

**ADR:** To be filled

### Удаление entity-service для воркеров и стриминг добавленных воркеров из скоринга 
Т.к. воркеры создаются в одном месте (сервис скоринга, после успешного прохождения тестирования) и не меняются в рамках 
системы, вся функциональность уже есть, достаточно начать продюссировать событие создания нового воркера, чтобы все 
заинтересованные сервисы его услышали.
Т.к. сервис скоринга - это core поддомен, который часто и интенсивно меняется, варианта копирования сервиса с 
параллельным развитием нет, большая опасность, что функциональность разойдется. Схема данных меняться не будет, поэтому
работа будет производиться "на горячую".
Первым этапом в данной части будет создание события. После того, как событие создано, поочередно будут отключаться связи
от entity-сервиса и сам сервис будет удален в момент разрыва последней связи.

**ADR:** To be filled

### Внедрение контекста создания тестов
Т.к. функциональность скоринга будет продаваться отдельно, важно ее полное соответствие исходным требованиям и консернам.
Исходя из них, у сервиса следующие бизнес-задачи:
- Подбор тестов под объект;
- Аннализ результатов тестов; 
Фактически, модуль должен содержать в себе жизненный цикл заявки соискателя:
- Новая заявка соискателя -> заявка может быть отклонена, если соискатель отбраковывался или пойти дальше на подбор тестов
- К заявке подобраны тесты
- Тесты по заявке проанализированы -> результат неудовлетворительный, соискатель отбраковывается; результат 
удовлетворительный -> новый воркер добавляется

Такая структура хорошо укладывается на пайплайн на данном этапе. Если принять во вниание то, что скейлинг будет продаваться
отдельно, то есть риск серьезных ограничений в расширяемости в будущем, т.к. возможно потребителям придется расширять 
процесс матчинга собственными этапами. Смена архитектурного стиля - очень дорогой процесс, поэтому решаю взять microkernel
сразу.
"На горячую" сделать это не получится, поэтому вариантов только 2:
1. Замораживать изменения в модуле, писать новый с нужной архитектурой и подменять старый тогда, когда новый будет готов; 
2. Продолжать разработку новой функциональности в старом модуле, писать новый под новую архитектуру, догоняя 
   функциональность. 
Второй вариант может быть весьма опасным, старый и новый модуль могут разойтись достаточно далеко и есть риск потерять
какую-то функциональность. Если бизнес готов, лучше заморозить разработку новой функциональности, т.е. выбрать вариант #1.


**Характеристики**

Скоринг потенциальных сотрудников - функциональность из core поддомена. Должна продаваться отдельно, работает в условиях 
средней нагрузки с опасностью DDoS атак.

| Характеристика        | Источник                                                                                       | Комментарий                              |
|:----------------------|:-----------------------------------------------------------------------------------------------|:-----------------------------------------|
| Deployability         | Требование низкого TTM                                                                         | *Релизный цикл модуля 1 неделя           |
| Testability           | Требование низкого TTM                                                                         | *Релизный цикл модуля 1 неделя           |
| Agility               | Требование низкого TTM                                                                         |                                          |
| Evolvability          | Система работает в инновационном секторе и проверка гипотез с быстрой реакцией как требование  |                                          |
| Security              | В системе есть разграничение ролей                                                             | Доступ пользователям с ролью HR-менеджер |
| Scalability           | Известна средняя штатная нагрузка                                                              | 1000 соискателей в сутки                 |
| Elasticity            | Высокая опасность DDoS атак                                                                    | 1000 * 10?                               |
| Maintainability       | После удавшейся атаки нужно быстро восстанавливаться                                           | [^1]                                     |
| Modifiability         | Сервис будет продаваться отдельно, будет активное развитие                                     |                                          |
| Portability           | Будет поставляться как самостоятельный продукт, д.б. отделяемым от системы                     |                                          |
| Modularity            | Продажа как отлельный продукт и быстрая изменяемость                                           |                                          |
| High model complexity | Специфика поддомена: работа с характеристиками объектов и оценкой                              |                                          |

**ADR:** To be filled

### Вынесение матчинга
Матчинг - отдельная core функциональность, над которой работает своя группа разработчиков. На функциональность не влияют
другие части системы, алгоритм секретен. В требованиях описано, что алгоритм работает по принципу map-reduce, т.о. может
быть хорошо смоделирован через pipeline.
Т.к. измение влечет смену архитектуры, данных и коммуникаций, работы будут производиться по плану:
- разработать новый сервис
- обучить центральный сервис кидать событие при создании новой задачи
- Обучить центральный сервис слушать событие назначения воркера
- Удалить функциональность из центрального модуля

**Характеристики**

Скоринг потенциальных сотрудников - функциональность из core поддомена. Алгоритм инновационный и часто меняющийся. 
Нагрузка та же, что и на сервис, где создаются задачи - 10 заказов в минуту.
Обладает характеристиками, отличными от центрального модуля - модульность - специфика алгоритма, высокая сложность 
моделей - идет сверка характеристик. 

| Характеристика        | Источник                                                                                      | Комментарий                                 |
|:----------------------|:----------------------------------------------------------------------------------------------|:--------------------------------------------|
| Deployability         | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц               |
| Testability           | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц               |
| Agility               | Требование низкого TTM                                                                        |                                             |
| Evolvability          | Система работает в инновационном секторе и проверка гипотез с быстрой реакцией как требование |                                             |
| Security              | В системе есть разграничение ролей                                                            | Доступ пользователям с ролью менеджер услуг |
| Scalability           | Известна средняя штатная нагрузка                                                             | 10 заказов в минуту                         |
| Modifiability         | Алгоритм матчинга постоянно меняется                                                          |                                             |
| Modularity            | Описывается пошаговым процессом                                                               |                                             |
| High model complexity | Специфика поддомена: сопоставление множеств                                                   |                                             |

**ADR:** To be filled

### Выделение контекста завершения заказа
В системе важную роль играет завершение задачи. После него инициируются процессы оплат, контроля качества и расчета ставок.
На данный момент, этим занимается контекст контроля качества, что нарушает зону его ответственности. Т.о. в данном сервисе
будут продьюсситься 2 события:
- Создана новая задача
- Завершена задача
Измнения повлекут за собой смену схемы данных и архитектурные изменения. Т.о. переделка масштабная и не может быть сделана
"на горячую". Варианты:
1. Замораживать изменения в модуле, писать новый с нужной архитектурой и подменять старый тогда, когда новый будет готов;
2. Продолжать разработку новой функциональности в старом модуле, писать новый под новую архитектуру, догоняя
   функциональность.
Если бизнес готов, лучше заморозить разработку новой функциональности, т.е. выбрать вариант #1.

**Характеристики**

Центральный сервис, на данный момент, содержит в себе 2 отдельные функциональности - менеджмент заказа и контроль качества.
Менеджмент заказа, кроме общих характеристик, обладает требованием на удобство, понятность и стабильность, а контроль качества - нет. 
При этом, к менеджменту заказа есть доступ у ролей:
- Клиент
- Ворвер
- Менеджер услуг
К контролю качества:
- Менеджер отдела качества

| Характеристика | Источник                                                                                      | Комментарий                                              |
|:---------------|:----------------------------------------------------------------------------------------------|:---------------------------------------------------------|
| Deployability  | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц                            |
| Testability    | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц                            |
| Agility        | Требование низкого TTM                                                                        |                                                          |
| Evolvability   | Система работает в инновационном секторе и проверка гипотез с быстрой реакцией как требование |                                                          |
| Security       | В системе есть разграничение ролей                                                            | Клиент, воркер, менеджер услуг, менеджер отдела качества |
| Scalability    | Известна средняя штатная нагрузка                                                             | 10 заказов в минуту                                      |
| Usability      | Для менеджмента задач: требование от клиентов на удобную и стабильную работу                  |                                                          |
| Simplicity     | Для менеджмента задач: требование от клиентов на удобную и стабильную работу                  |                                                          |
| Fail tolerance | Для менеджмента задач: требование от клиентов на удобную и стабильную работу                  |                                                          |

**ADR:** To be filled

### Вынесение контроль качества
Характеристики контекста контроля качества и роли, имеющие к нему доступ, отличаются от тех, что у контекста управления
заказами. Кроме того, контроль качества отнесен к generic функциональности и работу над ним можно делегировать.

**Характеристики**

~Сервис управления заказами~

| Характеристика | Источник                                                                                      | Комментарий                     |
|:---------------|:----------------------------------------------------------------------------------------------|:--------------------------------|
| Deployability  | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц   |
| Testability    | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц   |
| Agility        | Требование низкого TTM                                                                        |                                 |
| Evolvability   | Система работает в инновационном секторе и проверка гипотез с быстрой реакцией как требование |                                 |
| Security       | В системе есть разграничение ролей                                                            | Клиент, воркер, менеджер услуг  |
| Scalability    | Известна средняя штатная нагрузка                                                             | 10 заказов в минуту             |
| Usability      | Требование от клиентов на удобную и стабильную работу                                         |                                 |
| Simplicity     | Требование от клиентов на удобную и стабильную работу                                         |                                 |
| Fail tolerance | Требование от клиентов на удобную и стабильную работу                                         |                                 |

~Сервис контроля качества~

| Характеристика | Источник                                                                                      | Комментарий                       |
|:---------------|:----------------------------------------------------------------------------------------------|:----------------------------------|
| Deployability  | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц     |
| Testability    | Требование низкого TTM                                                                        | *Релизный цикл модуля 1 месяц     |
| Agility        | Требование низкого TTM                                                                        |                                   |
| Evolvability   | Система работает в инновационном секторе и проверка гипотез с быстрой реакцией как требование |                                   |
| Security       | В системе есть разграничение ролей                                                            | Менеджер отдела контроля качества |

**ADR:** To be filled


### Разнести контексты оплат
Работы по приведению в порядок биллинга масштабные и провести их в один этап может быть рискованно, поэтому нужно 
отрефакторить имеющийся сервис так, чтобы можно было разнести расчеты на разные сервисы. На этом этапе разрываем связь 
сервиса со стримом воркеров. Модуль получает все необходимое при запросе завершенных задач.
Эти работы можно провести "на горячую".

**ADR:** To be filled

### Делегирование исполнения платежа
Для того, чтобы гарантировать соответствие финансовому комплаенсу и обеспечить секьюрность данных, эффективнее всего 
купить взаимодействие со внешней платежной системой. Такое изменение потребует смены схемы данных и не может делаться на 
горячую. Вариант с параллельным развитием старого и нового сервиса не рассматривается, слишком дорого выйдет поддержка 
двух сервисов и высока серьезность проблем при утере дублированной функциональности.

**ADR:** To be filled

### Потеряна функциональность мотивирования воркеров
Функциональность простая, реализовываем при момощи layered архитектуры, пользуется той же базой, что и биллинг.

**ADR:** To be filled

## Вопросы к бизнесу
[^1]: За какое время нужно восстанавлизваться после падений?