# Прогнозирование оттока клиентов из «Бета-Банка»

Из «Бета-Банка» стали уходить клиенты. Отток клиентов небольшой на текущий момент, но уже заметен. Банковские маркетологи посчитали: сохранять текущих клиентов дешевле, чем привлекать новых. Необходимо уметь делать прогноз по клиенту: уйдет ли он из банка в ближайшее время или нет.

## В данном проекте исследуется
* Исторические данные о поведении клиентов «Бета-Банка», часть из которых уже перестали пользоваться услугами банка и растрогли договор.

## Цель проекта
* Построить и обучить модель, которая сможет прогнозировать, уйдёт клиент из банка в ближайшее время;
* Метрика качества модели F1-мера должна быть не менее 0.59.

## Описание предоставленных данных
Датасет `Churn.csv` содержит 10000 строк (явные дубликаты отсутствуют) и следующие столбцы:

* `RowNumber` — индекс строки в данных;
* `CustomerId` — уникальный идентификатор клиента;
* `Surname` — фамилия;
* `CreditScore` — кредитный рейтинг;
* `Geography` — страна проживания;
* `Gender` — пол;
* `Age` — возраст;
* `Tenure` — сколько лет человек является клиентом банка;
* `Balance` — баланс на счёте;
* `NumOfProducts` — количество продуктов банка, используемых клиентом;
* `HasCrCard` — наличие кредитной карты;
* `IsActiveMember` — активность клиента;
* `EstimatedSalary` — предполагаемая зарплата;
* `Exited` — факт ухода клиента;

## Выбор и обучение модели
* Для обучения модели в качестве целевого признака выбрано значение столбца `Exited`.

* Датасет разбит на три выборки в пропорции `3:1:1` - обучающая (5883 наблюдений), валидационная (2000 наблюдения) и тестовая (2000 наблюдений).

* В обучающей выборке обнаружен дисбаланс классов приблизительно в пропорции `5:1`. Для исследования методов подавления дисбаланса построены увеличенная и уменьшенная обучающая выборка, в которых пропорция классов близка к `1:1`.

* К обучающей выборке применен механизм масштабирования количественных признаков и OHE кодирование категориальных признаков.

* Произведена очистка обучающей выборки от аномальных значений признаков `Age`, `NumOfProducts` и `CreditScore`.

* Исследованы 3 классификатора: Решающее дерево, Случайный лес и Логистическая регрессия. Каждый классификатор обучался на выборке с дисбалансом классов, на увеличенной и уменьшенной сбалансированной выборке, а также применялся механизм взвешивания классов.

* Для Решающего дерева и Случайного леса более высокая F1-метрика была у моделей, которые обучены на увеличенной выборке без использования кросс-валидации. Для Логистической регрессии лучший результат показала модель, обученная с использованием взвешивания классов и также без использования кросс-валидации.

* Наилучшей моделью по критерию максимальной F1-метрики на валидационной выборке определен следующий классификатор: `RandomForestClassifier(max_depth=10, n_estimators=40, random_state=24092010, criterio='gini', 'class_weight': None)`, F1-мера на валидационной выборке: `0.6489`. При изменении порога классификации на значение `0.457` удалось достичь F1-меры `0.650`. Обучение данной модели проводилось на увеличенной выборке без использования кросс-валидации.



## Результаты тестирования модели

* Выбранная модель показала следующие результаты на тестовой выборке: 
    * `Accuracy: 0.807`
    * `Recall: 0.722`
    * `Precision: 0.529`
    * `F1-мера: 0.611`
    * `ROC_AUC: 0.851`

* Довольно высокое значение метрик F1 и ROC_AUC показывает, что модель является адекватной и показывает значительно лучшие результаты чем случайная модель предсказаний.

* Наиболее важными признаками для предсказания оказались: возраст клиента, число используемых продуктов банка, баланс на счету, предполагаемый доход, кредитный рейтинг, признак активности клиента и как долго клиент пользуется услугами банка.

