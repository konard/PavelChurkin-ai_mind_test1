# Варианты настройки OpenAI API

В этом документе собраны все возможные параметры настройки OpenAI API, используемого в проекте (`ai_mind1.py`, `ai_mind3.py`).

---

## 1. Инициализация клиента (`AsyncOpenAI`)

```python
from openai import AsyncOpenAI

client = AsyncOpenAI(
    api_key="sk-...",                          # API-ключ (обязательно)
    base_url="https://api.openai.com/v1",      # URL endpoint (можно заменить на прокси)
    organization="org-...",                    # ID организации (опционально)
    project="proj-...",                        # ID проекта (опционально)
    timeout=60.0,                              # Таймаут запроса в секундах
    max_retries=2,                             # Кол-во повторных попыток при ошибке
    default_headers={"X-Custom": "value"},     # Доп. заголовки для всех запросов
    default_query={"param": "value"},          # Доп. query-параметры
    http_client=None,                          # Кастомный httpx-клиент
)
```

**Используется в проекте:**
```python
client = AsyncOpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
    base_url="https://api.proxyapi.ru/openai/v1"  # Прокси вместо официального API
)
```

---

## 2. Выбор модели (`model`)

Параметр `model` в `chat.completions.create(...)` определяет, какая языковая модель будет использована.

| Модель | Описание | Контекст |
|--------|----------|----------|
| `gpt-3.5-turbo` | Быстрая и дешёвая (используется сейчас) | 16 385 токенов |
| `gpt-3.5-turbo-0125` | Последняя версия 3.5 | 16 385 токенов |
| `gpt-4o` | GPT-4 Omni — флагман (быстрее и дешевле gpt-4) | 128 000 токенов |
| `gpt-4o-mini` | Лёгкая версия GPT-4o | 128 000 токенов |
| `gpt-4-turbo` | GPT-4 Turbo с vision | 128 000 токенов |
| `gpt-4` | Оригинальный GPT-4 | 8 192 токена |
| `o1` | Модель с цепочкой рассуждений (reasoning) | 200 000 токенов |
| `o1-mini` | Лёгкая reasoning-модель | 128 000 токенов |

**Рекомендация для проекта:**
- `gpt-4o-mini` — хороший баланс цены/качества для анализа состояний
- `gpt-4o` — максимальное качество распознавания эмоций

---

## 3. Параметры генерации текста

```python
response = await client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[...],

    # --- Параметры случайности и разнообразия ---
    temperature=0.7,         # 0.0–2.0: 0 = детерминировано, 2 = очень случайно
    top_p=1.0,               # 0.0–1.0: nucleus sampling (альтернатива temperature)
    # Используйте либо temperature, либо top_p — не оба сразу

    # --- Ограничение вывода ---
    max_tokens=500,          # Макс. кол-во токенов в ответе
    max_completion_tokens=500, # Аналог max_tokens (новое название)

    # --- Повторяемость ---
    frequency_penalty=0.0,   # -2.0–2.0: штраф за частые повторы токенов
    presence_penalty=0.0,    # -2.0–2.0: штраф за повторное появление тем

    # --- Несколько вариантов ответа ---
    n=1,                     # Кол-во вариантов ответа (по умолчанию 1)

    # --- Формат ответа ---
    response_format={"type": "json_object"},  # Принудительный JSON-вывод
    # или: response_format={"type": "text"}   # Обычный текст (по умолчанию)

    # --- Стоп-последовательности ---
    stop=["###", "\n\n"],    # Список строк, при появлении которых генерация прекращается

    # --- Воспроизводимость ---
    seed=42,                 # Фиксирует случайность (для воспроизводимых результатов)

    # --- Потоковый вывод ---
    stream=False,            # True = выдаёт токены по одному (стриминг)

    # --- Логиты ---
    logit_bias={50256: -100},# Изменение вероятности конкретных токенов
    logprobs=False,          # Возвращать ли логарифмические вероятности токенов
    top_logprobs=None,       # Кол-во топ-токенов с logprobs (1–20)

    # --- Инструменты (function calling / tools) ---
    tools=[...],             # Список инструментов для вызова функций
    tool_choice="auto",      # "auto" | "none" | {"type": "function", "function": {"name": "..."}}

    # --- Пользователь ---
    user="user-123",         # Идентификатор пользователя (для модерации)

    # --- Дополнительно ---
    timeout=30.0,            # Таймаут конкретного запроса
)
```

---

## 4. Структура сообщений (`messages`)

