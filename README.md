# n8n RAG Knowledge Assistant

RAG-ассистент на n8n с OpenAI и двумя вариантами векторного хранилища: Pinecone и Supabase.

## Что внутри

- `workflows/01-knowledge-base-ingestion.json` — загрузка фрагментов базы знаний, генерация embeddings и запись в Pinecone.
- `workflows/02-rag-retrieval-chat.json` — чат с AI Agent, который использует Pinecone как retrieval tool.
- `workflows/03-supabase-rag.json` — альтернативная RAG-реализация на Supabase Vector Store: ingestion + retrieval + Simple Memory.
- `sample-data/return-policy-example.md` — синтетическая тестовая база знаний.
- `docs/architecture.md` — описание архитектуры и важных настроек.

## Основная архитектура — Pinecone

```text
Документы → OpenAI Embeddings → Pinecone

Вопрос пользователя → AI Agent → Pinecone retrieval → OpenAI Chat Model → ответ по базе знаний
```

Важно: ingestion и retrieval должны использовать одинаковые Pinecone index и namespace.

В опубликованных Pinecone-workflow используются демонстрационные значения:

```text
Index: rag-knowledge-base
Namespace: demo
```

## Альтернативная реализация — Supabase

В репозитории также есть вариант RAG на Supabase Vector Store.

```text
Manual Trigger
  → Default Data Loader
  → OpenAI Embeddings
  → Supabase Vector Store

Chat Trigger
  → AI Agent
     ├─ OpenAI Chat Model
     ├─ Simple Memory
     └─ Supabase Vector Store (retrieval tool)
```

В этой версии ingestion и retrieval собраны в одном workflow. Для векторного поиска используется таблица `documents` и функция `match_documents`.

## Возможности

- RAG в n8n
- Pinecone Vector Store
- Supabase Vector Store
- OpenAI Embeddings
- AI Agent tool calling
- vector search
- metadata для документов
- разделение ingestion и retrieval
- вариант с conversational memory
- ограничение ответов данными из базы знаний

## Запуск Pinecone-варианта

1. Создать Pinecone index.
2. Импортировать `01-knowledge-base-ingestion.json` и `02-rag-retrieval-chat.json` в n8n.
3. Подключить собственные OpenAI и Pinecone credentials в n8n.
4. Проверить одинаковые index/namespace в обоих workflow.
5. Запустить ingestion.
6. Открыть Retrieval Chat и задать вопрос по загруженным данным.

Пример:

```text
Какой порядок действий оператора при возврате товара и какие сроки указаны в регламенте?
```

## Запуск Supabase-варианта

1. Подготовить Supabase проект с поддержкой `pgvector`.
2. Создать таблицу `documents` и функцию поиска `match_documents`.
3. Импортировать `03-supabase-rag.json` в n8n.
4. Подключить OpenAI и Supabase credentials через n8n Credentials UI.
5. Запустить ingestion-ветку для загрузки тестовой базы.
6. Открыть чат и проверить retrieval по загруженным данным.

Если нужной информации в базе нет, агент должен явно сообщить, что данных недостаточно, а не придумывать ответ.

## Публикация

Workflow перед публикацией обезличены: credential references, webhook/instance-specific идентификаторы и внутренние ID удалены. Тестовые данные синтетические.

## Stack

**n8n · OpenAI · Pinecone · Supabase · PostgreSQL/pgvector · RAG · AI Agents · Vector Search · Embeddings**

## Author

Andrei Blagov  
https://github.com/Andrei-Blagov
