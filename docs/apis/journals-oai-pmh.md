# Метаданные электронных журналов: OAI-PMH

**Система:** [journals.ssau.ru](https://journals.ssau.ru)  
**Статус:** публичный протокол проверен 2026-10-27 на корневом портале и журнале `computeroptics`.

## Базовый адрес

```text
https://journals.ssau.ru/index/ru/oai
```

В интерфейсе отдельного журнала встречается отдельная база, например:

```text
https://journals.ssau.ru/computeroptics/ru/oai
```

Это OAI-PMH — XML-протокол выдачи метаданных публикаций. Проверены следующие read-only запросы:

| Запрос | Результат |
| --- | --- |
| `?verb=Identify` | Идентификация репозитория, версия протокола и сведения об администраторе |
| `?verb=ListMetadataFormats` | Поддерживаемые схемы: `rfc1807`, `oai_marc`, `oai_dc`, `marcxml` |
| `?verb=ListSets` | Список наборов; корневой портал вернул XML около 53 KB |
| `?verb=ListIdentifiers&metadataPrefix=oai_dc` | Идентификаторы записей и токен продолжения. Корневой портал вернул страницу из 500 заголовков |
| `?verb=GetRecord&metadataPrefix=oai_dc&identifier=…` | Метаданные одной записи; проверено на `computeroptics` по ID, полученному из `ListIdentifiers` |

Все пять запросов отвечали `200 text/xml;charset=utf-8` без входа; перечисление идентификаторов также выдаёт `resumptionToken` для следующей страницы. Для `computeroptics` отдельно проверены все пять операций; `ListSets` вернул 13 наборов. Сами записи статей не сохранялись.

### Ограничение общего OAI endpoint

У общего endpoint `/index/ru/oai` `ListIdentifiers` выдаёт идентификаторы, но передача первого возвращённого ID в `GetRecord` отвечает OAI-ошибкой `badArgument`. У endpoint `computeroptics` связка `ListIdentifiers` → `GetRecord` с точным идентификатором работает. Поэтому для получения записи используйте OAI endpoint конкретного журнала; общий endpoint пока пригоден для перечисления заголовков, но не подтверждён для `GetRecord`.

Пример:

```bash
curl 'https://journals.ssau.ru/index/ru/oai?verb=ListMetadataFormats'
```

`ListRecords`, `ListIdentifiers` с фильтром `set`/датами и переход по `resumptionToken` пока не проверялись. Учитывайте размер страницы: один запрос `ListIdentifiers` общего endpoint вернул 500 заголовков (около 89 KB). Не выгружайте коллекции циклом без ограничений.
