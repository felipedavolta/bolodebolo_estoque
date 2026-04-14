# Estoque-Precos

Aplicação de controle de estoque com autenticação via Firebase Google Sign-In e sincronização de dados no Firestore.

## Passos para configurar o Firebase

1. Acesse o Firebase Console: https://console.firebase.google.com/
2. Crie um novo projeto ou use um projeto existente.
3. Adicione um aplicativo web ao projeto.
   - No painel do projeto, vá em *Configurações do projeto* > *Seus apps* > *Adicionar app* > *Web*.
   - Copie o objeto de configuração e cole em `index.html` no bloco `FIREBASE_CONFIG`.
4. Habilite autenticação com Google:
   - Vá em *Authentication* > *Sign-in method*.
   - Ative o provedor *Google*.
5. Adicione domínios autorizados:
   - Em *Authentication* > *Settings* > *Authorized domains*, adicione:
     - `localhost`
     - `felipedavolta.github.io`
     - (ou o domínio onde o site será hospedado)
6. Crie o banco de dados Firestore:
   - Vá em *Firestore Database* e crie o banco de dados no modo de produção.
   - O app usa a coleção `estoque` e o documento `dados`.

## Permissões de acesso

O app já está configurado para liberar apenas estes e-mails:

- `felipedavolta@gmail.com`
- `bolodebolomf@gmail.com`

Também é recomendado aplicar regras de segurança no Firestore.

## Regras de segurança sugeridas

Aplique as regras em *Firestore Database* > *Rules*:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /estoque/dados {
      allow read, write: if request.auth != null && request.auth.token.email in [
        'felipedavolta@gmail.com',
        'bolodebolomf@gmail.com'
      ];
    }
  }
}
```

## Hospedagem no GitHub Pages

Você pode publicar o projeto como site estático no GitHub Pages.

### Opção 1: repositório de usuário/organização

1. Crie um repositório chamado `felipedavolta.github.io`.
2. Faça commit dos arquivos do projeto.
3. No GitHub, vá em *Settings* > *Pages* e publique a branch `main` com a pasta `/`.
4. O site ficará disponível em `https://felipedavolta.github.io`.

### Opção 2: repositório de projeto

1. Crie um repositório qualquer, por exemplo `Estoque-Precos`.
2. Faça commit dos arquivos do projeto.
3. No GitHub, vá em *Settings* > *Pages* e publique a branch `main` com a pasta `/`.
4. O site ficará disponível em `https://felipedavolta.github.io/Estoque-Precos`.

## Como usar

1. Abra o site.
2. Clique em *Entrar com Google*.
3. Apenas os e-mails autorizados poderão acessar o app.
4. O sistema salva os dados automaticamente no Firestore.

## Observações

- O arquivo `dados_estoque.json` no repositório é um backup local.
- Para importar/exportar dados, use os botões *Importar* e *Exportar* no app.
