# 📱 App Acabamento - DCJ Uniformes

Sistema web de cronometragem de produção para o setor de Acabamento da DCJ Uniformes.

## 🎯 Funcionalidades

### 👤 Para Operadores
- ⏱️ **Timer de Cronometragem**: Inicie e finalize atividades com precisão
- 📊 **Controle de Produção**: Registre quantidades realizadas e refugos
- 📱 **Mobile-First**: Interface otimizada para tablets e smartphones
- 🔐 **Login Rápido**: Matrícula + PIN de 4 dígitos

### 👨‍💼 Para Gestores/Admin
- 📋 **Gestão de OFs**: Criar, editar e deletar Ordens de Fabricação
- 📄 **Importação em Lote**: Importar OFs direto do PDF do Sisplan
- 📈 **Dashboard**: Visualizar estatísticas de produção
- 👥 **Gestão de Usuários**: Cadastrar operadores e processos
- ⚙️ **Gestão de Processos**: Configurar etapas de produção

## 🏗️ Arquitetura

### Backend (Node.js + Express)
- **Framework**: Express.js
- **Database**: Supabase (PostgreSQL)
- **Autenticação**: JWT
- **Validação**: Express-validator
- **Upload**: Multer (para PDFs)
- **Parsing**: pdf-parse (extração de dados de PDF)

### Frontend (React + Vite)
- **Framework**: React 19
- **Roteamento**: React Router v7
- **HTTP Client**: Axios
- **Estilização**: Tailwind CSS
- **Ícones**: Lucide React
- **Build**: Vite

### Database (Supabase/PostgreSQL)
Tabelas principais:
- `users` - Usuários do sistema
- `processes` - Processos de produção
- `ofs` - Ordens de Fabricação
- `activities` - Atividades cronometradas

## 🚀 Instalação Local

### Pré-requisitos
- Node.js >= 18.0.0
- npm ou yarn
- Conta no Supabase

### Backend
```bash
cd backend
npm install
cp .env.example .env
# Edite .env com suas credenciais do Supabase
npm run dev
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

Acesse: http://localhost:5173

## 📦 Deploy em Produção

Siga o guia completo em: [DEPLOY_GUIDE.md](./DEPLOY_GUIDE.md)

**Resumo:**
- Backend: Railway (grátis)
- Frontend: Vercel (grátis)
- Database: Supabase (grátis)

## 🔒 Segurança

- ✅ Autenticação JWT
- ✅ Bcrypt para senhas
- ✅ Rate limiting
- ✅ Helmet para headers de segurança
- ✅ CORS configurado
- ✅ Validação de inputs
- ✅ Sanitização de dados

## 📊 Estrutura do Projeto

```
acabamento-app/
├── backend/
│   ├── src/
│   │   ├── config/         # Configurações (DB, etc)
│   │   ├── middlewares/    # Auth, rate limit, etc
│   │   ├── routes/         # Rotas da API
│   │   └── server.js       # Entrada do servidor
│   ├── migrations/         # Scripts SQL
│   └── package.json
│
├── frontend/
│   ├── src/
│   │   ├── pages/          # Páginas (Login, Timer, Admin)
│   │   ├── services/       # API client (Axios)
│   │   └── App.jsx         # Componente principal
│   └── package.json
│
└── DEPLOY_GUIDE.md        # Guia de deploy
```

## 🎨 Screenshots

### Login
Interface simples com matrícula e PIN de 4 dígitos.

### Timer (Operadores)
Cronômetro digital grande, fácil de ler mesmo de longe. Formulário intuitivo para quantidade realizada e refugos.

### Admin (Gestores)
Dashboard com estatísticas, tabela de OFs com filtros, importação de PDF do Sisplan.

## 📝 Fluxo de Trabalho

1. **Operador faz login** (matrícula + PIN)
2. **Seleciona processo e OF**
3. **Inicia cronometragem**
4. **Realiza o trabalho**
5. **Finaliza** informando quantidade realizada e refugos
6. **Sistema calcula TPU** (Tempo Por Unidade)
7. **Dados salvos** para relatórios futuros

## 🔧 Tecnologias

- **Node.js** 18+
- **Express** 4.18
- **React** 19
- **Vite** 7
- **Supabase** (PostgreSQL)
- **Tailwind CSS** 3.4
- **Axios** 1.12
- **JWT** 9.0
- **Multer** 2.0
- **pdf-parse** 1.1

## 📄 Licença

MIT License - DCJ Uniformes

## 👨‍💻 Desenvolvimento

Desenvolvido para DCJ Uniformes - Setor de Acabamento

**Contato**: regi@dcjuniformes.com.br

---

**🎉 Sistema em produção desde Out/2024**
