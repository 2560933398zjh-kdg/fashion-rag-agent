# RAGAgent — 服装领域 RAG 智能客服

基于 **LangChain + 通义千问 + ChromaDB** 构建的 RAG（检索增强生成）智能客服系统，知识库为服装商品领域资料（尺码推荐、洗涤养护、颜色选择），通过 Streamlit 提供问答界面，支持文件上传扩充知识库。

## 项目简介

系统将服装知识文档切分后向量化存入 ChromaDB，用户提问时检索相关片段，结合对话历史由通义千问生成回答。面向电商服装客服场景，帮助解答尺码、养护、配色等常见问题。

## 技术栈

- Python + Streamlit（Web 界面）
- LangChain（RAG 链路编排）
- ChromaDB（向量数据库）
- DashScope Embedding（text-embedding-v4）+ ChatTongyi 通义千问对话模型（qwen3-max）

## 目录结构

```
RAGAgent/
├── RAG/
│   ├── rag.py                    # RagService 核心：检索 + 对话历史 + 生成
│   ├── vector_stores.py          # ChromaDB 向量库服务（增删查）
│   ├── knowledge_base.py         # 知识库构建（文档切分、入库）
│   ├── file_history_store.py     # 对话历史存取
│   ├── config_data.py            # 配置（模型名、切分参数、向量库路径）
│   ├── app_qa.py                 # Streamlit 问答界面（streamlit run app_qa.py）
│   ├── app_file_uploader.py      # 文件上传扩展知识库界面
│   └── data/                     # 知识库原始资料
│       ├── 尺码推荐.txt
│       ├── 洗涤养护.txt
│       └── 颜色选择.txt
├── chroma_db/                    # 向量库持久化（chroma.sqlite3）
└── chat_history/                 # 会话历史存储（user_001）
```

## 运行方式

```bash
# 构建知识库（首次运行）
python -c "from RAG.knowledge_base import *; ..."   # 见 knowledge_base.py 说明

# 启动问答界面
cd RAGAgent
streamlit run RAG/app_qa.py

# 启动文件上传界面（扩充知识库）
streamlit run RAG/app_file_uploader.py
```

## 配置说明

`RAG/config_data.py` 可配置：

| 参数 | 默认值 | 说明 |
| --- | --- | --- |
| `collection_name` | rag | Chroma 集合名 |
| `persist_directory` | ./chroma_db | 向量库持久化路径 |
| `chunk_size` / `chunk_overlap` | 1000 / 100 | 文本切分参数 |
| `embedding_model_name` | text-embedding-v4 | Embedding 模型 |
| `chat_model_name` | qwen3-max | 对话模型 |
| `session_config` | user_001 | 会话 ID（历史隔离） |

模型调用依赖 DashScope API Key，需在环境变量或代码中配置。
