# Портфолио: аналитик данных

## Обо мне

Привет! Меня зовут Оксана, я начинающий аналитик данных. В этом репозитории вы можете найти некоторые из моих проектов, выполненных во время обучения и практики. 

## Навыки и технологии

- Инструменты анализа данных: SQL, Excel:
- Языки программирования и библиотеки: Python
- Системы управления базами данных: PostgreSQL
- Средства визуализации данных: PowerBi, Matplotlib, Superset

## Проекты

# Проект 1: Калькулятор юнит-экономики онлайн-школы

Что нужно было сделать:

Задача №1 Настроить в калькуляторе учет корректировок планов маркетинга.

Как решала(-а): 

1) Добавила в список параметров калькулятора показатель "Поправочный коэф-т на привлечение"	
2) Установила значение этого показателя по умолчанию как 100% и добавила возможность изменять этот показатель для расчётного периода 05.21-04.22 по аналогии с другими - через столбец "Изменение"	
3) Настроила динамический расчёт количества новых студентов за период 05.21-04.22 с поправкой на этот коэффициент. Если для 05.21-04.22 он равен 120%, то в каждый месяц этого периода рассчётное значение количества новых студентов должно быть больше на 20%	
4) Просчитала сценарий, при котором планы маркетинга будут увеличены на 12% (настройки остальных показателей не меняются), настроила связь с листом План Маркетинга через ВПР.	

Итог:	обновленный лист с калькулятором + новое расчётное количество студентов на 04.2022

Задача №2 Пересчитать план найма преподавателей.

Как решала(-а): 

1) Добавила в калькулятор найма преподавателей возможность изменять входные параметры: Пропускную способность П и Retention П - с помощью дополнительного столбца с процентными изменениями, как это сделано на Главном листе с основным калькулятором Юнит-экономики	
2) Сделала поправку в расчёте общей пропускной способности - изменила формулу так, чтобы отразить, что новые преподаватели в первый месяц своей работы могут проводить только половину уроков от средней пропускной способности преподавателя, задаваемой параметром	
3) Обновила прогнозное количество уроков, добавив новые значения из Задачи 1 (полученные после поправки на планы маркетинга)	
4) Просчитала сценарий, при котором Пропускная способность П увеличится на 15 процентов, а Retention П упадёт на 2 процента	
5) С помощью Поиска решений составила новый план найма с ограничением: за месяц нельзя нанять более 70 преподавателей	
	
Итог:	Обновлённый план по найму - с количеством новых преподавателей по месяцам за период с 05.2021 по 04.2022

# Проект 2: Калькулятор юнит-экономики онлайн-кинотеатра

Что нужно было сделать:

Задача №1 Определить динамику подписок, просмотров, первых просмотров юзера и количества уникальных юзеров на платформе

Как решала(-а): 
С помощью сводных таблиц рассчитала нужные метрики и проанализировала какая динамика пользовательской активности на платформе, опираясь на все рассчитанные метрики.

Итог: Большинство пользователей подписываются, а также совершают первые просмотры в период с мая по июнь				

Задача №2 Посчитать юнит-экономику продукта и предложить сценарий по настройке параметров для выхода на 25-процентную маржинальность

