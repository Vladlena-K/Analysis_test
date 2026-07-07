# Проектирование REST API для экрана «Список магазинов-партнеров» 

### HTTP-Запрос

```http 
GET /api/v1/partners/stores?city_code=MOW&limit=10&offset=0 HTTP/1.1 
Host: api.green-parsley.shop
Accept: application/json 
Authorization: Bearer <user_token> 
```

Параметры: 
- `city_code` — код города (пример: `MOW` для Москвы),
- `limit` — количество записей на страницу (по умолчанию 10), 
- `offset` — смещение для пагинации (по умолчанию 0).

### Структура JSON:

{
  "status": "success",
  "data": {
    "stores": [
      {
        "id": "store_metro_001",
        "name": "METRO",
        "logo_url": "https://cdn.green-parsley.shop/logos/metro_blue.png",
        "delivery": {
          "type": "nearest",
          "text": "Ближайшая доставка сегодня 21:00-23:00"
        },
        "action_url": "https://metro-cc.ru/app-redirect"
      },
      {
        "id": "store_vkusvill_112",
        "name": "ВкусВилл",
        "logo_url": "https://cdn.green-parsley.shop/logos/vkusvill_green.png",
        "delivery": {
          "type": "fast",
          "text": "Быстрая доставка от 20 до 60 минут"
        },
        "action_url": "https://vkusvill.ru/app-redirect"
      }
    ]
  },
  "meta": {
    "total_count": 4,
    "offset": 0,
    "limit": 10,
    "has_more": false
  }
}


### Корневые поля 
| Поле | Тип | Обязательное | Возможные значения | Описание | 
| --- | --- | --- | --- | --- | 
| `status` | string | Да | `success`, `error` | Общий статус обработки запроса. | 
| `data` | object | Да | — | Полезная нагрузка (данные магазинов). | 
| `meta` | object | Да | — | Метаданные для пагинации и аналитики. | 

### Поля объекта `data.stores[]` (карточки магазинов) 
| Поле | Тип | Обязательное | Описание | Пример | 
| --- | --- | --- | --- | --- | 
| `id` | string | Да | Уникальный идентификатор магазина. | `store_metro_001` | 
| `name` | string | Да | Название магазина (отображается на плашке). | `METRO` | 
| `logo_url` | string | Да | Ссылка на логотип (используется в интерфейсе). | `https://cdn.../metro.png` | 
| `delivery.type` | string | Да | Тип доставки для логики отображения. | `fast`, `nearest` | 
| `delivery.text` | string | Да | Текст слота доставки (под названием). | `Быстрая доставка от 20 до 60 минут` | 
| `action_url` | string | Да | Ссылка перехода при клике на карточку. | `https://metro-cc.ru/app-redirect` | 

### Поля объекта `meta` (пагинация) 
| Поле | Тип | Обязательное | Описание | 
| --- | --- | --- | --- | 
| `total_count` | integer | Да | Общее количество магазинов по фильтру. | 
| `offset` | integer | Да | Смещение (соответствует запросу). | 
| `limit` | integer | Да | Лимит записей (соответствует запросу). | 
| `has_more` | boolean | Да | Есть ли ещё страницы для подгрузки. | 
