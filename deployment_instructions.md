# Instruções de Deployment do Portal de Estudos ESPRO

Este documento contém todas as informações e passos necessários para você fazer o deploy do Portal de Estudos da ESPRO em sua própria máquina ou em serviços de hospedagem na nuvem, garantindo acesso e controle total.

## 1. Estrutura do Projeto

O projeto é dividido em duas partes principais:

-   **Frontend:** `portal-estudos-espro/` (Aplicação React)
-   **Backend:** `portal-backend-espro/` (API Flask com banco de dados SQLite/PostgreSQL)

## 2. Requisitos

Para rodar o projeto localmente ou fazer o deploy, você precisará:

-   **Node.js e npm/pnpm:** Para o frontend (React).
-   **Python 3.x e pip:** Para o backend (Flask).
-   **Git:** Para clonar o repositório (se for o caso).
-   **Conta em serviços de deploy:** (Opcional, mas recomendado para produção) Netlify/Vercel para o frontend e Railway/Render para o backend.

## 3. Deploy do Backend (API Flask)

O backend é responsável pela lógica de negócio, autenticação, gerenciamento de usuários, módulos, aulas, exercícios e progresso. Ele utiliza Flask e SQLAlchemy com SQLite (para desenvolvimento) ou PostgreSQL (para produção).

### 3.1. Configuração Local

1.  **Navegue até a pasta do backend:**
    ```bash
    cd portal-backend-espro
    ```
2.  **Crie e ative o ambiente virtual:**
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```
3.  **Instale as dependências:**
    ```bash
    pip install -r requirements.txt
    ```
    *   **Nota:** Se o `requirements.txt` não existir ou estiver desatualizado, gere-o primeiro:
        ```bash
        pip freeze > requirements.txt
        ```
4.  **Popule o banco de dados (dados iniciais):**
    ```bash
    python seed_data.py
    ```
5.  **Inicie o servidor Flask:**
    ```bash
    python src/main.py
    ```
    O backend estará rodando em `http://localhost:5000`.

### 3.2. Deploy em Produção (Ex: Railway.app ou Render.com)

Para deploy em produção, é altamente recomendado usar um banco de dados PostgreSQL.

1.  **Crie um repositório Git:**
    ```bash
    git init
    git add .
    git commit -m "Initial commit"
    git branch -M main
    git remote add origin <URL_DO_SEU_REPOSITORIO_GIT>
    git push -u origin main
    ```
2.  **Crie um novo serviço no Railway/Render:**
    -   Conecte seu repositório Git.
    -   Configure as variáveis de ambiente:
        -   `DATABASE_URL`: URL de conexão com seu banco de dados PostgreSQL (fornecido pelo Railway/Render).
        -   `JWT_SECRET`: Uma chave secreta forte para JWT (ex: `openssl rand -hex 32`).
    -   O Railway/Render detectará automaticamente o `requirements.txt` e o `Procfile` (se necessário) para rodar a aplicação.
    -   **Comando de Build:** `pip install -r requirements.txt`
    -   **Comando de Start:** `gunicorn --bind 0.0.0.0:$PORT src.main:app` (instale `gunicorn` no `requirements.txt`)
3.  **Popule o banco de dados em produção:** Após o deploy do backend, você precisará executar o `seed_data.py` no ambiente de produção. Muitos serviços de deploy permitem executar comandos pós-deploy ou via console.

## 4. Deploy do Frontend (Aplicação React)

O frontend é a interface do usuário do portal. Ele se comunica com o backend através da API.

### 4.1. Configuração Local

1.  **Navegue até a pasta do frontend:**
    ```bash
    cd portal-estudos-espro
    ```
2.  **Instale as dependências:**
    ```bash
    pnpm install # ou npm install
    ```
3.  **Inicie o servidor de desenvolvimento:**
    ```bash
    pnpm run dev # ou npm run dev
    ```
    O frontend estará rodando em `http://localhost:5173` (ou outra porta).

### 4.2. Deploy em Produção (Ex: Netlify ou Vercel)

1.  **Crie um repositório Git:**
    ```bash
    cd .. # Volte para a pasta raiz do projeto
    git init
    git add .
    git commit -m "Initial commit"
    git branch -M main
    git remote add origin <URL_DO_SEU_REPOSITORIO_GIT>
    git push -u origin main
    ```
2.  **Crie um novo site no Netlify/Vercel:**
    -   Conecte seu repositório Git.
    -   **Configurações de Build:**
        -   **Build Command:** `pnpm build` (ou `npm run build`)
        -   **Publish directory:** `dist`
    -   **Variáveis de Ambiente:**
        -   `VITE_API_URL`: A URL pública do seu backend (ex: `https://seu-backend.railway.app/api`).
            *   **Importante:** Você precisará alterar a URL do backend no código do frontend (`src/App.jsx` e `src/contexts/AuthContext.jsx`) de `http://localhost:5000` para a URL pública do seu backend antes de fazer o build para produção.

## 5. Testes Finais

Após o deploy de ambos (frontend e backend), acesse a URL pública do seu frontend e:

-   **Cadastre um novo usuário.**
-   **Faça login.**
-   **Navegue pelos módulos e aulas.**
-   **Tente completar os exercícios** (o progresso deve ser salvo).
-   **Verifique o progresso** no dashboard.

## 6. Acesso e Controle Total

Com o deploy realizado, você terá acesso total ao código-fonte e controle sobre a infraestrutura de hospedagem. Isso permite:

-   **Atualizações:** Fazer modificações no código e redeployar facilmente.
-   **Escalabilidade:** Ajustar os recursos de hospedagem conforme a demanda.
-   **Personalização:** Adaptar o portal às necessidades específicas da ESPRO.

Qualquer dúvida durante o processo de deploy, estou à disposição!

