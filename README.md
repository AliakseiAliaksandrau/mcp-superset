# mcp-superset (форк)

Форк [bintocher/mcp-superset](https://github.com/bintocher/mcp-superset) — MCP-сервера для Apache Superset.
Полная документация по инструментам, настройке и запуску — в [оригинальном репозитории](https://github.com/bintocher/mcp-superset#readme).

## Зачем этот форк

1. **Источник кода для нашего прода.** Два MCP-сервиса рядом с Superset (`superset-mcp-admin`
   и `superset-mcp-user`) ставят пакет отсюда при каждом старте контейнера — по конкретному коммиту:

   ```
   pip install 'https://github.com/AliakseiAliaksandrau/mcp-superset/archive/<sha>.tar.gz'
   ```

   Ветка `main` этого форка соответствует тому, что сейчас работает на проде.

2. **Свои доработки, которых нет в upstream.** Сейчас это одна:

   | Коммит | Что делает |
   |--------|-----------|
   | `cce8d4a` | `superset_chart_create` и `superset_chart_update` принимают необязательный параметр `description`. В оригинале его не было, и описание графика нельзя было задать через MCP, хотя API Superset (`POST`/`PUT /api/v1/chart`) его принимает. |

## Как обновить прод

1. Подтянуть нужное из `upstream` в `main` и сохранить доработки форка сверху.
2. Запушить `main`.
3. На проде заменить sha в `pip install` у обоих сервисов на новый коммит `main`
   и пересоздать только их контейнеры (с `--no-deps`, чтобы не задеть сам Superset).
