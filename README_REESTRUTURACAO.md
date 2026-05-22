# MAR Compressores — Reestruturação para Vercel

## 🎯 O que foi feito

Seu projeto foi completamente reestruturado para funcionar na Vercel com serverless functions. O backend Express foi convertido para API routes serverless.

---

## 📂 Nova Estrutura

```
mar-compressores/
├── client/                    # React + Vite (frontend)
│   ├── src/
│   │   ├── pages/
│   │   │   └── Home.tsx       # ✅ Atualizado para chamar /api/orcamento
│   │   └── ...
│   ├── index.html
│   └── package.json
├── api/                       # ✅ NOVO - Serverless functions
│   ├── health.ts              # Health check
│   ├── orcamento.ts           # Envio de orçamentos
│   └── types.ts               # Tipos TypeScript
├── vercel.json                # ✅ NOVO - Configuração Vercel
├── package.json               # ✅ Atualizado
├── DEPLOYMENT.md              # ✅ NOVO - Guia de deployment
└── README_REESTRUTURACAO.md   # Este arquivo
```

---

## 🚀 Como Começar

### 1. Instalar dependências
```bash
npm install
```

### 2. Rodar em desenvolvimento
```bash
npm run dev
```

### 3. Testar a API
```bash
# Em outro terminal
curl http://localhost:3000/api/health
```

### 4. Build para produção
```bash
npm run build
```

---

## 🔑 Mudanças Principais

### ❌ Removido
- `server/index.ts` — Express não funciona na Vercel
- `express` dependency — Não é mais necessário
- `esbuild` — Não precisa compilar backend
- Scripts de build complexos

### ✅ Adicionado
- `/api/` — Pasta com serverless functions
- `vercel.json` — Configuração para Vercel
- `@vercel/node` — Tipos para serverless
- Endpoints `/api/health` e `/api/orcamento`

### ✏️ Atualizado
- `package.json` — Scripts simplificados
- `Home.tsx` — Agora usa fetch para `/api/orcamento`
- Estrutura geral — Pronta para Vercel

---

## 📡 Endpoints Disponíveis

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/api/health` | Verifica se a API está funcionando |
| POST | `/api/orcamento` | Recebe solicitações de orçamento |

---

## 🧪 Testando Localmente

### Health Check
```bash
curl http://localhost:3000/api/health
```

### Enviar Orçamento
```bash
curl -X POST http://localhost:3000/api/orcamento \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "João Silva",
    "telefone": "(11) 99999-9999",
    "email": "joao@email.com",
    "servico": "compressor",
    "equipamento": "Schulz CSL 20BR",
    "descricao": "Compressor não liga"
  }'
```

---

## 📤 Deploy na Vercel

1. Faça push do código para GitHub
2. Conecte o repositório à Vercel
3. Clique em "Deploy"
4. Pronto! Seu site estará no ar

**Mais detalhes:** Veja `DEPLOYMENT.md`

---

## ⚠️ Importante

- O arquivo `/server/index.ts` pode ser deletado (não é mais usado)
- A pasta `/api` é lida automaticamente pela Vercel
- O `vercel.json` configura automaticamente o roteamento
- Não precisa fazer `npm run start` — Vercel gerencia tudo

---

## 🐛 Problemas Comuns

**P: Recebo erro "Cannot find module 'express'"**
R: Express foi removido. Use a API serverless em `/api/` em vez disso.

**P: A API retorna 404**
R: Verifique se o arquivo está em `/api/` com extensão `.ts`.

**P: CORS error**
R: O `vercel.json` já configura CORS. Se persistir, verifique os headers.

---

## 📚 Documentação Completa

Para instruções detalhadas de deployment, testes e troubleshooting, veja **`DEPLOYMENT.md`**.

---

## ✅ Checklist

- [ ] Dependências instaladas (`npm install`)
- [ ] Desenvolvimento rodando (`npm run dev`)
- [ ] API testada localmente
- [ ] Repositório enviado para GitHub
- [ ] Projeto conectado à Vercel
- [ ] Deploy bem-sucedido
- [ ] Endpoints respondendo em produção

---

**Seu projeto está pronto para a Vercel! 🚀**
