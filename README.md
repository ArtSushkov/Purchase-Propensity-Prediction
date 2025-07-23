# Purchase Propensity Prediction
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

Прогнозирование вероятности совершения покупки клиентом в течение 90 дней для оптимизации маркетинговых кампаний интернет-магазина.

## Описание проекта

Интернет-магазин собирает историю покупок и данные о маркетинговых рассылках. Цель проекта — идентификация клиентов с высокой вероятностью совершения покупки в ближайшие 90 дней для точечного таргетинга.

**Цель:**  
Предсказать вероятность покупки (бинарная классификация) с оптимизацией метрики ROC-AUC.

### Данные
- **apparel-purchases**: История покупок
  - `client_id`, `quantity`, `price`, `category_ids`, `date`, `message_id`
  
- **apparel-messages**: Рекламные рассылки
  - `bulk_campaign_id`, `client_id`, `message_id`, `event`, `channel`, `date`, `created_at`
  
- **apparel-target_binary**: Целевая переменная
  - `client_id`, `target` (1/0)

## Ключевые признаки

### Общие признаки по истории покупок
| Признак               | Описание                                  | 
|-----------------------|------------------------------------------|
| `purchase_count`      | Общее количество покупок клиента         |
| `total_spent`         | Общая сумма всех покупок                 |
| `max_purchase`        | Максимальная сумма покупки               |

### Временные признаки
| Признак                     | Описание                                    |
|-----------------------------|--------------------------------------------|
| `customer_age_days`         | "Возраст" клиента в днях                   |
| `days_since_last_purchase`  | Дней без покупок                           |
| `purchase_trend_90d`        | Динамика покупок (разница между периодами) |

### Финансовые метрики
| Признак               | Описание                                |
|-----------------------|----------------------------------------|
| `cv_spent`            | Коэффициент вариации трат              |
| `is_single_purchase`  | Флаг единственной покупки (1/0)        |

### Реакция на рассылки
| Признак                     | Описание                                           |
|-----------------------------|---------------------------------------------------|
| `purchases_after_campaign`  | Покупки после рассылок                            |
| `campaign_purchase_ratio`   | Доля покупок, связанных с рассылками              |
| `total_clicks`              | Общее количество кликов по рассылкам              |
| `total_purchase_events`     | Кол-во покупок, напрямую связанных с рассылками   |
| `global_negative_rate`      | Процент негативных реакций на рассылки            |

## Установка и запуск

1. **Клонировать репозиторий**:
   ```bash
   git clone https://github.com/yourusername/purchase-propensity-prediction.git
   cd purchase-propensity-prediction
   ```

2. **Создать виртуальное окружение**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # Linux/Mac
   venv\Scripts\activate     # Windows
   ```

3. **Установить зависимости**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Запуск Jupyter Notebook**:
   ```bash
   jupyter notebook
   ```
   Открыть файл `notebooks/purchase_propensity_prediction.ipynb`

## Структура проекта
```
.gitignore
LICENSE
README.md
requirements.txt
notebooks/
    └── purchase_propensity_prediction.ipynb
```

## Методология

### Feature Engineering
1. **Общие признаки по истории покупок клиента**
2. **Временные признаки** (активность клиента)
3. **Финансовые метрики** (стабильность трат)
4. **Признаки реакции на рассылки**

### Использованные модели
- LogisticRegression (RandomUnderSampler+SMOTETomek)
- LogisticRegression (class_weight='balanced')
- DecisionTreeClassifier
- CatBoostClassifier
- LGBMClassifier

## Результаты

### Пример вывода модели
```
    client_id	        probability
0  1515915625468060902	0.368312
1  1515915625468061003	0.391517
2  1515915625468061099	0.220703
3  1515915625468061100	0.554892
4  1515915625468061170	0.700665
```

### Метрики LogisticRegression
```
              precision    recall  f1-score   support

           0       0.99      0.71      0.83     48888
           1       0.04      0.60      0.07       961

    accuracy                           0.71     49849
   macro avg       0.51      0.66      0.45     49849
weighted avg       0.97      0.71      0.81     49849

ROC-AUC: 0.7131
```

## Лицензия
MIT License. Подробнее см. в файле [LICENSE](LICENSE).
