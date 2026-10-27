# Публичный поиск групп расписания

**Система:** официальный портал Самарского университета (`ssau.ru`)  
**Статус:** маршрут найден в клиенте расписания и проверен 2026-10-27  
**Доступ:** публичный; POST требует cookie/CSRF-контекст страницы

## Маршрут

```http
POST https://ssau.ru/rasp/search
Content-Type: application/x-www-form-urlencoded
Accept: application/json
X-CSRF-TOKEN: <токен со страницы /rasp>

text=4144
```

Перед поиском клиент получает `GET https://ssau.ru/rasp`, сохраняет выданные сайтом cookies и извлекает значение из `<meta name="csrf-token">`. Затем отправляет это значение в заголовке `X-CSRF-TOKEN` вместе с полем формы `text`.

## Ответ

JSON-массив совпадений:

```json
[
  {
    "id": 755901942,
    "url": "/rasp?groupId=755901942",
    "text": "4144-100301D"
  }
]
```

Приведённый результат получен запросом `text=4144`. Типы и семантика дополнительных полей и поведение для пустого результата не проверялись.

## Повторить запрос

```python
from http.cookiejar import CookieJar
from urllib.parse import urlencode
from urllib.request import HTTPCookieProcessor, Request, build_opener
import re

base = "https://ssau.ru"
opener = build_opener(HTTPCookieProcessor(CookieJar()))
page_request = Request(base + "/rasp", headers={"User-Agent": "Mozilla/5.0"})
with opener.open(page_request) as response:
    page = response.read().decode("utf-8")
    referer = response.geturl()

match = re.search(r'<meta[^>]+name=["\']csrf-token["\'][^>]+content=["\']([^"\']+)', page, re.I)
headers = {
    "Accept": "application/json",
    "Referer": referer,
    "Content-Type": "application/x-www-form-urlencoded",
}
if match:
    headers["X-CSRF-TOKEN"] = match.group(1)
request = Request(
    base + "/rasp/search",
    data=urlencode({"text": "4144"}).encode(),
    headers=headers,
    method="POST",
)
with opener.open(request) as response:
    print(response.status, response.headers.get("content-type"), response.read().decode())
```

HTTP-метод POST используется для поиска и не изменяет данные. API Anything обучает и вызывает только чтения через безопасный GET-путь по умолчанию, поэтому этот маршрут оставлен в OpenAPI и карточке, но не добавлен в его read-only operations spec.
