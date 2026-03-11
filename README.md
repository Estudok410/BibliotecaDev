# biblioteca-emprestimos-cloud
Sistema de empréstimos de livros em nuvem para a disciplina de Desenvolvimento de Software em Nuvem (ADS/IA - Unifor), grupo 35.

## Estrutura do repositório

- `backend/` – API Node/Express com Prisma (PostgreSQL).
- `frontend/` – Single‑page application criada com Create React App.

## Preparar para desenvolvimento

1. **Backend**
   ```bash
   cd backend
   cp .env.example .env          # e preencha as variáveis (DATABASE_URL, JWT_KEY, ...)
   npm install                   # instala dependências
   npx prisma migrate dev        # aplica migrations no banco local
   npm run dev                   # inicia servidor em http://localhost:3001
   ```

2. **Frontend**
   ```bash
   cd frontend
   npm install                   # instala dependências
   npm start                     # inicia o app em http://localhost:3000
   ```

3. **Testes**
   - Backend: `cd backend && npm test` (usa Jest).
   - Frontend: `cd frontend && npm test`.

## Build para produção

- Frontend: `cd frontend && npm run build` — gera a pasta `build/`.
- Backend não tem etapa de build; apenas assegure que `npm install` foi executado.

## Deploy no Vercel

1. Faça commit de todo o código‑fonte (não inclua `node_modules` ou a pasta `build/`, que já está no `.gitignore`).
2. Configure a aplicação no Vercel:
   - Defina o **build command** como `cd frontend && npm run build` (já está em `vercel.json`).
   - Adicione um campo `homepage` em `frontend/package.json` (por exemplo, "/"); isso orienta o CRA a gerar URLs relativas adequadas e evita avisos durante o build.
   - No `vercel.json` já há uma regra de proxy para `/api/*` e uma regra de fallback que envia todas as outras rotas para `index.html`. Se o backend estiver em outro domínio, ajuste a URL do proxy; mantenha a regra de SPA para que o roteamento do React Router funcione corretamente.
   - Caso planeje hospedar a API no mesmo monorepo, considere mover o código para uma pasta `api/` ou configurar funções serverless. Por enquanto o rewrite assume que a API está em um domínio separado.
3. Configure as variáveis de ambiente no dashboard do Vercel: `DATABASE_URL`, `JWT_KEY`, etc.
4. Após push para o branch `main`, o Vercel irá construir automaticamente o frontend usando as dependências instaladas.

> **Observação:** o `build/` do frontend não deve ser versionado – deixe o Vercel gerar em cada deploy.

## Notas adicionais

- O arquivo `vercel.json` já inclui cabeçalhos de segurança HTTP recomendados.
- O `.gitignore` cobre as pastas e arquivos gerados pelo `npm` e pelo CRA.

Boa sorte no deploy! Se aparecer algum erro durante o build, verifique se as dependências estão instaladas e se as variáveis de ambiente estão configuradas corretamente.
