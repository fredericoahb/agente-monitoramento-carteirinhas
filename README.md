# Agente de Monitoramento Preventivo de Carteirinhas

![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=flat&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?style=flat&logo=fastapi&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-14-000000?style=flat&logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?style=flat&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=flat&logo=typescript&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=flat&logo=postgresql&logoColor=white)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-2.0-D71F00?style=flat&logo=sqlalchemy&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.4-06B6D4?style=flat&logo=tailwindcss&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat&logo=docker&logoColor=white)
![Pydantic](https://img.shields.io/badge/Pydantic-2.9-E92063?style=flat&logo=pydantic&logoColor=white)

Sistema para monitoramento preventivo de vencimentos de carteirinhas e requisitos obrigatórios de funcionários de empresas contratadas, com alertas classificados por criticidade e dashboard executivo.

![Demo do Dashboard](docs/demo-dashboard.svg)

## Objetivo de Negócio

Antecipar riscos de impedimento de acesso de funcionários de contratadas às unidades operacionais, substituindo a gestão reativa (planilhas, avisos em cima da hora) por uma **gestão proativa e automatizada**, com alertas priorizados e ações recomendadas.

## Arquitetura da Solução

```
┌─────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│   Frontend       │────▶│   Backend API     │────▶│   PostgreSQL     │
│   Next.js/React  │◀────│   FastAPI/Python  │◀────│   Banco de dados │
│   (Vercel)       │     │   (Render/Railway)│     │   (Neon/Supabase)│
└─────────────────┘     └──────────────────┘     └──────────────────┘
```

**Fluxo:**
1. Dados de funcionários, contratos e requisitos são armazenados no PostgreSQL
2. O motor de processamento (backend) analisa todos os vencimentos
3. Classifica por criticidade: Vencido → Hoje → 7d → 15d → 30d → 60d
4. Gera alertas com ação recomendada e responsável sugerido
5. Dashboard exibe visão consolidada com filtros e gráficos

## Stack Utilizada

| Camada | Tecnologia | Motivo |
|--------|-----------|--------|
| Backend | Python + FastAPI | Performance, tipagem, docs automáticas |
| Banco | PostgreSQL | Robusto, gratuito (Neon/Supabase) |
| ORM | SQLAlchemy | Mapeamento completo, migrations |
| Validação | Pydantic | Type-safety, serialização |
| Frontend | Next.js 14 + React 18 | SSR, rotas automáticas, deploy Vercel |
| Estilo | Tailwind CSS | Utility-first, responsivo |
| Gráficos | Recharts | Leve, React-native |
| Ícones | Lucide React | Consistente, leve |

## Como o Stitch foi usado no desenho da interface

O **Stitch** (ferramenta de design de UI da Anthropic) foi utilizado para:
- Conceber o layout do dashboard principal com cards executivos e gráficos
- Definir a paleta de cores corporativa (teal/dark navy)
- Projetar o sistema de navegação lateral responsivo
- Desenhar a tabela de alertas com linhas expansíveis
- Validar a hierarquia visual da classificação de criticidade
- Criar os badges coloridos por nível de urgência

O design final foi implementado em Next.js com Tailwind CSS mantendo fidelidade ao protótipo.

## Estrutura de Pastas

```
agente-monitoramento-carteirinhas/
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py              # FastAPI app
│   │   ├── config.py            # Configurações
│   │   ├── database.py          # Conexão PostgreSQL
│   │   ├── models.py            # SQLAlchemy models
│   │   ├── schemas.py           # Pydantic schemas
│   │   ├── routers/
│   │   │   └── api.py           # Todos os endpoints
│   │   └── services/
│   │       └── processamento.py # Motor de classificação
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   │   ├── layout.tsx
│   │   │   ├── globals.css
│   │   │   ├── page.tsx              # Dashboard
│   │   │   ├── alertas/page.tsx      # Alertas detalhados
│   │   │   ├── funcionarios/page.tsx # Funcionários
│   │   │   ├── consolidacao/page.tsx # Consolidação
│   │   │   └── importacao/page.tsx   # Importação
│   │   ├── components/
│   │   │   ├── AppShell.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   ├── StatCard.tsx
│   │   │   └── CriticidadeBadge.tsx
│   │   └── lib/
│   │       └── api.ts
│   ├── package.json
│   ├── next.config.js
│   ├── tailwind.config.js
│   └── tsconfig.json
├── database/
│   ├── schema.sql
│   └── seeds.sql
├── scripts/
│   └── setup_db.sh
├── docs/
├── docker-compose.yml       # Orquestra todos os serviços
├── Makefile                 # Todos os comandos do projeto
├── .env.example
├── .gitignore
└── README.md
```

## Pré-requisitos

- **Docker Desktop** → [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
- **Make** (nativo em Linux/Mac, no Windows vem com Git Bash ou `choco install make`)

Só isso. Não precisa instalar Python, Node.js nem PostgreSQL — tudo roda dentro do Docker.

## Quick Start (Docker)

```bash
git clone https://github.com/fredericoahb/agente-monitoramento-carteirinhas.git
cd agente-monitoramento-carteirinhas

# Sobe tudo (banco + backend + frontend)
make up

# Gera os alertas (primeiro processamento)
make processar
```

Acesse: **http://localhost:3000** (Dashboard) | **http://localhost:8000/docs** (API)

### Comandos Disponíveis

```bash
make help                # Lista todos os comandos
```

| Comando | Descrição |
|---------|-----------|
| `make up` | Sobe tudo (banco + backend + frontend) |
| `make down` | Para tudo |
| `make restart` | Reinicia os serviços |
| `make reset` | Para, limpa volumes e recria do zero |
| `make build` | Rebuild das imagens Docker |
| `make clean` | Remove containers, imagens e volumes |
| `make logs` | Logs de todos os serviços (tempo real) |
| `make logs-backend` | Logs do backend |
| `make logs-frontend` | Logs do frontend |
| `make logs-db` | Logs do banco |
| `make processar` | Executa processamento de vencimentos |
| `make test-api` | Testa os principais endpoints |
| `make db-shell` | Abre psql dentro do container |
| `make db-check` | Verifica dados no banco |
| `make info` | Exibe URLs e status dos containers |
| `make deploy-info` | Instruções de deploy |

### O que o `make up` faz

1. Sobe um container **PostgreSQL 16** com o banco já criado e populado (schema + seeds)
2. Sobe o **backend FastAPI** conectado ao banco
3. Sobe o **frontend Next.js** conectado ao backend
4. Tudo pronto em ~30 segundos

## Setup Manual (sem Docker)

<details>
<summary>Clique para expandir</summary>

**Pré-requisitos:** Python 3.10+, Node.js 18+, PostgreSQL 14+

### Banco de dados
```bash
createdb carteirinhas_db
psql -d carteirinhas_db -f database/schema.sql
psql -d carteirinhas_db -f database/seeds.sql
```

### Backend
```bash
cd backend
python -m venv venv
source venv/bin/activate    # Windows: venv\Scripts\activate
pip install -r requirements.txt
echo "DATABASE_URL=postgresql://postgres:postgres@localhost:5432/carteirinhas_db" > .env
uvicorn app.main:app --reload --port 8000
```

### Frontend
```bash
cd frontend
npm install
echo "NEXT_PUBLIC_API_URL=http://localhost:8000" > .env.local
npm run dev
```

</details>

## PostgreSQL Gratuito na Nuvem

**Neon (recomendado):**
1. Crie conta em [neon.tech](https://neon.tech)
2. Crie um projeto e copie a connection string
3. Atualize `DATABASE_URL` no `backend/.env`
4. Execute os scripts via console SQL do Neon ou `psql`

**Supabase:**
1. Crie conta em [supabase.com](https://supabase.com)
2. Crie um projeto, vá em Settings > Database
3. Copie a connection string e atualize o `backend/.env`
4. Execute os scripts no SQL Editor do Supabase

## Como Testar a API

### Via Makefile (recomendado)
```bash
make test-api      # Testa health, resumo, empresas e alertas
make processar     # Executa processamento de vencimentos
```

### Via Swagger UI
Acesse http://localhost:8000/docs e teste cada endpoint interativamente.

### Via curl

```bash
# Processar vencimentos (gera alertas)
curl -X POST http://localhost:8000/api/processar

# Resumo executivo
curl http://localhost:8000/api/resumo

# Listar alertas
curl http://localhost:8000/api/alertas

# Alertas críticos
curl http://localhost:8000/api/alertas/criticos

# Consolidação por empresa
curl http://localhost:8000/api/consolidacao/empresas

# Funcionários
curl http://localhost:8000/api/funcionarios

# Requisitos de um funcionário
curl http://localhost:8000/api/funcionarios/1/requisitos
```

## Como Publicar na Vercel

1. Faça push do projeto para o GitHub
2. Acesse [vercel.com](https://vercel.com) e conecte sua conta GitHub
3. Importe o repositório `agente-monitoramento-carteirinhas`
4. Configure:
   - **Root Directory:** `frontend`
   - **Framework Preset:** Next.js
   - **Environment Variables:** `NEXT_PUBLIC_API_URL` = URL do seu backend
5. Clique em Deploy

### Deploy do Backend
O backend pode ser hospedado gratuitamente em:
- **Render** (render.com) - plano free
- **Railway** (railway.app) - trial gratuito
- **Fly.io** (fly.io) - plano free

## Como Subir no GitHub — Passo a Passo

### 1. Criar o repositório no GitHub

1. Acesse [github.com/new](https://github.com/new)
2. Preencha:
   - **Repository name:** `agente-monitoramento-carteirinhas`
   - **Description:** `Sistema de monitoramento preventivo de carteirinhas e requisitos de acesso de contratadas — FastAPI + Next.js + PostgreSQL + Docker`
   - **Visibilidade:** Private (ou Public)
   - **NÃO** marque "Add a README" (já temos)
   - **NÃO** marque "Add .gitignore" (já temos)
3. Clique em **Create repository**

### 2. Gerar um Personal Access Token (se ainda não tem)

1. No GitHub, vá em **Settings → Developer Settings → Personal Access Tokens → Tokens (classic)**
2. Clique em **Generate new token (classic)**
3. Dê um nome (ex: `meu-pc`)
4. Marque o escopo **repo** (acesso completo a repositórios)
5. Clique em **Generate token**
6. **Copie o token** (ele não será mostrado novamente)

### 3. Inicializar o Git e fazer o push

Abra o Git Bash na pasta do projeto e rode os comandos abaixo, um de cada vez:

```bash
# Entrar na pasta do projeto
cd /c/Projetos/agente-monitoramento-carteirinhas

# Inicializar o repositório Git
git init

# Adicionar todos os arquivos
git add .

# Criar o primeiro commit
git commit -m "feat: sistema completo de monitoramento de carteirinhas"

# Definir a branch principal como main
git branch -M main

# Conectar ao repositório remoto no GitHub
git remote add origin https://github.com/fredericoahb/agente-monitoramento-carteirinhas.git

# Enviar para o GitHub
git push -u origin main
```

Quando pedir credenciais:
- **Username:** `fredericoahb`
- **Password:** cole o **Personal Access Token** (não é a senha do GitHub)

### 4. Verificar

Acesse [github.com/fredericoahb/agente-monitoramento-carteirinhas](https://github.com/fredericoahb/agente-monitoramento-carteirinhas) — o projeto deve estar lá com o README e a imagem demo renderizados.

### 5. Atualizações futuras

Sempre que fizer alterações:

```bash
git add .
git commit -m "fix: descrição da alteração"
git push
```

## Endpoints da API

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/api/empresas` | Listar empresas |
| GET | `/api/contratos` | Listar contratos |
| GET | `/api/requisitos` | Listar requisitos |
| GET | `/api/funcionarios` | Listar funcionários |
| GET | `/api/funcionarios/{id}/requisitos` | Requisitos do funcionário |
| GET | `/api/alertas` | Listar alertas (com filtros) |
| GET | `/api/alertas/criticos` | Apenas alertas críticos |
| GET | `/api/resumo` | Resumo executivo |
| GET | `/api/consolidacao/empresas` | Consolidação por empresa |
| GET | `/api/consolidacao/unidades` | Consolidação por unidade |
| GET | `/api/consolidacao/contratos` | Consolidação por contrato |
| POST | `/api/processar` | Reprocessar vencimentos |
| GET | `/api/processamentos` | Histórico de processamentos |
| GET | `/api/filtros/unidades` | Unidades disponíveis |
| GET | `/api/filtros/tipos-requisito` | Tipos de requisito |

## Classificação de Criticidade

| Faixa | Criticidade | Ação |
|-------|-------------|------|
| Vencido | Crítica | Tratativa imediata |
| Vence hoje | Crítica | Ação emergencial |
| Até 7 dias | Alta | Acionar contratada |
| Até 15 dias | Média | Planejar renovação |
| Até 30 dias | Média | Agendar renovação |
| Até 60 dias | Baixa | Monitorar |
| Sem risco | OK | Nenhuma ação |

## Limitações do MVP

- Sem autenticação/autorização (implementar JWT)
- Sem upload de arquivos CSV/Excel (importação via SQL)
- Sem notificações por email/WhatsApp
- Sem histórico de alterações (audit trail completo)
- Sem integração direta com o SMS
- Processamento síncrono (sem filas/workers)

## Roadmap de Evolução

1. **v1.1** - Autenticação JWT + roles (admin, fiscal, gestor)
2. **v1.2** - Importação via CSV/Excel com validação
3. **v1.3** - Notificações por email (SendGrid/SES)
4. **v1.4** - Integração com SMS via API
5. **v2.0** - Processamento assíncrono com Celery/Redis
6. **v2.1** - Agente de IA com Google ADK + Gemini para análise
7. **v2.2** - Notificações via WhatsApp/Teams
8. **v2.3** - Relatórios PDF automatizados
9. **v3.0** - Dashboard de BI com drill-down e previsões

## Licença

Projeto interno - GTIC / TBG
