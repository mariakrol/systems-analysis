# Systems Analysis. Home works of the educational course Systems Analysis

## Домашнее задание

- [x] Определить стейкхолтеров;
- [x] Выбрать архитектурный стиль;
- [x] Аргументировать выбор;
  - [x] Только для распределенных: описать сервисы; 
- [x] Выбрать базу (-ы);
- [x] Выберите стиль коммуникаций и их вид (синхронный/асинхронный);
- [x] Аргументировать выбор коммуникаций;
- [x] Выбрать фитнес-функции для валидации итоговой системы;
- [ ] Сделать ADR, описать принятие решения по изоляции одного из элементов как изолированного сервиса.

## Консёрны стейкхолдеров по группам
### Топ-менеджмент
Cкоринг потенциальных работников уникален в своём роде, и логика его работы сильно выше, чем планировалось. Бизнес в будущем хочет продавать его другим компаниям и тестировать больше гипотез;
релизный цикл для всей системы — месяц, для скоринга работников — неделя максимум.
### Менеджеры
Хотят, чтобы о системе ставок не знали другие отделы, иначе будет некрасивая ситуация. Они хотят скрыть эту систему даже от разработчиков, которые не будут ей заниматься, и от начальства;
Выяснилось, что котам из Happy Cat Box наш проект понравился, поэтому приходит не 10 заказов в день, а 10 заказов в минуту.
### Финотдел
списывать деньги с клиентов каждую неделю слишком затратно для отдела, поэтому они хотят списывать деньги раз в месяц, но платить воркерам и дальше раз в неделю. При этом необходимо постоянно добавлять новые способы списания денег для клиентов. Воркеры всегда работают через компанию «Золотая шляпа»;
боятся потерять любую финансовую информацию и хотят решение, которое будет гарантировать, что всё будет ок.
### Разработчики
система должна работать без сбоев, а если сбой случается, то должно быть понятно, что и где чинить.
### Админы
простота мониторинга системы для своевременного замечания сбоев, чтобы не работать в авральном режиме.
### Юристы
соответствие всей системы правовым нормам.
### Клиенты
ожидаемое поведение системы: без сбоев и тупняков.
### Ограничения
инфраструктуру считаем бесплатной, прямо как в уроке, так как Happy Cat Box расскажет нам, как такое организовать;
соблюдение CatFinComplience, который говорит об особом способе хранения данных и особой наблюдаемости за системой. Компания не хочет повторять опыт с маски-шоу, которые были в Happy Cat Box.

