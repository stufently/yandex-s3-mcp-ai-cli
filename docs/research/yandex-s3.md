# Yandex Cloud S3 — официальный ресерч

Проверено 2026-10-10 по официальной документации; живой аккаунт не использовался. **[предположение]** означает, что сведения не подтверждены документом или требуют проверки сервиса. Значения профиля описывают возможности этой реализации, а не обещание полной AWS-совместимости.

## Эндпоинт и адресация

| Параметр | Документированное значение | Источник |
|---|---|---|
| S3 endpoint | `https://storage.yandexcloud.net` | [S3 quickstart](https://yandex.cloud/ru/docs/storage/s3/s3-api-quickstart) |
| SigV4 region | `ru-central1` | [boto / конфигурация региона](https://yandex.cloud/ru/docs/storage/tools/boto) |
| Path-style | `https://storage.yandexcloud.net/bucket/key` | [S3 API](https://yandex.cloud/ru/docs/storage/s3/) |
| Virtual-host | `https://bucket.storage.yandexcloud.net/key` | [Объект](https://yandex.cloud/ru/docs/storage/concepts/object) |
| Резервирование | Классы защищают данные при потере одной из трёх зон доступности; STANDARD/COLD используют три реплики | [Классы](https://yandex.cloud/ru/docs/storage/concepts/storage-class) |

Профиль использует path-style и один документированный регион подписи. Панель и IAM API не входят в CLI; статический S3-ключ создаёт владелец. [Настройка boto](https://yandex.cloud/ru/docs/storage/tools/boto).

## Условные запросы и возможности

| Возможность | Что подтверждено | Источник |
|---|---|---|
| PUT `If-None-Match:*`, `If-Match` | Запись только при отсутствии ключа либо совпадении текущего ETag | [PutObject](https://yandex.cloud/ru/docs/storage/s3/api-ref/object/upload) |
| CompleteMultipartUpload | Те же условные заголовки документированы | [completeUpload](https://yandex.cloud/ru/docs/storage/s3/api-ref/multipart/completeupload) |
| CopyObject | Документированы `X-Amz-Copy-Source-If-*`; условие отсутствия **целевого** ключа не описано | [CopyObject](https://yandex.cloud/ru/docs/storage/s3/api-ref/object/copy) |
| DeleteObject `If-Match` | **[предположение]** поддержка не установлена: метод перечисляет общие заголовки и обход governance-retention, но не условное удаление | [DeleteObject](https://yandex.cloud/ru/docs/storage/s3/api-ref/object/delete) |
| Object tagging | Put/Get/DeleteObjectTagging | [Методы Object](https://yandex.cloud/ru/docs/storage/s3/api-ref/object) |
| Bucket tagging | Put/Get/DeleteBucketTagging | [Методы Bucket](https://yandex.cloud/ru/docs/storage/s3/api-ref/bucket) |
| Lifecycle, ACL, CORS, policy | Есть соответствующие группы S3 REST-методов | [Все методы S3](https://yandex.cloud/ru/docs/storage/s3/api-ref/) |
| Versioning | Включение/приостановка и список версий | [Versioning](https://yandex.cloud/ru/docs/storage/concepts/versioning) |
| Object Lock config / retention | Блокировки версий и governance/compliance режимы | [Object Lock](https://yandex.cloud/ru/docs/storage/concepts/object-lock) |
| Multipart | Составная загрузка и завершение доступны | [completeUpload](https://yandex.cloud/ru/docs/storage/s3/api-ref/multipart/completeupload) |

**[предположение]** Destination `If-None-Match` для CopyObject не считаем поддержанным без живого теста. Общий `conditional_writes:false` поэтому консервативно отключает режим записи без перезаписи для всего набора операций kit, хотя PUT/complete документированы. Запись требует явного `overwrite:true` и политики подтверждения; HEAD не заменяет атомарность. `conditional_delete:false` исключает `s3_move_object`; CLI mv отказывает до запросов. [Copy](https://yandex.cloud/ru/docs/storage/s3/api-ref/object/copy), [Delete](https://yandex.cloud/ru/docs/storage/s3/api-ref/object/delete).

Lifecycle, ACL, bucket tagging и retention записаны в профиль как возможности облака, но инструменты их настройки отсутствуют в текущем kit. Доступные операции отображаются в `capabilities` и README.

## Классы и цены

Классы: `STANDARD`, `COLD`, `ICE`, `INTELLIGENT_TIERING`; класс можно выбрать при загрузке/копировании и менять через повторную загрузку либо lifecycle. У ICE минимальная оплачиваемая длительность — 12 месяцев. [Классы](https://yandex.cloud/ru/docs/storage/concepts/storage-class), [Тарификация](https://yandex.cloud/ru/docs/storage/pricing).

Тарифы региона Россия с 30.04.2026: STANDARD — 0,0033 ₽/ГБ·час сверх бесплатного объёма; COLD — 0,00176; ICE — 0,00088. Intelligent-tiering использует эти ставки по уровню доступа плюс 0,00039 ₽ за 1000 объектов·час. В биллинге ГБ = 2^30 байт; операции и исходящий трафик оплачиваются отдельно. [Действующие тарифы](https://yandex.cloud/ru/docs/storage/pricing).

**[предположение]** Числа 2,376 / 1,2672 / 0,6336 ₽/ГБ·месяц в профиле — только нормализация на 720 часов, не фиксированный месячный счёт (`verified:false`). Для intelligent-tiering `price:0` означает отсутствие фиксированной оценки, не бесплатное хранение. [Правила и примеры расчёта](https://yandex.cloud/ru/docs/storage/pricing).

## Presign

`X-Amz-Expires` допускает до **2592000 секунд (30 дней)**; GET и PUT с подписью доступны. Профиль сохраняет этот документированный потолок, но `verified:false`: нужно подтвердить реальную ссылку и срок ключа. [Подписанные URL](https://yandex.cloud/ru/docs/storage/concepts/pre-signed-urls).

## Лимиты и ошибки

- Объект до 5 ТБ, один запрос загрузки до 5 ГБ, multipart до 10000 частей; минимальная часть, кроме последней, 5 МБ. Заголовки до 8 КБ, пользовательские метаданные до 2 КБ, policy до 20 КБ. Квоты по умолчанию: 25 бакетов и 1024 ГБ на облако; квоты можно увеличить. [Квоты и лимиты](https://yandex.cloud/ru/docs/storage/concepts/limits).
- Превышение числа бакетов: `400 TooManyBuckets`; слишком большой объект: `400 EntityTooLarge`; метаданные: `400 MetadataTooLarge`; условие не выполнено: HTTP 412; частота запросов: `429 TooManyRequests`, также документированы `503 SlowDown`/`ServiceUnavailable`. [Коды S3](https://yandex.cloud/ru/docs/storage/s3/api-ref/response-codes).
- Числовой лимит RPS в проверенной странице лимитов не установлен: `rate_limits:{}`. **[предположение]** `max_put_bytes:5000000000` — консервативное десятичное представление документированных 5 ГБ; точную границу и ошибки квоты объёма проверить живым ключом. [Лимиты](https://yandex.cloud/ru/docs/storage/concepts/limits).

## Нужна живая проверка

Проверить условные PUT/complete/copy и DeleteObject, presign на границе срока, классы по умолчанию, тариф аккаунта, реальные ответы квот и rate limits, поддерживаемые bucket API. Docker/versitygw проверяет реализацию и регистрацию инструментов, но не заменяет проверку Yandex Cloud.
