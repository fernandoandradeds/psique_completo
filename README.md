# Psique IA — Assistente de Saúde Mental com IA

Sistema web para psicólogos que combina **Django**, **LangChain** e **FAISS** para análise inteligente de transcrições de consultas psicológicas. O assistente responde perguntas sobre o histórico do paciente com base em busca semântica sobre as sessões gravadas.

## Funcionalidades

- **Gravação de consultas** — upload de vídeos com transcrição automática
- **Análise de humor** — avaliação do estado emocional do paciente por sessão (1–5)
- **Resumos automáticos** — geração de resumos estruturados de cada consulta via LLM
- **RAG por paciente** — busca semântica no histórico individual usando FAISS + OpenAI Embeddings
- **Chat com o histórico** — psicólogo faz perguntas em linguagem natural e recebe respostas contextualizadas
- **Filtro por data** — buscas como "o que foi discutido na consulta de 15/03?"

## Arquitetura

```
psique_completo/
├── consultas/
│   ├── agents.py          # SummaryAgent, EvaluationAgent, RAGContext (LangChain)
│   ├── models.py          # Gravacoes, DataTreinamento, Pergunta
│   ├── tasks.py           # Tarefas assíncronas (Django-Q)
│   ├── signals.py         # Triggers pós-save
│   └── views.py           # Endpoints da aplicação
├── usuarios/
│   └── models.py          # Pacientes, autenticação
├── faiss_banco/           # Índices FAISS por paciente
├── prompts/
│   └── prompts.py         # Prompts: PSI_PROMPT, SUMMARY_PROMPT, EVALUATION_PROMPT
└── core/
    └── settings.py        # Configurações Django
```

## Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat&logo=langchain&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat&logo=openai&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-0467DF?style=flat)

## Como executar

### Pré-requisitos
- Python 3.11+
- Chave de API da OpenAI

### Setup

```bash
git clone https://github.com/fernandoandradeds/psique_completo.git
cd psique_completo

python -m venv venv
venv\Scripts\activate      # Windows
# source venv/bin/activate  # Linux/Mac

pip install -r requirements.txt
```

### Variáveis de ambiente

Crie um arquivo `.env` na raiz:

```env
OPENAI_API_KEY=sua-chave-aqui
```

### Executar

```bash
python manage.py migrate
python manage.py createsuperuser
python manage.py qcluster &   # worker para tarefas assíncronas
python manage.py runserver
```

Acesse `http://127.0.0.1:8000`

## Fluxo de uso

1. Cadastrar paciente
2. Fazer upload do vídeo/áudio da consulta
3. O sistema transcreve, gera resumo e avalia o humor automaticamente (via Django-Q)
4. Na aba de consultas, fazer perguntas sobre o histórico do paciente

## Autor

**Fernando Andrade** — [GitHub](https://github.com/fernandoandradeds) · [LinkedIn](https://www.linkedin.com/in/fernandoandradeds/)
