# MAR Compressores — Guia de Deployment na Vercel

## 📋 Visão Geral

Este documento descreve como o projeto foi reestruturado para funcionar na Vercel com serverless functions, convertendo o backend Express para API routes serverless.

---

## 🏗️ Estrutura do Projeto

### Antes (Express)
```
/server/index.ts          → Express server com app.listen()
/client/                  → React + Vite
package.json              → Scripts que compilavam Express
```

### Depois (Vercel Serverless)
```
/api/                     → Serverless functions
  ├── health.ts           → Health check
  ├── orcamento.ts        → Envio de orçamentos
  └── types.ts            → Tipos TypeScript
/client/                  → React + Vite (sem mudanças)
vercel.json               → Configuração Vercel
package.json              → Scripts atualizados
```

---

## 🚀 Endpoints da API

### 1. Health Check
**Endpoint:** `GET /api/health`

**Resposta (200):**
```json
{
  "status": "ok",
  "message": "MAR Compressores API is running",
  "timestamp": "2026-05-22T14:00:00.000Z"
}
```

### 2. Envio de Orçamento
**Endpoint:** `POST /api/orcamento`

**Headers:**
```
Content-Type: application/json
```

**Body:**
```json
{
  "nome": "João Silva",
  "telefone": "(11) 99999-9999",
  "email": "joao@email.com",
  "servico": "compressor",
  "equipamento": "Schulz CSL 20BR",
  "descricao": "Compressor não liga"
}
```

**Resposta (200):**
```json
{
  "success": true,
  "message": "Orçamento recebido com sucesso",
  "data": {
    "to_email": "mrodriguescompressores@gmail.com",
    "from_name": "João Silva",
    "from_email": "joao@email.com",
    "phone": "(11) 99999-9999",
    "service_type": "compressor",
    "equipment": "Schulz CSL 20BR",
    "description": "Compressor não liga"
  }
}
```

**Erros:**
- `400` — Campos obrigatórios faltando
- `405` — Método não permitido
- `500` — Erro ao processar

---

## 🧪 Testando Localmente

### 1. Instalar dependências
```bash
cd /home/ubuntu/mar-compressores
npm install
```

### 2. Rodar em desenvolvimento
```bash
npm run dev
```

O site estará disponível em `http://localhost:5173`

### 3. Testar a API localmente

#### Opção A: Com curl
```bash
# Health check
curl http://localhost:3000/api/health

# Enviar orçamento
curl -X POST http://localhost:3000/api/orcamento \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Teste",
    "telefone": "(11) 99999-9999",
    "email": "teste@email.com",
    "servico": "compressor",
    "equipamento": "Schulz",
    "descricao": "Teste"
  }'
```

#### Opção B: Com JavaScript/Fetch
```javascript
// Health check
fetch('/api/health')
  .then(res => res.json())
  .then(data => console.log(data));

// Enviar orçamento
fetch('/api/orcamento', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    nome: 'Teste',
    telefone: '(11) 99999-9999',
    email: 'teste@email.com',
    servico: 'compressor',
    equipamento: 'Schulz',
    descricao: 'Teste'
  })
})
  .then(res => res.json())
  .then(data => console.log(data));
```

---

## 📦 Deploy na Vercel

### 1. Preparar o repositório Git
```bash
cd /home/ubuntu/mar-compressores
git init
git add .
git commit -m "Reestruturar para Vercel serverless"
git branch -M main
git remote add origin https://github.com/seu-usuario/mar-compressores.git
git push -u origin main
```

### 2. Conectar à Vercel
1. Acesse [vercel.com](https://vercel.com)
2. Clique em "New Project"
3. Selecione seu repositório GitHub
4. Configure as variáveis de ambiente (se necessário)
5. Clique em "Deploy"

### 3. Variáveis de Ambiente (opcional)
Se quiser usar variáveis de ambiente na Vercel, adicione no painel:

```
EMAILJS_SERVICE_ID=service_96t6j2e
EMAILJS_TEMPLATE_ID=template_rnv70qu
EMAILJS_PUBLIC_KEY=VQMUvqAGcfg13gJtl
```

---

## ✅ Checklist de Deploy

- [ ] Repositório Git criado e enviado para GitHub
- [ ] Projeto conectado à Vercel
- [ ] Build bem-sucedido (`npm run build`)
- [ ] Endpoints da API respondendo corretamente
- [ ] Formulário enviando dados para `/api/orcamento`
- [ ] E-mails sendo recebidos em `mrodriguescompressores@gmail.com`
- [ ] CORS funcionando corretamente
- [ ] Domínio Vercel funcionando

---

## 🔧 Troubleshooting

### Problema: "Cannot find module 'express'"
**Solução:** Express foi removido. As rotas agora são serverless functions em `/api`.

### Problema: "API retorna 404"
**Solução:** Verifique se o arquivo está em `/api/` com extensão `.ts` e se o `vercel.json` está configurado corretamente.

### Problema: "CORS error"
**Solução:** O `vercel.json` já configura CORS. Se o erro persistir, verifique o header `Access-Control-Allow-Origin`.

### Problema: "Formulário não envia dados"
**Solução:** Verifique se o `handleSubmit` em `Home.tsx` está chamando `/api/orcamento` corretamente.

---

## 📝 Mudanças Realizadas

| Arquivo | Mudança |
|---------|---------|
| `/server/index.ts` | ❌ Removido (Express não funciona na Vercel) |
| `/api/health.ts` | ✅ Novo (health check serverless) |
| `/api/orcamento.ts` | ✅ Novo (envio de orçamentos serverless) |
| `/api/types.ts` | ✅ Novo (tipos TypeScript) |
| `package.json` | ✅ Atualizado (removido Express, scripts simplificados) |
| `vercel.json` | ✅ Novo (configuração Vercel) |
| `client/src/pages/Home.tsx` | ✅ Atualizado (fetch para `/api/orcamento`) |

---

## 🎯 Próximos Passos

1. **Implementar autenticação** — Adicionar login para gerenciar orçamentos
2. **Banco de dados** — Armazenar orçamentos em um banco de dados
3. **Notificações em tempo real** — Alertar quando novo orçamento chega
4. **Dashboard de admin** — Gerenciar orçamentos recebidos
5. **Integração com CRM** — Sincronizar com sistema de gestão

---

## 📞 Suporte

Para dúvidas ou problemas, consulte:
- [Documentação Vercel](https://vercel.com/docs)
- [Vercel Node.js Runtime](https://vercel.com/docs/functions/nodejs)
- [Configuração vercel.json](https://vercel.com/docs/projects/project-configuration)

---

**Última atualização:** 22 de maio de 2026
