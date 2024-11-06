# From Zero to Create Self GraphRAG With LLama - 从零开始你的LLama+ GraphRAG

## 我的环境配置

## LLama 大语言模型准备

```python
pip install ollama

ollama pull mistral  #llm

ollama pull nomic-embed-text  #embedding
```

此处需要选择一个llm和一个embedding模型，这里我选择 **mistral** (4.1GB) 和 **nomic-embed-text** (278MB)

llama官网：https://ollama.com/library

```
ollama list
```

检查是否下载成功

```
ollama serve
```

启动服务

## Python[Conda] 环境准备

conda create --name RAG python=3.12

conda activate RAG

pip install ollama graphrag==0.3.6 tiktoken

如果是在我的win11的wsl的ubuntu22上graphrag直接下载是0.4.0

在工作环境初始化的时候会出现：

No module named graphrag.index.__main__; 'graphrag.index' is a package and cannot be directly executed

## GraphRAG 工作环境准备

mkdir -p ./input/markdown

python -m graphrag.index --init  --root .

## 修改包的 LLMs 调用

在运行 GraphRAG 进行索引、创建嵌入和执行本地查询之前，您必须修改位于 GraphRAG 包中的 Python 文件 `openai_embeddings_llm.py` 和 `embedding.py`。

如果不进行此修改，GraphRAG 在创建嵌入时将抛出错误，因为它不会将 "nomic-embed-text" 识别为来自 Ollama 的有效嵌入模型。

在我的设置中，这些文件位于

 `/home/username/conda/envs/RAG_agents/lib/python3.12/site-packages/graphrag/llm/openai/openai_embeddings_llm.py`

和 `/home/username/conda/envs/RAG_agents/lib/python3.12/site-packages/graphrag/query/llm/oai/embedding.py`

## 建立知识图谱

预热文档 - input - md

爸爸的爸爸叫什么，爸爸的爸爸叫爷爷，爸爸的妈妈叫什么，爸爸的妈妈叫奶奶，爸爸的哥哥叫什么，爸爸的哥哥叫伯伯，爸爸的弟弟叫什么，爸爸的弟弟叫叔叔，爸爸的姐妹叫什么，爸爸的姐妹叫姑姑，妈妈的爸爸叫什么，妈妈的爸爸叫外公。

妈妈的妈妈叫什么，妈妈的妈妈叫外婆，妈妈的兄弟叫什么，妈妈的兄弟叫舅舅，妈妈的姐妹叫什么，妈妈的姐妹叫阿姨，爷爷爸爸的爸爸叫爷爷，奶奶爸爸的妈妈叫奶奶，伯伯爸爸的哥哥叫伯伯，叔叔爸爸的弟弟叫叔叔。

姑姑爸爸的姐妹叫姑姑，外公妈妈的爸爸叫外公，外婆妈妈的妈妈叫外婆，舅舅妈妈的兄弟叫舅舅，阿姨妈妈的姐妹叫阿姨，小妹妹小妹妹，从来不淘气，衣服清洁又整齐，用功读书勤学习，爸爸妈妈都心爱，哥哥姐姐都欢喜，老师同学，说她将来了不起，小妹妹小妹妹，从来不淘气，衣服清洁又整齐，用功读书勤学习，爸爸妈妈都心爱。

python -m graphrag.index --root .

python -m graphrag.query --root . --method local "爸爸的爸爸叫什么？" #类似
