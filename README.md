# Telegram Video Audit Bot

Бот принимает видео и mp3 в Telegram, делает транскрипт через AssemblyAI и отправляет аудит по правилам из `prompt.md` через OpenAI.
Результат каждой обработки сохраняется в Postgres.

## Запуск

1. Установите зависимости:

```bash
pip install -r requirements.txt
```

2. Создайте `.env` на основе `.env.example` и заполните ключи:

```env
TELEGRAM_BOT_TOKEN=...
ASSEMBLYAI_API_KEY=...
OPENAI_API_KEY=...
OPENAI_MODEL=gpt-4.1-mini
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/tg_audit
```

3. Запустите бота:

```bash
python bot.py
```

## Запуск через Docker Compose

1. Заполните `.env` (минимум `TELEGRAM_BOT_TOKEN`, `ASSEMBLYAI_API_KEY`, `OPENAI_API_KEY`).
2. Поднимите сервисы:

```bash
docker compose up --build
```

Это запустит:
- `bot` с вашим приложением
- `db` (Postgres 16) с volume для данных

В Docker-сети бот использует `DATABASE_URL=postgresql://postgres:postgres@db:5432/tg_audit`.
Значение `DATABASE_URL` из `.env` для контейнера игнорируется, чтобы не было ошибки с `localhost`.

## Как использовать

1. Откройте диалог с ботом в Telegram и отправьте видео или mp3.
2. Дождитесь статусов обработки.
3. Получите итоговый аудит в чате.

## Важно

- Файл `prompt.md` используется напрямую как архитектурный документ анализа.
- Для длинных видео обработка может занять несколько минут.
- Таблица `video_audits` создается автоматически при старте бота.