Как решала(-а): 
Построила калькулятор юнит-экономики, в котором автоматически пересчитываются определенные метрики. Затем предложила сократить постоянные расходы (например, убрав из аренды непопулярные фильмы, или пересмотрев мотивацию ЗП, уменьшить расходы САС, так как будем опираться больше на «старую» базу клиентов, которые у нас уже есть, но при этом будем также получать «новых» клиентов нашего кинотеатра, и повысить процент удержания клиентов при помощи увеличения скидок.

Итог: Построен калькулятор юнит-экономики, с помощью которого можно просчитать возможные варианты выхода на 25%-ную маржинальность.


# Проект 3: Когортный анализ онлайн-кинотеатра с помощью SQL

Что нужно было сделать: Постройте распределение выплаченных клиентами денег по месяцам и построить три разных скользящих средних (по сумме денег по месяцам)

Как решала(-а): 

select a.*,

       avg(amt_payment) over (order by date_month rows between 2 preceding and current row) as ma3,
       
       avg(amt_payment) over (order by date_month rows between 6 preceding and current row) as ma7,
       
       avg(amt_payment) over (order by date_month rows between 2 preceding and 2 following) as MA_2side,
       
       avg(amt_payment) over () as avg_all
       
from

(

select date_trunc('month', date_purchase) as date_month,

       sum(amt_payment) as amt_payment
       
from skycinema.client_sign_up

group by date_month 

) a

Итог: написан скрипт и построен график скользящего среднего

# Проект 4: Построение витрины для модели машинного обучения в банке

Что нужно было сделать: построить витрину для машинного обучения в банке.

Как решала(-а): 

select  t.*

       , amt_loan::float / sum_city::float as share_loan_city
       
       , amt_loan::float / sum_type::float as share_loan_type
       
       , amt_loan::float / sum_type_city::float as share_type_city
       
from

(select   id_client

        , name_city
        
        , (case when gender = 'M' then 1  
        
            when gender = 'F' then 0 end) as nflag_gender
            
        , age
        
        , first_time
        
        , (case when cellphone is null then 0 
        
            when cellphone is not null then 1 end) as nflag_cellphone
            
        , is_active
        
        , cl_segm
        
        , amt_loan
        
        , date_loan::date
        
        , credit_type
        
        , sum(amt_loan) over (partition by name_city) as sum_city
        
        , sum(amt_loan) over (partition by credit_type) as sum_type
        
        , sum(amt_loan) over (partition by credit_type, name_city) sum_type_city
        
        , count(amt_loan) over (partition by name_city) as cnt_city
        
        , count(amt_loan) over (partition by credit_type) as cnt_type
        
        , count(amt_loan) over (partition by credit_type, name_city) as cnt_type_city
        
from skybank.late_collection_clients as l

    join skybank.region_dict as r
    
        on l.id_city = r.id_city
        
) t

Итог: написан скрипт, который сделал витрину с заданными полями.

# Проект 5: Моделирование изменения балансов студентов

Что нужно было сделать: смоделировать изменение балансов студентов.

Задача №1 Узнать сколько всего уроков было на балансе всех учеников за каждый календарный день, как это количество менялось под влиянием транзакций (оплат, начислений, корректирующих списаний) и уроков (списаний с баланса по мере прохождения уроков).
И создать таблицу, где будут балансы каждого студента за каждый день.

Как решала(-а): 
- Узнаем, когда была первая транзакция для каждого студента.
- Соберем таблицу с датами за каждый календарный день 2016 года
- Узнаем, за какие даты имеет смысл собирать баланс для каждого студента
- Найдем все изменения балансов, связанные с успешными транзакциями
- Найдем баланс студентов, который сформирован только транзакциями
- Найдем изменения балансов из-за прохождения уроков
- Создадим CTE для хранения кумулятивной суммы количества пройденных уроков
- Создадим CTE balances с вычисленными балансами каждого студента
- Выберем топ-1000 строк из CTE `balances` с сортировкой по `user_id` и `dt`. Посмотрим на изменения балансов студентов.

Итог: запрос, который собирает данные о балансах студентов за каждый "прожитый" ими день.
Выводы: 
     У некоторых студентов наблюдается списание уроков без начисления, т.е. кредитовое списание уроков. Вопрос к дата-инженерам: возможно ли в нашем случае давать уроки без оплаты или это какой-то сбой в системе?
     У студента 3.773.716 с 18 августа и до конца года наблюдается нулевой баланс, происходят какие-то движения по датам, но нет ни начислений ни списаний. Вопрос к дата-инженерам: что-то случилось с базой? может не подгрузились какие-то данные?
 

Задача №2. Создайте визуализацию (линейную диаграмму) итогового результата. Какие выводы можно сделать из получившейся визуализации?

Как решала(-а): построена линейная диаграмма по вычесленным параметрам

Итог: На графике мы видим, что у нас очень большая разница между начисленными и списанными уроками, около 5 тысяч. Это говорит о том, что студенты не заинтересованы в прохождении оплаченных уроков, что может привести к заявлениям по возврату денежных средств или заморозке обучения, что негативно скажется на нашем финансовом состоянии. 
Необходимо чем-то замотивировать студентов, чтобы им стало интересно обучаться.

# Контактная информация

Email: oksanagri00@gmail.com