## Доска Miro
[Ссылка на доску] (https://miro.com/app/board/uXjVMDmE-HA=/?share_link_id=169410494033)

## Список стейкхолдеров
В дополнение к основному списку стейкхолдеров выделены разработчики системы мотивации менеджеров (ставки), т.к. подпроект полусекретный.

| Стейкхолдер                       | Степень влияния                                                                                                                                    | Уровень интереса                                                                                                                             |
|:----------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|
| Топ-менеджмент                    | Высокая                                                                                                                                            | Высокий                                                                                                                                      |
| HR-менеджеры                      | Ниже среднего, они контролируют только найм                                                                                                        | Ниже среднего. У них узкая специализация                                                                                                     |
| Дата-саентисты                    | Выше среднего, их гипотезы направляют главный бизнес-продукт                                                                                       | Выше среднего, т.к. для создания алгоритма необходимо полное погружение в специфику доменмена                                                |
| Разработчики системы ставок       | Низкая. Занимаются узкой фичей                                                                                                                     | Низкий. Занимаются узкой фичей                                                                                                               |
| Остальные разработчики            | ?                                                                                                                                                  | ?                                                                                                                                            |
| Работники склада                  | Ниже среднего. Могут дать информацию по особенностям хранения и процесса подбора расходников, но не могут кардинально повлиять на систему          | Выше среднего, т.к. являются непосредственными участниками процесса, от которых так же зависит успех заказа                                  |
| Исполнители заказов               | Выше среднего, от их работы зависит успешность бизнеса                                                                                             | Высокая, т.к. система направлена на доставку их услуги пользователю                                                                          |
| Бухгалтеры                        | Ниже среднего. Влияют только на финансовые проводки, которые, по плану, будут совершаться через payment provider, отвечающего законным требованиям | Высокий. Судя по апдейтам, большое количество информации будет проходить через бухгалтерию                                                   |
| Менеджеры по обеспечению качества | Выше среднего. Могут предоставить информацию, кардинально меняющую представление об исполнении заказа                                              | Высокий. Генерят гипотезы, влияющие на основной продукт                                                                                      |
| Менеджеры по услугам              | Ниже среднего. Только контролируют набор услуг компании                                                                                            | Средний. Узкая специальность                                                                                                                 |
| Юристы                            | ?                                                                                                                                                  | ?                                                                                                                                            |
| Админы                            | Высокая. Есть требование на TTM                                                                                                                    | Низкий. Интересуются только доставкой, но не созданием системы                                                                               |
| Клиенты                           | Ниже среднего. Разработка идет не под заказчика                                                                                                    | Низкий. Чаще всего, конечный пользователь идет на контакт только в случаях проблем, при штатной работе вводных от рядового пользователя мало |
| Техподдержка                      | Выше среднего. Могут дать информацию непосредственно от клиента                                                                                    | Ниже среднего                                                                                                                                |

## Консерны к разработке

| ID  | Источник              | Консерн                                   |
|:----|:----------------------|:------------------------------------------|
| 1   | Топ-менеджмент        | Хорошая изоляция скоринга                 |
| 2   | Топ-менеджмент        | Релизный цикл системы 1 месяц             |
| 3   | Топ-менеджмент        | Релизный цикл скоринга <= 1 недели        |
| 4   | Менеджмент            | Скрыть ставки от коллег                   |
| 5   | Менеджмент            | В систему приходит 10 заказов в минуту    |
| 6   | Разработчики, клиенты | Минимизировать сбои                       |
| 7   | Разработчики          | Упростить отладку                         |
| 8   | Админы                | Мониторинг сбоев                          |
| 9   | Юристы                | Соответствие закону                       |
| 10  | Бухгалтеры            | Списывать деньги 1р. в месяц              |
| 11  | Бухгалтеры            | Исполнители всегда через шляпу            |
| 12  | Бухгалтеры            | Платить исполнителям 1 р. в неделю        |
| 13  | Бухгалтеры            | Расширяемость способов оплат для клиентов |
| 14  | Бухгалтеры            | Сохранность фин. инфы                     |
| 15  | Клиент                | Работает быстро                           |

## Ограничения MCF

| Проблема                                                         | Цена упущения                                                                                                                    |
|:-----------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------|
| Только 3% всех котов могут подойти в исполнители                 | Есть предел наращивания объемов исполняемых заказов. Если не учесть, ожидание исполнения возрастет сильно                        |
| Финансовый комплаенс                                             | Штрафы и уголовные дела                                                                                                          |
| Есть агрессивные конкуренты                                      | DDOS атаки с кражей данных и долговременной недоступностью системы                                                               |
| Бюджет описан как "не критичный"                                 | Когда нет четких вилок, не ясно, сколько все-таки доступно и можно выйти за бюджет, что повлечет перепроектирование              |
| Высокая сложность алгоритма матчинга                             | Нужно достаточное количество опытных девелоперов                                                                                 |
| Поддержка нескольких платежных систем                            | Будут уходить клиенты, которые не пользуются поддерживаемой                                                                      |
| Матчинг должен быть отдельным подпродуктом                       | Бизнес потеряет деньги и даст пинка                                                                                              |
| Система хранит много личных данных клиентов и исполнителей       | Если утечет, будет много судов и затрат и отсутствие репутации                                                                   |
| Печенье едет 10 минут                                            | Заказ не может быть начат ранее чем 10 мин. + время добраться                                                                    |
| Есть "секретная" функциональность ставок                         | Если узнает команда, от которой скрывается, будет скандал и кто-то может уйти или опубликовать компрометирующую информацию       |
| Нужен централизованный способ нотифицировать участников процесса | Нотификация д.б. на каждое действие, получают нотификации почти все. Если размажется по системе, будет сложно поддерживать       |
| Ресурсы пекарни                                                  | В идеале не очень страшно, пользователи должны возвращаться не из-за печеньки, а благодаря качеству услуги, но осадочек оставит  |

## Обновленные характеристики

| Поддомен                              | Характеристика  | Тип     | Значение                                                                                                    | Коммент                                                                                                                                                                                                 |
|:--------------------------------------|:----------------|:--------|:------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Найм лучших исполнителей              | Scalability     | Явная   | 1000 заявок в день                                                                                          | Обрабатывать заявки потребуется без задержет т.к. из-за потока заказов нужно много исполнителей                                                                                                         |
| Найм лучших исполнителей              | Securability    | Явная   |                                                                                                             | Разграничение ролей: доступ к поддомену у соискателя и HR менеджеров. Поддомен имеет доступ к личным sensitive данным                                                                                   |
| Найм лучших исполнителей              | Elasticity      | Явная   | 10 000 заявок в день [^1]                                                                                   | Известно, что вероятен DDOS                                                                                                                                                                             |
| Найм лучших исполнителей              | Testability     | Неявное | Релизный цикл 1 месяц                                                                                       | Оговаривается для системы в целом, поддомен не входит в исключения                                                                                                                                      |
| Найм лучших исполнителей              | Modifability    | Неявное | Релизный цикл 1 месяц                                                                                       |                                                                                                                                                                                                         |
| Найм лучших исполнителей              | Deployability   | Неявное | Релизный цикл 1 месяц                                                                                       |                                                                                                                                                                                                         |
| Подбор исполнителя по характеристикам | Portability     | Явная   | Поставляется как самостоятельный продукт                                                                    | Явное требование от топ-менеджеров                                                                                                                                                                      |
| Подбор исполнителя по характеристикам | Scalability     | Неявная | 30 запросов в минуту                                                                                        | Есть информация по количеству заказов, подбор нужен к каждлому. Т.к. уже есть 10 заказов в минуту в нормальном режиме, нагрузка может пиково возрастать [^2]                                            |
| Подбор исполнителя по характеристикам | Testability     | Явная   | Релизный цикл 1 неделя                                                                                      | Явно из требований                                                                                                                                                                                      |
| Подбор исполнителя по характеристикам | Modifability    | Явная   | Релизный цикл 1 неделя                                                                                      |                                                                                                                                                                                                         |
| Подбор исполнителя по характеристикам | Deployability   | Явная   | Релизный цикл 1 неделя                                                                                      |                                                                                                                                                                                                         |
| Подбор исполнителя по характеристикам | Securability    | Явная   | Сервис закрыт аутентификацией                                                                               | Продается как отдельный продукт                                                                                                                                                                         |
| Ведение заказов на услуги             | Testability     | Неявное | Релизный цикл 1 месяц                                                                                       | Оговаривается для системы в целом, поддомен не входит в исключения                                                                                                                                      |
| Ведение заказов на услуги             | Modifability    | Неявное | Релизный цикл 1 месяц                                                                                       |                                                                                                                                                                                                         |
| Ведение заказов на услуги             | Deployability   | Неявное | Релизный цикл 1 месяц                                                                                       |                                                                                                                                                                                                         |
| Ведение заказов на услуги             | Usability       | Явное   |                                                                                                             | Оговаривается пользователем как "без глюков", но часто непродуманная функциональность может быть не понятна пользователю и восприниматься как ошибочная                                                 |
| Ведение заказов на услуги             | Simplicity      | Явное   |                                                                                                             | Оговаривается пользователем как "без глюков", но часто непродуманная функциональность может быть не понятна пользователю и восприниматься как ошибочная                                                 |
| Ведение заказов на услуги             | Securability    | Явная   |                                                                                                             | Разграничение ролей: Клиент, Менеджер услуг, исполнитель, бухгалтер, QA-менеджер. У поддомена доступ к личным данным и финансовой информации                                                            |
| Ведение заказов на услуги             | Concistency     | Явная   | Клиенты видят все свои заказы и затраты, исполнители видят заработок, бухгалтеры не теряют истории проводок | Потеря финансовых данных влечет проблемы с законом и с доверием клиентов                                                                                                                                |
| Обеспечение исполнителя расходниками  | Simplicity      | Неявная |                                                                                                             | Склад должен быть максимально удобен для сбора расходников, т.к. заказов много                                                                                                                          |
| Обеспечение исполнителя расходниками  | Securability    | Явная   |                                                                                                             | Роли: сотрудник склада, исполнитель. Доступ к данным заказа и личным данным                                                                                                                             |
| Обеспечение исполнителя расходниками  | Performance     | Неявная | > 7000 сборок в день                                                                                        | Не оговорено прямо, рассчитываю примерно из количества заказов в минуту [^3]                                                                                                                            |
| Исполнение заказов                    | Performance     | Неявная | > 7000 сборок в день                                                                                        |                                                                                                                                                                                                         |
| Исполнение заказов                    | Simplicity      | Неявная |                                                                                                             | Исполнитель должен четко понимать, что нужно сделать, чтобы выполнить заказ                                                                                                                             |
| Исполнение расчитанных платежей       | Modifiability   | Неявная |                                                                                                             | Т.к. проводки идут через шляпу и payment provider, мы должны легко поддержать внешние изменения в случает чего. При этом изменения во внешней среде не должны влиять ни на что, кроме данного поддомена |
| Исполнение расчитанных платежей       | Securability    | Явная   |                                                                                                             | Роли: бухгалтер, у поддомена доступ к фин. инфе                                                                                                                                                         |
| Повышение лояльности клиентов         | Securability    | Явная   |                                                                                                             | Роли: менеджеры заказов, работники склада                                                                                                                                                               |
| Повышение лояльности клиентов         | Performance     | Неявная | > 7000 сборок в день                                                                                        |                                                                                                                                                                                                         |
| Повышение мотивации исполнителей      | Securability    | Явная   |                                                                                                             | Роли: QA-менеджеры, менеджеры заказов                                                                                                                                                                   |
| Повышение мотивации менеджеров        | Securability    | Явная   | Роли: менеджеры, разработчики ставок                                                                        |                                                                                                                                                                                                         |

## Выделение сервисов
**Исходные bounded contexts**
- [x] Тестирование  (поддомен найма лучших исполнителей); 
- [x] Ставки (поддомен мотивации менеджеров); 
- [x] Премирование (поддомен мотивации исполнителей); 
- [x] Матчинг (поддомен подбора исполнителя); 
- [x] Расчет стоимости (поддомен подбора исполнителя); 
- [x] Сборка расходников (поддомен обеспечение расходными материалами); 
- [x] Заказ печенья (поддомен лояльность); 
- [x] Оказание помощи (поддомен ведение заказов); 
- [x] Управление типами услуг (поддомен ведение заказов); 
- [x] Проверка качества (поддомен ведение заказов); 
- [x] Расчет зарплаты исполнителя (поддомен ведение заказов); 
- [x] Расчет затрат клиента (поддомен ведение заказов); 
- [x] Расчет скидки клиента (поддомен ведение заказов);
- [x] Проведение платежа от клиента (поддомен исполнения рассчитанных платежей);
- [x] Проведение платежа исполнителю (поддомен исполнения рассчитанных платежей);
- [x] Создание заказа (поддомен ведение заказов); 
- [x] Пересоздание заказа (поддомен ведение заказов);
- [x] Пересоздание заказа (поддомен ведение заказов);
- [x] Подготовка заказа к исполнению (поддомен ведение заказов);
- [x] Закрытие заказа (поддомен ведение заказов);
- [x] Исполнение заказа (поддомен исполнения заказа);

Расположила контексты так, чтобы рассматривать, начиная с самых очевидных. 

**Тестирование кандидатов - не зависит ни от чего в MCF**
Проблема подбора исполнителей одна из важнейших в MCF, однако она самодостаточная и не зависит ни от чего.

*Выделенные сервисы*
- Тестирование  (поддомен найма лучших исполнителей);

**Ставки - элемент секретности**
Т.к. наличие ставок может негативно повлиять на погоду в команде, данный сервис должен быть хорошо изолирован и скрыт от 
тех, кто к нему не имеет непосредственного отношения.

*Выделенные сервисы*
- Ставки (поддомен мотивации менеджеров);

**Премирование - больше пряников**
На данный момент премирование, хоть и выделено, по сути, в отдельный поддомен (мотивирование исполнителей), является 
простейшим по функциональности. С одной стороны, держать целый сервис, который нужно поддерживать и разворачивать может
быть затратно, но в будущем, если появятся другие варианты мотивирования, премирование не будет жестко связано с другими 
сервисами и сможет быть заменено или изменено.

*Выделенные сервисы*
- Премирование (поддомен мотивации исполнителей);

**Матчинг - как продукт**
Я выделила поддомен подбора исполнителя, т.к. для MCF это ключевой вопрос. В поддомене 2 отдельных контекста - матчинг 
по параметрам и расчет стоимости. 
Алгоритм матчинга будет продаваться как самостоятельный продукт и он должен быть четко отделен. Релизный цикл так же 
отличается (1 неделя против 1 месяца для остальной системы). Расчет стоимости - проблема, касающаяся MCF и к алгоритму 
имеющая опосредованное значение. Внешним потребителям матчинга расчет не нужен. Объединение контекстов из разных 
поддоменов в один сервис может вызвать проблему с модификацией и поддержкой, поэтому принимаю решение сделать расчет 
отдельным сервисом.

*Выделенные сервисы*
- Матчинг (поддомен подбора исполнителя);
- Расчет стоимости (поддомен подбора исполнителя);

**Склад - независимый отдел компании** 
Склад определен мною как неделимый контекст, хотя он тесно связан с заказом печенья, которое, по сути, является
расходником, объединять не хочу. Это контексты разных поддоменов и связывать их жестко выглядит не логичным и может 
привести к проблемам с изменением программ лояльности и комплектации.

*Выделенные сервисы*
- Сборка расходников (поддомен обеспечение расходными материалами);
- Заказ печенья (поддомен лояльность);

**Оказание помощи - нет прямого влияния на флоу заказа**
С одной стороны, оказание помощи непосредственно связано с созданным заказом, но участники этого процесса - специалисты 
техподдержки, которым не нужен доступ к жизненному циклу заказа.

*Выделенные сервисы*
- Оказание помощи (поддомен ведение заказов);

**Управление типами услуг - не касается клиента и исполнителя**
Управление типами услуг - задача, которой занимается менеджер и на нее не влияют действия клиента и исполнителя.

*Выделенные сервисы*
- Управление типами услуг (поддомен ведение заказов);

**Проверка качества - серый кардинал в стороне**
Проверка качества не влияет на заказы и на процесс исполнения. Это только механизм для стороннего наблюдателя, позволяющий
сгенерировать гипотезу по изменению процесса. Но данные гипотезы не влияют на поведение системы в моменте.

*Выделенные сервисы*
- Проверка качества (поддомен ведение заказов);

**Расчет зарплаты - только потребляет информацию, не влияя на процесс**
При расчете заработной платы исполнителей учитываются финансовые результаты заказов (начисление или штраф) и прремии. 
При этом нам необходимо отображать историю, которая хранится консистентно и не меняется. 
Т.к. до того, как заказ завершен, он может изменяться и фактический результат не известен, все расчеты должны быть отделены 
и исполняться там где изменений уже нет.

*Выделенные сервисы*
- Расчет зарплаты исполнителя (поддомен ведение заказов);

**Затраты - больше тратишь - меньше платишь**
Т.к. затраты клиента влияют на его скидку, решила объединить в один сервис. Одно без другого не существует.

*Выделенные сервисы*
- Расчет затрат и скидок:
  - Расчет затрат клиента (поддомен ведение заказов);
  - Расчет скидки клиента (поддомен ведение заказов);

**Биллинг - все по закону**
Процессинг и хранение финансовой информации - сложный и тесно сопряженной с законом процесс. Информация не должна
искажаться и стать доступной третьим лицам. Поэтому финансовые проводки, по плану, будут делегированы payment processor-у.
При этом, предварительные суммы уже будут расчитаны ранее.
Т.к. и для затрат клиента и для зарплат исполнителей не будет дополнительной логики в данном месте, а лишь перенаправление
информации в стороннюю систему, хочу объединить 2 контекста в сервис.
Выносить общение с внешней системой в уже выделенные сервисы не хочу. Если будет принято решение менять провайдера или 
писать свой, будет проще модифицировать отдельно от остальной системы.

*Выделенные сервисы*
- Биллинг
  - Проведение платежа от клиента (поддомен исполнения рассчитанных платежей);
  - Проведение платежа исполнителю (поддомен исполнения рассчитанных платежей);

**Жизненый цикл - не делить**
Каждая задача проходит свой жизненный цикл - от создания до завершения (успешного или нет) и это единый процесс, этапы
которого следуют по порядку и вытекают друг из друга. Если разделить его, то получится, что сервисы, в любом случае,
тесно связаны и невозможно будет изменять один, не затронув другой.

*Выделенные сервисы*
- Менеджмент заказа
  - Создание заказа (поддомен ведение заказов);
  - Пересоздание заказа (поддомен ведение заказов);
  - Подготовка заказа к исполнению (поддомен ведение заказов);
  - Закрытие заказа (поддомен ведение заказов);

**Исполнение заказа - темная зона**
Сам процесс исполнения заказа происходит во внешней среде и MCF на это не может повлиять. В системе заказ будет отмечен 
как завершенный с использованием сервиса, отвечающего за жизненный цикл.

## Стили реализации сервисов

**Сервис тестирования и оценки кандидатов**
Т.к. процесс тестирования не атомарный и включает в себя подбор тестов, оценку результатов и беседу, хочется иметь
управляемую структуру. При этом, т.к. компания обещает улучшать уровень нанимаемых сотрудников и тщательно фильтровать, 
могут появиться дополнительные этапы отбора. При этом кандидат может отвалиться на любом из этапов, поэтому процесс
должен контролироваться из единой точки.
Выбираю microkernel

**Сервис мотивирования исполнителей**
Нет требований, указывающих на сложные процессы внутри. Подойдет layered

**Сервис расчета заработков**
Нет требований, указывающих на сложные процессы внутри. Подойдет layered

**Сервис расчета стоимости заказа**
Нет требований, указывающих на сложные процессы внутри. Подойдет layered

**Сервис подбора по характеристикам**
В изначальных требованиях звучит то, что матчинг состоит из шагов, которые можно менять, пересортировывать и настраивать.
Это четко отображается стилем pipeline 

**Сервис менеджмента заказа**
Т.к. менеджмент заказа - довольно нагруженная часть, моделирующая жизненный цикл, этапы в котором связаны с происходящими 
событиями, сервис хорошо ложится на event driven с топологией broker.

**Сервис расчета затрат клиента**
Нет требований, указывающих на сложные процессы внутри. Подойдет layered   

**Сервис оценки качества**
Хорошо ложится на pipeline, т.к. у заявки строго определенный жизненный цикл. При желании можно легко внедрить
дополнительные этапы оценки.

**Сервис управления типами услуг**
Нет требований, указывающих на сложные процессы внутри. Подойдет layered 

**Сервис Help Desk**
Хорошо ложится на pipeline, т.к. у заявки строго определенный жизненный цикл. При желании можно легко внедрить
дополнительные этапы контроля.

**Сервис ставок на заказы**
Нет требований, указывающих на сложные процессы внутри. Подойдет layered

## Выбор баз данных
**Сервис тестирования и оценки кандидатов**
Хранит тесты, характеристики, заявки и отбракованные образцы. Данные не вязаны и не обладают единой структурой.
Подойдет документоориентированная база.

**Сервис расчета заработков**
Т.к. необходимо хранить историю и консистентно предоставлять данные к просмотру и на исполнение платежей, база должна
быть отдельной.
Т.к. данные структурированы, выбираю реляционную.

**Сервис мотивирования исполнителей**
Не имеет отдельной базы. Использует базу сервиса расчетов зп

**Сервис расчета затрат клиента**
Т.к. необходимо хранить историю и консистентно предоставлять данные к просмотру и на исполнение платежей, база должна
быть отдельной.
Т.к. данные структурированы, выбираю реляционную.

**Сервис расчета стоимости заказа**
Не нуждается в отдельной базе. Будет получать все необходимое для расчета с запросом.

**Сервис подбора по характеристикам**
Т.к. основа работы алгоритма - характеристики, которые должны мапиться, подойдет графовая база.

**Сервис менеджмента заказа**
Связывает большую часть сущностей системы. Подойдет реляционная база

**Сервис оценки качества**
Результат оценки качества - отчет и набор гипотез. Подойдет документоориентированная.

**Сервис управления типами услуг**
Данные простые и структурированные. Подойдет документоориентированная

**Сервис Help Desk**
Связей и сложной структуры нет. Хранит заявку и результат. Подойдет документоориентированная

**Сервис ставок на заказы**
Хранит связку заказа и ставки, подойдет key-value

## Выбор стиля коммуникаций
В основном система работает через асинхронные не блокирующие вызовы. Единственное место, где идет синхронный запрос - 
подбор исполнителя заказа.
В данном случае задача не может идти по циклу, пока не будет назначен исполнитель. 

## Выбор фитнесс функций
**Сервис тестирования и оценки кандидатов**
- Выдерживает регулярную нагрузку в 1000 запросов (нагрузочные тесты)
- Выдерживает кратковременную пиковую нагрузку (DDOS) ~10 000 (нагрузочные тесты)
- Релизится раз в месяц (Обсудить, как отследить релиз)

**Сервис расчета заработков**
- Данные неизменяемы (автоматические тесты)
- Релизится раз в месяц (Обсудить, как отследить релиз)

**Сервис мотивирования исполнителей**
- Релизится раз в месяц (Обсудить, как отследить релиз)

**Сервис расчета затрат клиента**
- Данные неизменяемы (автоматические тесты)
- Релизится раз в месяц (Обсудить, как отследить релиз)

**Сервис расчета стоимости заказа**
- Релизится раз в месяц (Обсудить, как отследить релиз)

**Сервис подбора по характеристикам**
- Релизится раз в неделю (Обсудить, как отследить релиз)
- Изолирован (обсудить, как отследить)
- Выдерживает стандартную нагрузку (определить с менеджментом величину, проверять нагрузочными тестами)
- Выдерживает пиковую нагрузку
- Причины падения определяются не дольше чем X (Обсудить с менеджментом, мерить по issue tracker)
- Анализ coupling and cohesion (Обсудить с разработчиками)

**Сервис менеджмента заказа**
- Релизится раз в месяц (Обсудить, как отследить релиз)
- Пользователь может рабобраться с функциональностью за фиксированное время (обсудить за какое, замерять google analytics)
- Выдерживает стандартную нагрузку в >7000 заказов в день (уточнить величину, оценивается нагрузочными тестами)

**Сервис оценки качества**
- Релизится раз в месяц (Обсудить, как отследить релиз)
- ?

**Сервис управления типами услуг**
- Релизится раз в месяц (Обсудить, как отследить релиз)

**Сервис Help Desk**
- Релизится раз в месяц (Обсудить, как отследить релиз)
- Запрос обрабатывается не дольше, чем за фиксированное время (обсудить, за какое)

**Сервис ставок на заказы**
- Исходный код в отдельном репозитории
- Ни кто из разработчиков основной системы не работает над сервисом
- У всех разработчиков подписан договор о неразглашении
- У менеджеров подписан договор о неразглашении

## Комментарии
[^1]: Известно, что вероятность атак высокая, но не уточняется количественно. 10 000 приблизительное предположеине, 
не имеющее под собой подтверждений
[^2]: Т.к. известно среднее штатное число заказов, нужно продумать, что в пиковые часы число может быть больше. 
30 не имеет под собой четкого объяснения, беру просто в 3 раза больше текущего среднего
[^3]: Т.к. оговаривается, что в минуту система получает 10 заказов, то, если ~понадеяться~ предположить, что коты спят 
ночью и делают заказы днем в течении, положим, 12 часов (что, безусловно, дичь), 
то за день появляется `12 * 60 * 10 = 7200` заказов. Т.о. чтобы беклог задач не разрастался, в день должно обработаться
как минимум это количетво заказов, если в какие-то дни заказы не исполняются (выходные, праздники), то больше.