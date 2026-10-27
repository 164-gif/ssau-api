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

`ListIdentifiers`, `GetRecord` и `ListRecords` пока не проверялись. Не выполнялась выгрузка записей репозитория.

## Другие проверенные адреса

| Путь | Результат `Identify` |
| --- | --- |
| `/server/oai/request` | `404` |
| `/jspui/oai/request` | `404` |
| `/jspui/dspace-oai/request` | `404` |

Эти ответы ограничивают проверку именно указанными путями; они не доказывают отсутствие других интерфейсов DSpace.
