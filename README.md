# Dashboard Orçamentário — Filiais (Inpasa)

App de análise orçamentária por filial com Firebase Firestore como backend.

## Stack
- **Frontend**: HTML + CSS + JS puro (sem framework)
- **Banco de dados**: Firebase Firestore
- **Hosting**: Vercel (via GitHub)

---

## Deploy

### 1. Firebase — configurar Firestore

1. Acesse [console.firebase.google.com](https://console.firebase.google.com)
2. Abra o projeto **orcamentomultimodal**
3. Vá em **Firestore Database** → **Criar banco de dados**
4. Escolha modo **Produção**
5. Selecione a região `southamerica-east1` (São Paulo)
6. Em **Regras**, cole as regras abaixo e publique:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /filiais/{filial} {
      allow read, write: if true;
    }
    match /config/{doc} {
      allow read, write: if true;
    }
  }
}
```

> ⚠️ Essas regras permitem acesso público. Para produção com autenticação, substitua `if true` por `if request.auth != null`.

---

### 2. GitHub — subir o repositório

```bash
cd orcamento-filiais
git init
git add .
git commit -m "feat: dashboard orçamentário com Firebase"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/orcamento-filiais.git
git push -u origin main
```

---

### 3. Vercel — conectar e deployar

1. Acesse [vercel.com](https://vercel.com) e faça login
2. Clique em **Add New Project**
3. Importe o repositório `orcamento-filiais` do GitHub
4. Em **Framework Preset**, selecione **Other**
5. Em **Root Directory**, deixe `/` (raiz)
6. Em **Build & Output Settings**:
   - Build Command: *(deixe em branco)*
   - Output Directory: `public`
7. Clique em **Deploy**

✅ O Vercel vai gerar uma URL tipo `https://orcamento-filiais.vercel.app`

---

## Estrutura Firestore

```
filiais/
  {nomeDaFilial}/
    data: { rows: [...], months: [...] }
    updatedAt: "2026-05-28T..."

config/
  obs/
    obs: { "Filial__grupo__emp__mes": "texto observação", ... }
    updatedAt: "2026-05-28T..."
```

---

## Uso

1. Abra o app pela URL do Vercel
2. Vá na aba **⚙️ Dados** e faça upload dos arquivos CSV das filiais
3. Os dados são salvos automaticamente no Firestore
4. Qualquer pessoa com o link acessa os mesmos dados em tempo real
