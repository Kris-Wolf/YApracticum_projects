# Определение стоимости автомобилей
## Описание проекта
Разработка моделей машинного обучения для предсказания рыночной стоимости автомобилей клиентов сервиса.
## Навыки и инструменты
- datetime
- lightgbm.**LGBMRegressor**
- matplotlib.**pyplot**
- numpy
- pandas
- phik
- seaborn
- sklearn.compose.**ColumnTransformer**
- sklearn.ensemble.**RandomForestRegressor**
- sklearn.impute.**SimpleImputer**
- sklearn.linear_model.**LinearRegression**
- sklearn.metrics.**mean_squared_error**
- sklearn.**model_selection**
- sklearn.pipeline.**Pipeline**
- sklearn.**preprocessing**
- sklearn.tree.**DecisionTreeRegressor**
## Общий вывод
**Цели проекта выполнены:** 
- Загружены, изучены и очищены данные.
- Подготовлены выборки для обучения моделей.
- Обучены разные модели и подобраны гиперпараметры.
- Проанализировано время обучения, время предсказания и качество моделей.
- Выбрана лучшая модель, предсказывающая рыночную стоимость автомобиля, проверено её качество на тестовой выборке.


**В ходе выполнения работы проделаны следующие шаги:**
- Загружены и проверены соответствия данных в датафреймах;
- В результате **предобработки данных** 
    - названия столбцов датафрейма приведены к змеиному регистру;
    - найдены и удалены явные дубликаты;
    - найдены и обработаны неявные дубликаты;
    - обработаны пропуски: данные приведены к моде по значению другого подходящего столбца (бренда или модели авто).
- В ходе **исследовательского анализа данных**:
    - удалены автомобили с нулевой стоимостью;
    - значение `power` равное `0` л.с. заменено на медиану, автомобили с мощьностью более 650 л.с. удалены из выборки;
    - удалены записи, где `registration_year` более `2016` и менее `1885`;
    - проведен корреляционный анализ.
        - найдена мультиколлинеарность между признаками `model` и `brand` и силльный коэффициент корреляции между признаками `model` и `vehicle_type`.
        - изучены коэффициенты корреляции, с целевым признаком `price` больше всего коррелируют `model`, `registration_year`, `power`; меньше всего - `fuel_type`.
    - удалены признаки `date_crawled `, `vehicle_type`, `registration_month`, `brand`, `date_created`, `number_of_pictures`, `postal_code`, `last_seen`.
--------------------------------------------------------------------------
Самые частые значения признаков выглядят следующим образом:
- `price` - 2900 евро;
- `registration_year`- 2003;
- `power` - 105;
- `kilometer` -  150 000 км;
- `vehicle_type` - sedan (31%), small (25%), wagon (20%);
- `gearbox` - manual (81%);
- `fuel_type` - gasoline (98%);
- `brand` - volkswagen (22%), opel (11%) и bmw (10%);
- `repaired` - большинство авто **не были в ремонте** (90%).
--------------------------------------------------------------------------
- Обучены модели (`LinearRegression`, `LightGBM`, `DecisionTree`): 
     - Лучшей моделью является **DecisionTree** с параметрами `max_depth=152`, `min_samples_leaf=11`, `min_samples_split=4`. Модель дает следующие результаты:
        - Метрика **RMSE** на тренировочной выбборке: 2056.467
        - Метрика **RMSE** на тестовой выбборке: 2062.201
        - Время обучения: 0.011 минуты  
        - Время предсказания на тренировочной: 0.003 минуты
        - Время предсказания на тестовой: 0.001 минуты

Рекомендуется использовать модель дерева решений (**DecisionTree**) с параметрами {`max_depth=152`, `min_samples_leaf=11`, `min_samples_split=4`}, так как она подходит под критерии заказчика (скорость и качество обучения и предсказаний).
- В дальшейшем можно улучшить качество предсказаний используя другие виды моделей машинного обучения и/или другой набор гиперпараметров. 
- Для улучшения качества предсказаний следует избавиться от неинформативных признаков (кол-во фотографий, почтовый индекс владельца анкеты и тд.), проследить и исключить аномалии в данных (год, мощность и тп.), а также добавить новые признаки, влияющие на стоимость автомобиля.