```python
messages = [
    # Системный промпт — задаёт роль/поведение бота
    {"role": "system", "content": "Ты психологический аналитик."},

    # Сообщение пользователя
    {"role": "user", "content": "Ты опять ошибся"},

    # Ответ ассистента (для многоходового диалога)
    {"role": "assistant", "content": "Мне жаль, что так получилось."},

    # Результат вызова инструмента (при использовании tools)
    {"role": "tool", "content": "...", "tool_call_id": "call_abc123"},
]
```

---

## 5. Конфигурация через переменные окружения (`.env`)

```dotenv
# Обязательно
OPENAI_API_KEY=sk-...

# Опционально — используются автоматически библиотекой openai
OPENAI_BASE_URL=https://api.proxyapi.ru/openai/v1
OPENAI_ORG_ID=org-...
OPENAI_PROJECT_ID=proj-...
OPENAI_TIMEOUT=60
OPENAI_MAX_RETRIES=2
```

---

## 6. Прокси и альтернативные endpoint'ы

Текущий проект использует `https://api.proxyapi.ru/openai/v1`. Список совместимых альтернатив:

| Сервис | `base_url` | Примечание |
|--------|------------|------------|
| OpenAI (официальный) | `https://api.openai.com/v1` | Оригинальный |
| ProxyAPI (текущий) | `https://api.proxyapi.ru/openai/v1` | Российский прокси |
| Azure OpenAI | `https://<resource>.openai.azure.com/` | Требует `AzureOpenAI` клиент |
| Ollama (локально) | `http://localhost:11434/v1` | Локальные модели |
| LM Studio | `http://localhost:1234/v1` | Локальные модели |
| Together AI | `https://api.together.xyz/v1` | Дешевле для больших объёмов |
| Groq | `https://api.groq.com/openai/v1` | Очень быстрый inference |

---

## 7. Рекомендуемые настройки для задач проекта

### Анализ эмоциональных состояний (`analyze_user_states`)
```python
model="gpt-4o-mini"
temperature=0.3        # Низкая для точного анализа
max_tokens=200
response_format={"type": "json_object"}
```

### Генерация эмоционального ответа (`generate_response`)
```python
model="gpt-4o-mini"
temperature=0.8        # Выше для естественности и разнообразия
max_tokens=150
frequency_penalty=0.5  # Избегаем повторений в ответах
```

### Повторный анализ весов (`delayed_analysis`)
```python
model="gpt-3.5-turbo"  # Дешевле — задача менее критична
temperature=0.2
max_tokens=150
response_format={"type": "json_object"}
```

### Спонтанный ответ (`generate_spontaneous_response`)
```python
model="gpt-4o-mini"
temperature=1.0        # Максимальная спонтанность
max_tokens=100
presence_penalty=0.6   # Новые темы вместо повторений
```

---

## 8. Параметры экономии токенов и бюджета

```python
# Подсчёт токенов в ответе
response.usage.prompt_tokens      # Токены в запросе
response.usage.completion_tokens  # Токены в ответе
response.usage.total_tokens       # Итого

# Ограничение стоимости
max_tokens=100  # Чем меньше, тем дешевле (но может обрезать ответ)

# Более дешёвые модели в порядке возрастания стоимости:
# gpt-3.5-turbo < gpt-4o-mini < gpt-4o < gpt-4-turbo < gpt-4
```

---

## 9. Обработка ошибок

```python
from openai import (
    APIConnectionError,    # Нет соединения с сервером
    APITimeoutError,       # Превышен таймаут
    AuthenticationError,   # Неверный API-ключ
    BadRequestError,       # Ошибка в запросе (400)
    RateLimitError,        # Превышен лимит запросов (429)
    InternalServerError,   # Ошибка сервера OpenAI (500)
)

try:
    response = await client.chat.completions.create(...)
except RateLimitError:
    # Подождать и повторить
    await asyncio.sleep(60)
except AuthenticationError:
    # Проверить OPENAI_API_KEY
    pass
except APITimeoutError:
    # Увеличить timeout или повторить
    pass
```

---

## 10. Краткая сводка: что менять в первую очередь

| Параметр | Текущее значение | Альтернатива | Эффект |
|----------|-----------------|--------------|--------|
| `model` | `gpt-3.5-turbo` | `gpt-4o-mini` | Лучшее качество за сопоставимую цену |
| `temperature` | `0.7` | `0.3` (анализ) / `1.0` (генерация) | Точность vs. творчество |
| `max_tokens` | не задан | `100–300` | Контроль длины и стоимости |
| `base_url` | `proxyapi.ru` | `api.openai.com/v1` | Прямой доступ к OpenAI |
| `response_delay` | `5 сек` | `2–10 сек` | Частота повторного анализа |
| `spontaneous_threshold` | `1.0` | `0.7–0.9` | Чаще/реже спонтанные ответы |
| `increase_state_weight` amount | `0.2` | `0.1–0.4` | Скорость накопления состояний |
