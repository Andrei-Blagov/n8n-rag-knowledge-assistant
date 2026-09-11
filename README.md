# n8n RAG Knowledge Assistant

RAG-ассистент на n8n с OpenAI и Pinecone.

## Что внутри

- `workflows/01-knowledge-base-ingestion.json` — загрузка фрагментов базы знаний, генерация embeddings и запись в Pinecone.
- `workflows/02-rag-retrieval-chat.json` — чат с AI Agent, который использует Pinecone как retrieval tool.
- `sample-data/return-policy-example.md` — синтетическая тестовая база знаний.
- `docs/architecture.md` — описание архитектуры и важных настроек.

## Архитектура

```text
Документы → OpenAI Embeddings → Pinecone

Вопрос пользователя → AI Agent → Pinecone retrieval → OpenAI Chat Model → ответ по базе знаний
```

Важно: ingestion и retrieval должны использовать одинаковые Pinecone index и namespace.

В опубликованных workflow используются демонстрационные значения:

```text
Index: rag-knowledge-base
Namespace: demo
```

## Возможности

- RAG в n8n
- Pinecone Vector Store
- OpenAI Embeddings
- AI Agent tool calling
- metadata для документов
- разделение ingestion и retrieval
- ограничение ответов данными из базы знаний

## Запуск

1. Создать Pinecone index.
2. Импортировать оба workflow в n8n.
3. Подключить собственные OpenAI и Pinecone credentials в n8n.
4. Проверить одинаковые index/namespace в обоих workflow.
5. Запустить ingestion.
6. Открыть Retrieval Chat и задать вопрос по загруженным данным.

Пример:

```text
Какой порядок действий оператора при возврате товара и какие сроки указаны в регламенте?
```

Если нужной информации в базе нет, агент должен явно сообщить, что данных недостаточно, а не придумывать ответ.

## Публикация

Workflow перед публикацией обезличены: ссылки на credentials и instance-specific идентификаторы удалены. Тестовые регламенты синтетические.

## Stack

**n8n · OpenAI · Pinecone · RAG · AI Agents · Vector Search · Embeddings**

## Author

Andrei Blagov  
https://github.com/Andrei-Blagov
