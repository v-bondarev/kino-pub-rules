# kino-pub-rules

Готовый rule-set для sing-box с доменами `kino.pub` и связанными поддоменами.

## Состав

- `kino-pub.json` — исходный source rule-set.
- `kino-pub.srs` — бинарный rule-set после компиляции.
- `.github/workflows/update-and-release.yml` — GitHub Actions для автоматического обновления, сборки и публикации `.srs` в Release `latest`.

## Структура rule-set

Используется source format sing-box с `version` и `rules`, который затем компилируется в бинарный `.srs` через `sing-box rule-set compile`.

## Локальная сборка

```bash
sing-box rule-set compile --output kino-pub.srs kino-pub.json
```

## Постоянная ссылка на `.srs`

Workflow публикует свежий `kino-pub.srs` в Release `latest`, перезаписывая asset при каждом обновлении:

```text
https://github.com/v-bondarev/kino-pub-rules/releases/download/latest/kino-pub.srs
```

## Пример подключения в sing-box

```json
{
  "route": {
    "rule_set": [
      {
        "tag": "kino-pub",
        "type": "remote",
        "format": "binary",
        "url": "https://github.com/v-bondarev/kino-pub-rules/releases/download/latest/kino-pub.srs",
        "download_detour": "direct"
      }
    ],
    "rules": [
      {
        "rule_set": "kino-pub",
        "outbound": "proxy"
      }
    ]
  }
}
```

## Обновление

1. Запусти workflow вручную через `Run workflow` или дождись расписания.
2. GitHub Actions обновит `kino-pub.json`, соберёт `kino-pub.srs` и выложит его по той же ссылке в Release `latest`.
