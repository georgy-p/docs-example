# Чеклист для код-ревью разработанных DAG и dbt-моделей
Используйте чеклист для проведения код-ревью задач по разработке DAG и dbt-моделей. Чеклист помогает убедиться, что они соответствуют нашим стандартам и готовы к внедрению на продуктивный контур.

## Общие требования
1. Проверьте файл с результатами тестов:
   - Атрибуты проверочных функций должны быть заполнены корректно.
   - Target-таблицы в `table_name` не дублируются.
   - Результаты тестов выдают значение 0.
2. Проверьте файл с парсингом dbt-моделей, чтобы там не было ошибок. Подробнее см. [Парсинг dbt-моделей](./cheklist).
3. Если вы изменяли название объекта или перезагружали данные, проверьте скрипты и зависимые объекты.
4. Проверьте, что количество объектов merge request совпадает с требованиями в задаче.
5. Убедитесь, что все файлы из списка [Регламента сборки релиза](./cheklist) есть в релизе (кроме файлов .spk).
6. Проверьте настройки хранения для таблиц в Greenplum.
7. Убедитесь, что обслуживающие операции `vacuum` и `analyze` присутствуют и корректно работают.
8. Убедитесь, что целевые схемы основных слоев хранилища загружаются в sql-файлы релиза с помощью лоадеров. Не используйте `insert` вне лоадеров.
9.  Проверьте, что приоритеты в моделях состоят из 8 символов.
10. Убедитесь, что в теле DAG в параметре `dagrun_timeout` указана переменная `DAG_VAR_NAME`. Если в параметре указано другое значение, проверьте, что в задаче Kaiten в комментарии сотрудник команды DWH согласовал изменения. Если изменения не согласованы, верните задачу, чтобы ее согласовали.

## Проверка DAG
1. Название DAG задано заглавными буквами.
2. Убедитесь, что корректно назвали DAG. В названии должен быть ключ из словаря `libs_mapping` файла file.yml (см. [шаблон](./cheklist)).
3. Проверьте, что DAG был запущен на контуре разработки и все используемые библиотеки импортированы.
4. Убедитесь, что модуль Param импортирован из библиотеки airflow.models.param. В параметрах подключения params в свойстве `target_name` указан модуль Param со значением `prod`:
    
```
    from airflow.models.param import Param
    ***
    params = {
        "target_name": Param("prod", type="string"),
        "<имя_переменной_cut_param>": Param("", type=["null", "string"])
        },
```