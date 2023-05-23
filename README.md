# Systems Analysis. Homeworks of the educational course Systems Analysis
# Week 2. Strategic business analysis and architectural styles

## Домашнее задание

- [x] Выделить поддомены;
- [x] Определить все типы поддоменов и заполнить core domain chart;
- [x] Определить боундед-контексты для каждого из поддоменов, основываясь на требованиях;
- [x] Сравнить полученные боундед-контексты из поддоменов и боундед-контексты, полученные из ES. 
Описать, что разошлось (можно показать на картинке в сравнении) и предположить, почему так получилось; 
- [x] Сделать исправленную версию ES-модели и модели данных, если боундед-контексты разошлись; 
- [x] Если есть места, где бизнес-команда разбилась на технические шаги, — отметить эти места на модели; 
- [x] Выписать характеристики, важные для проекта; 
- [x] Для каждой найденной характеристики указать место, где она была взята;
- [x] Выбрать архитектурный стиль. Описать, почему сделан такой выбор и по каким характеристикам сравнивались стили;
- [ ] Сделать итоговую модель системы, указать виды коммуникаций между элементами, если выбрали распределённый стиль.

### Описание поддоменов

| Поддомен                                      | Конкурентное преимущество | Сложность | Изменчивость | Варианты реализации        | Интерес проблемы | Вид поддомена |
|:----------------------------------------------|:--------------------------|:----------|:-------------|----------------------------|:-----------------|:--------------| 
| Подбор worker-ов к заказу и рассчет стоимости | Да                        | Высокая   | Частая       | Самостоятельная реализация | Высокий          | Core          |
| Создание и управление заказами                | Нет                       | Высокая   | Редкая       | Самостоятельная реализация | Высокий          | Supporting    |
| Подбор расходников к заказу                   | Нет                       | Низкая    | Редкая       | Купить готовое             | Низкий           | Generic       |
| Тестирование и найм worker-ов                 | Да                        | Высокая   | Частая       | Самостоятельная реализация | Высокий? [^1]    | Core          |
| Оказание помощи клиентам                      | Нет                       | Низкая    | Редкая       | ???                        | Низкий           | Generic       |
| Ставки                                        | Нет                       | Низкая    | Редкая       | Самостоятельная реализация | Низкий           | Generic       |
| Проверка качества выполнения заказов          | Нет                       | Низкая    | Редкая       | ???                        | Низкий           | Generic       |
| Биллинг                                       | Нет                       | Высокая   | Редкая       | Купить готовое             | Высокий          | Generic       |

