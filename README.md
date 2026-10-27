# Прогноз медицинских расходов

Прогноз годовых медицинских расходов по возрасту, BMI, числу детей, курению, полу и региону проживания.

Автор исходного урока — **Aakash NS (Jovian)**: [ноутбук](https://jovian.ai/aakashns/python-sklearn-linear-regression), [официальная копия курса](https://github.com/JovianHQ/notebooks/blob/main/machine-learning-with-python-zero-to-gbms/lesson-1-linear-regression-with-scikit-learn/lesson-1-linear-regression-with-scikit-learn.ipynb). Данные: [medical-charges.csv](https://raw.githubusercontent.com/JovianML/opendatasets/master/data/medical-charges.csv), [исходный репозиторий](https://github.com/stedy/Machine-Learning-with-R-datasets).

Рабочий ноутбук — `medical-insurance-cost-prediction.ipynb`. Англоязычная версия до перевода сохранена в `medical-insurance-cost-prediction-en.ipynb`, неизменённый оригинал преподавателя — в `python-sklearn-linear-regression-original.ipynb`. Русификация меняет только представление информации; названия столбцов и категории исходных данных сохранены.

Продолжение находится в разделе **«Сравнение моделей и итоговая оценка качества»**. В исходном уроке уже есть разделение 90/10. Продолжение повторяет эксперимент на исходных признаках с разделением 80/20; предобработка обучается внутри Pipeline каждого фолда.

## Результаты

После удаления одного полного дубликата: **1069 наблюдений в обучающей выборке и 268 в тестовой**, `random_state=42`. Модели сравниваются по средним метрикам кросс-валидации на пяти фолдах обучающей выборки. Параметры Ridge и RandomForestRegressor подбираются по RMSE.

| Модель | MAE на валидации | RMSE на валидации | R² на валидации |
|---|---:|---:|---:|
| RandomForestRegressor | 2544.33 | 4577.40 | 0.8438 |
| Ridge | 4222.98 | 6123.64 | 0.7228 |
| LinearRegression | 4221.96 | 6123.65 | 0.7228 |
| DummyRegressor | 8789.32 | 11688.80 | -0.0025 |

По кросс-валидации выбран **RandomForestRegressor**: `n_estimators=300`, `max_depth=5`, `min_samples_leaf=5`. RMSE на валидации — **4577.40 ± 195.95** (среднее ± стандартное отклонение по фолдам).

На тестовой выборке проверена только выбранная модель: **MAE 2382.14, RMSE 4206.10, R² 0.9037**. MAE и RMSE выражены в единицах `charges`. Тестовая выборка не используется для выбора модели или гиперпараметров.

В ноутбуке показаны ошибки прогнозирования, графики и важность признаков. Наиболее значимы курение, BMI и возраст. Файл `medical_cost_model.joblib` содержит Pipeline с предобработкой и моделью, обученными на обучающей выборке. Последние ячейки проверяют загрузку модели и прогноз для нового клиента.

## Запуск

Python 3.13. Выполните команды из папки проекта:

```powershell
py -3.13 -m venv .venv
./.venv/Scripts/python.exe -m pip install -r requirements.txt
./.venv/Scripts/python.exe -m ipykernel install --sys-prefix --name medical-insurance --display-name "Python (Medical Insurance)"
./.venv/Scripts/python.exe -m jupyterlab medical-insurance-cost-prediction.ipynb
```

Выберите ядро **Python (Medical Insurance)**, затем перезапустите ядро и выполните все ячейки (**Restart Kernel and Run All Cells**). Данные `medical.csv` и файл русской локализации `plotly-ru.json` должны находиться рядом с ноутбуком. Локализация основана на [официальном словаре Plotly](https://cdn.plot.ly/plotly-locale-ru-latest.js).

Полный запуск с чистого ядра, совпадение вычислений с англоязычной версией и сохранение/загрузка модели проверены. Для сравнения случайного разбиения 90/10 в исходном уроке использовано одинаковое начальное состояние генератора; при обычном повторном запуске метрики этого учебного примера могут изменяться.

На английском сохранены технические идентификаторы, автоматическая справка и служебные сообщения библиотек, а также текст внутри внешних авторских изображений. Ссылки на эти изображения оставлены без изменений.

