# Yandex Cloud S3 MCP Server & AI CLI

## English

**Yandex Object Storage MCP server + AI CLI** — one Go binary that lets AI agents (Claude Code, Cursor,
Codex, Claude Desktop) and humans manage Yandex Object Storage: buckets, objects, presigned URLs,
multipart uploads and backups, with a safety gate for destructive operations.

> **Status: in development.** The first implementation of this family is
> [selectel-s3-mcp-ai-cli](https://github.com/stufently/selectel-s3-mcp-ai-cli);
> Yandex Cloud follows with the same feature set.

## О проекте

**MCP-сервер и CLI для S3 Яндекс Облако.** Один бинарь `yandex-s3`: MCP-сервер для AI-агентов
(Claude Code, Cursor, Codex, Claude Desktop), CLI для людей и скриптов и skill с
готовыми сценариями — бэкап, раздача файлов по ссылке, аудит расходов, миграция.

**Статус: в разработке.** Первая реализация семейства —
[selectel-s3-mcp-ai-cli](https://github.com/stufently/selectel-s3-mcp-ai-cli);
Yandex Cloud получит тот же функционал.

## Что будет

- Бакеты и объекты через S3 API, presigned-ссылки и multipart — данные идут мимо модели.
- Уровни доступа: по умолчанию только чтение, запись и удаление — явными флагами.
- Подтверждение опасных операций, скоуп по бакетам и префиксам, аудит каждого вызова.
- `mcp install` для Claude Code, Cursor, Codex и Claude Desktop одной командой.
- Skill для AI-агентов и таблица совместимости Yandex Object Storage с S3 API.

## Лицензия

MIT