### Core domain chart
[Доска Miro](https://miro.com/app/board/uXjVMGyh5Ig=/)

Комментарии:
* Не ясна сложность контекста найма [^1];
* Обдумать, покупать help desk as a service или нет? Или аутсорс? 
* Обдумать, аутсорсить качество или писать самим?

### Выделение боундед-контекстов
| Поддомен                                      | Вид поддомена | Bounded context             | Subdomain                                                                                                         |
|:----------------------------------------------|:--------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------|
| Подбор worker-ов к заказу и рассчет стоимости | Core          | Матчинг                     | <ul><li>Матчинг worker-а</li><li>Рассчет стоимости зазаза</li></ul>                                               |
| Создание и управление заказами                | Supporting    | Менеджмент заказов и оплаты | <ul><li>Создание и пересоздание проваленного заказа</li><li>Подготовка заказа</li><li>Исполнение заказа</li></ul> |
| Подбор расходников к заказу                   | Generic       | Склад                       | <ul><li>Сборка расходников</li><li>Взаимодействие с печеньем</li></ul>                                            |
| Тестирование и найм worker-ов                 | Core          | Найм                        | <ul><li>Найм</li></ul>                                                                                            |
| Оказание помощи клиентам                      | Generic       | Help desk                   | <ul><li>Help desk</li></ul>                                                                                       |
| Ставки                                        | Generic       | Ставки                      | <ul><li>Ставки</li></ul>                                                                                          |
| Проверка качества выполнения заказов          | Generic       | Качество                    | <ul><li>Качество заказов</li><li>Премирование</li></ul>                                                           |
| Биллинг                                       | Generic       | Биллинг                     | <ul><li>Рассчет затрат клиентов и инвойсы</li><li>Расчет зарплат worker-ов и инвойсы</li></ul>                    |

### Сравнение полученных контекстов
#### Совпавшие контексты
* Склад (Контекст был выделен, но на текущем этапе есть желание разбить его на 2 сабдомена: сам склад и взаимодействие с печеньем)
* Help Desk
* Матчинг worker-ов на услуги (Контекст был выделен, есть желание разбить на сабдомены: подбор и расчет)
* Найм
* Биллинг (В ES было ветвление на процессинг клиентских финансов и финансов worker-ов, так и оставляю)
* Ставки
* QA

#### Разошедшиеся контексты
| Работа #1                                                                                                                                         | Текущее состоянии                                                                                                                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Отдельные контексты: <br/><ul><li>Создание нового заказа;</li><li>Управление типами услуг;</li><li>MCF Main;</li><li>Исполнение заказа;</li></ul> | Единый контекст подготовки и исполнения заказа, разбитый на поддомены:<br/><ul><li>Создание и пересоздание заказа;</li><li>Подготовка заказа;</li><li>Исполнение заказа;</li><li>Управление типами услуг;</li></ul> |
 
### Характеристики проекта
#### Требования к системе в целом
* `Agility`
  Источник: Требование низкого TTM
* `Testability`
  Источник: Требование низкого TTM 
* `Deployability`
  Источник: Требование низкого TTM
* `Securability`                  
  * Роли:                         
    * Менеджер обычный            
    * Менеджер отдела качества    
    * Worker                      
    * Клиент                      
    * Работник склада             

#### MCF main                                                 
* `Consistency`                 
  Источник: `US-250`, `US-260` 
* `Usability`
  Источник: У сервиса есть конкуренты
* `Simplicity`
  Источник: У сервиса есть конкуренты 

#### Требования к модулю матчинга
* `Modifability`                                             
  Источник: уникальность алгоритма матчинга  
* `Extensibility`                                            
  Источник: уникальность алгоритма матчинга
* `Supportability`
  Источник: уникальность алгоритма матчинга
* `Deployability`                                    
  Источник: через подбор worker проходят все заказы  

#### Модуль найма worker
* `Fault tolerance`
  Источник: `[US-081]`         
  * ddos на заявках в воркеры  
* `Maintainability`            
  Источник: `[US-081]`         
  * ddos на заявках в воркеры  
* `Scalability`
  Источник: `[US-081]` 
  * 1000 заявок в воркеры в день
* `Elasticity`
  Источник: `[US-081]`           
  * ddos на заявках в воркеры
* `Modifability`
  Источник: уникальность поиска worker
* `Extensibility`                                            
  Источник: уникальность поиска 

#### Подбор расходников к заказу
`Configurability`                            
  Источник: т.к. работа с внешней системой   
`Extensibility`                              
  Источник: т.к. работа с внешней системой         
`Maintainability`                            
  Источник: т.к. работа с внешней системой

#### Биллинг                             
`Configurability`
  Источник: т.к. работа с внешней системой
`Extensibility`
  Источник: т.к. работа с внешней системой 
`Maintainability`
  Источник: т.к. работа с внешней системой 

### Выбор архитектуры
По выделенным характеристикам микросервисы и service-based. Система создается как внутренний продукт с планом 
выйти на внешний рынок, на котором уже есть конкуренты. Т.о. если бюджет позволяет вложиться в микросервисную архитектуру,
стоит это сделать. Если бюджет маленький, подойдет service-based.
Отталкиваюсь от того, что бюджета хватит и выбираю микросервисы.

### Вопросы к бизнесу

[^1]: Т.к. в матчинге используются характеристики worker, полученные в процессе найма, то алгоритм выявления их может быть
сопоставимым по сложности с матчингом, т.о. необходим ответ, какой процесс выявления характеристик и анализа тестов.