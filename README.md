# TxopelaGO Moz — site completo

Site estático (HTML/CSS/JS puro) com autenticação, pedido de corrida em tempo real, painel do motorista e painel administrativo, tudo ligado ao Supabase.

## ⚠️ Escopo real deste projeto (leia antes de divulgar como app)

Isto é uma **aplicação web completa e funcional**, não um app nativo. Fique atento ao que está e não está incluído:

**Incluído:**
- Cadastro/login por e-mail e senha (passageiro ou motorista)
- Pedido de corrida com preço estimado (tarifa base + por km), código promocional
- Aceitação de corrida pelo motorista, atualização de status em tempo real (Supabase Realtime)
- Avaliação por estrelas ao final da corrida
- Seleção de M-Pesa / e-Mola como método — fica registrado como **pagamento pendente** para confirmação manual (não existe integração oficial ativa)
- Link de rota no Google Maps (sem precisar de chave de API)
- Painel admin: métricas, aprovação de motoristas, tarifas/comissão, promoções, confirmação de pagamentos

**Não incluído (precisa de trabalho adicional fora deste chat):**
- App nativo Android/iOS (Flutter) publicável nas lojas
- GPS/rastreamento ao vivo da localização real
- Integração oficial com APIs do M-Pesa / e-Mola (exige contrato comercial)
- Verificação por OTP/SMS
- Notificações push

## Passo 1 — Configurar o Supabase

1. Crie um projeto em https://supabase.com/dashboard (se ainda não criou)
2. Em **SQL Editor**, rode nesta ordem:
   - `supabase/schema.sql`
   - `supabase/extensoes.sql`
3. Em **Project Settings → API**, copie a **Project URL** e a **anon public key**
4. Cole essas duas informações no arquivo `assets/js/supabase-client.js`, substituindo:
   ```js
   const SUPABASE_URL = 'COLOQUE_AQUI_SUA_PROJECT_URL';
   const SUPABASE_KEY = 'COLOQUE_AQUI_SUA_ANON_PUBLIC_KEY';
   ```

## Passo 2 — Criar o seu usuário administrador

1. No Supabase: **Authentication → Users → Add user**
2. Preencha o e-mail e senha que você quer usar como admin
3. Marque **Auto Confirm User**
4. Acesse `admin-login.html` no site com esse e-mail/senha

(Este login de admin é separado do cadastro normal de passageiros/motoristas.)

## Passo 3 — Publicar / atualizar no Git e Netlify

```bash
# dentro da pasta txopelago
git init
git add .
git commit -m "TxopelaGO — versão inicial"
git remote add origin <url-do-seu-repositorio>
git push -u origin main
```

Se o site já está publicado no Netlify ligado a este repositório, o **push já atualiza o site automaticamente**. Se você publica arrastando a pasta manualmente, é só arrastar a pasta atualizada de novo em https://app.netlify.com/drop (ou no site já existente, em **Deploys → arrastar nova versão**).

## Estrutura de arquivos

```
txopelago/
├── index.html          → página inicial
├── entrar.html          → login/cadastro (passageiro ou motorista)
├── pedir.html           → passageiro: pedir corrida, acompanhar, avaliar
├── motorista.html       → motorista: ficar online, aceitar corridas, ganhos
├── admin-login.html     → login do administrador
├── admin.html           → painel administrativo completo
├── seguranca.html       → página de segurança e ajuda
├── assets/css/style.css → visual do site
├── assets/js/supabase-client.js → conexão com o Supabase (⚠️ preencher credenciais)
└── supabase/
    ├── schema.sql       → tabelas principais
    └── extensoes.sql    → avaliações, pagamentos, promoções, comissão
```
