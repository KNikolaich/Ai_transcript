# 📞 Call Analysis API (FastAPI + OpenAI)

API-сервис для анализа диалогов (звонков, переговоров, поддержки).  
Поддерживает:
- 📄 текстовые диалоги
- 🎧 аудиофайлы (с автоматической расшифровкой)
- 📊 анализ по заданным критериям
- 🧠 глубокий AI-анализ разговора

---

## 🚀 Основной функционал

### 1. Принимает вход:
- Текст диалога  
**или**
- Аудиофайл (mp3, wav, m4a и др.)

### 2. Если передан аудиофайл:
- Конвертирует в WAV (через ffmpeg)
- Распознаёт речь (OpenAI STT)
- Разделяет по спикерам

### 3. Выполняет анализ:
- По списку критериев (если переданы)
- Общий разбор разговора:
  - цель диалога
  - сильные/слабые стороны
  - ошибки
  - рекомендации
  - альтернативные формулировки

---

## 🔌 API Endpoint

### POST `/analyze`

### 📥 Вход (2 варианта)

#### JSON:
```json
{
  "text": "текст диалога",
  "criteria": ["критерий 1", "критерий 2"]
}
```
---
### Multipart (для аудио):
- file — аудиофайл
- criteria — строка или JSON-массив
####📤 Ответ:
```{
  "status": "ok",
  "analysis": "текст анализа..."
}
```
---
⚙️ Переменные окружения

Обязательно:
```
UI_OPENAI_KEY=your_openai_api_key
```
---

### ☁️ Деплой на Render (Web Service)
1. Подготовка репозитория

В репозитории должно быть:

```
app.py
requirements.txt
```

### 2. Создание сервиса
- Перейди в 👉 https://render.com
- Нажми "New +" → Web Service
- Подключи GitHub-репозиторий
  
### 3. Настройки сервиса

Environment:

Python 3

Build Command:
```
pip install -r requirements.txt
```
Start Command:
```
uvicorn app:app --host 0.0.0.0 --port $PORT
```
### 4. Добавь переменную окружения

В разделе Environment Variables:
```
Key: UI_OPENAI_KEY
Value: your_openai_api_key
```
### 5. Важно ⚠️ (для аудио)

Render не всегда имеет ffmpeg по умолчанию.

👉 Добавь в Build Command:
```
apt-get update && apt-get install -y ffmpeg && pip install -r requirements.txt
```
### 6. Деплой

Нажми Deploy — после сборки ты получишь URL:
```
https://your-app.onrender.com/analyze
```

## 🔗 Пример использования

```
curl -X POST https://your-app.onrender.com/analyze \
  -F "file=@audio.mp3"
```
или
```
curl -X POST https://your-app.onrender.com/analyze \
  -H "Content-Type: application/json" \
  -d '{"text": "пример диалога"}'
```

### ⚠️ Ограничения

Требуется валидный OpenAI API ключ
Скорость зависит от длины аудио
Большие файлы могут обрабатываться дольше

### 💡 Применение
анализ звонков продаж
контроль качества поддержки
разбор переговоров
обучение менеджеров

### ✅ Готово

После деплоя у тебя будет публичный API, который можно подключить к:

веб-интерфейсу
CRM
Telegram-боту
любому фронту
