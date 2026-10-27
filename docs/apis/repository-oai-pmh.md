# OAI-PMH репозитория университета

**Система:** [repo.ssau.ru](https://repo.ssau.ru)  
**Статус:** `Identify`, `ListMetadataFormats` и `ListSets` проверены 2026-10-27 без авторизации.

## Базовый адрес

```text
https://repo.ssau.ru/oai/request
```

Это OAI-PMH endpoint репозитория. Документация DSpace описывает такой адрес как стандартный шаблон OAI servlet и рекомендует проверять его запросом `?verb=Identify`; на сайте университета именно этот путь вернул OAI-PMH XML. [Справочник DSpace по OAI](https://wiki.lyrasis.org/spaces/DSDOC6x/pages/68064776/OAI)

## Проверенные операции

| Запрос | Ответ |
| --- | --- |
| `?verb=Identify` | `200 application/xml`; ответ — OAI-PMH XML |
| `?verb=ListMetadataFormats` | `200 application/xml`; доступны `uketd_dc`, `qdc`, `didl`, `mods`, `ore`, `mets`, `oai_dc`, `rdf`, `marc`, `xoai`, `dim`, `etdms` |
| `?verb=ListSets` | `200 application/xml`; в ответе 100 наборов, токена продолжения нет |
| `?verb=ListIdentifiers&metadataPrefix=oai_dc` | `200 application/xml`; первая страница содержит 100 заголовков и токен продолжения |
| `?verb=GetRecord&metadataPrefix=oai_dc&identifier=…` | `200 application/xml`; проверено по идентификатору из `ListIdentifiers` |

У проверенной записи присутствовали элементы Dublin Core `title`, `creator`, `subject`, `date`, `type`, `identifier`, `language`, `relation`, `coverage` и `source`. Значения метаданных в каталог не сохранялись. `ListRecords` и переход по токену продолжения пока не проверялись; выгрузка корпуса не выполнялась.

## Другие проверенные адреса

| Путь | Результат `Identify` |
| --- | --- |
| `/server/oai/request` | `404` |
| `/jspui/oai/request` | `404` |
| `/jspui/dspace-oai/request` | `404` |

Эти ответы ограничивают проверку именно указанными путями; они не доказывают отсутствие других интерфейсов DSpace.
