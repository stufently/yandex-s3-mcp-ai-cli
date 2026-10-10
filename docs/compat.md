# Yandex Cloud S3 compatibility

Generated from the embedded provider profile.

| Feature | Supported |
|---|---|
| acl | true |
| bucket_policy | true |
| bucket_tagging | true |
| conditional_delete | false |
| conditional_writes | false |
| cors | true |
| lifecycle | true |
| multipart | true |
| object_lock_config | true |
| object_retention | true |
| object_tagging | true |
| presign | true |
| versioning | true |

| Pool | Endpoint | Location | Multi-AZ | Classes |
|---|---|---|---|---|
| ru-central1 | https://storage.yandexcloud.net | Россия, ru-central1 | true | STANDARD, COLD, ICE, INTELLIGENT_TIERING |

| Class | Name | RUB/GB/month (from) | Verified | Checked |
|---|---|---|---|---|
| STANDARD | Стандартное | 2.38 | false | 2026-10-10 |
| COLD | Холодное | 1.27 | false | 2026-10-10 |
| ICE | Ледяное | 0.63 | false | 2026-10-10 |
| INTELLIGENT_TIERING | Умное (нет фиксированной цены) | 0.00 | false | 2026-10-10 |

Presign maximum: 2592000 seconds; verified: false. Документация: 30 дней; нужна живая проверка. Условные PUT/complete документированы, destination Copy и условный Delete не подтверждены; защищённая запись отключена консервативно.
