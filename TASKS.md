# Статус

- Завершено: ресерч официальной документации и консервативный профиль.
- Завершено: профиль, README RU/EN, compat, Registry manifest, skill, Dockerfile, GoReleaser и переиспользуемый CI.
- Завершено: Docker build/vet/race/lint, versitygw, MCP для 8 уровней, мутации 87/87 (100%), check_docs, actionlint, GoReleaser snapshot (6 архивов), сборка образа под uid владельца и runtime USER 65532.
- Доставка завершена: main опубликован, описание/topics настроены; [CI реализации `930570d`](https://github.com/stufently/yandex-s3-mcp-ai-cli/actions/runs/38063914470) зелёный.
- Предположения: поддержка destination conditional copy и conditional delete; десятичная граница PUT; нормализация месячных цен. Детали и источники — docs/research/yandex-s3.md.
- Нужна живая проверка: эндпоинты/регионы, условные заголовки, классы/цены, presign, API и ошибки на реальном тестовом бакете с ключом владельца.
- Релиз v0.1.0 и публикация OCI/Registry отложены до живой проверки.
