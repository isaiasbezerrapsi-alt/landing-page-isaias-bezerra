LANDING PAGE | ISAÍAS BEZERRA

Arquivos:
- index.html
- firebase-config.js
- images/

AVALIAÇÕES COM LOGIN GOOGLE
A seção “Como foi sua experiência comigo?” já está inserida antes de “Perguntas frequentes”. Ela possui:
- login com conta Google;
- nota de 1 a 5 estrelas;
- comentário breve;
- publicação no Firestore;
- listagem das avaliações no site.

Para ativar a função no Vercel:
1. Crie um projeto no Firebase Console.
2. Ative Authentication > Sign-in method > Google.
3. Crie o Firestore Database.
4. Cadastre um aplicativo Web e copie a configuração para firebase-config.js.
5. No Firebase Authentication > Settings > Authorized domains, adicione isaiasbezerrapsi.com.br e www.isaiasbezerrapsi.com.br.
6. Publique novamente o projeto no Vercel.

REGRAS BÁSICAS DO FIRESTORE
Cole estas regras no Firestore > Rules e publique:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /reviews/{reviewId} {
      allow read: if true;
      allow create: if request.auth != null
        && request.resource.data.uid == request.auth.uid
        && request.resource.data.rating is number
        && request.resource.data.rating >= 1
        && request.resource.data.rating <= 5
        && request.resource.data.comment is string
        && request.resource.data.comment.size() >= 5
        && request.resource.data.comment.size() <= 400;
      allow update, delete: if request.auth != null
        && resource.data.uid == request.auth.uid;
    }
  }
}

OBSERVAÇÃO
O login Google e o armazenamento das avaliações precisam de Firebase para funcionar de verdade entre diferentes visitantes. Sem a configuração do Firebase, a seção permanece visualmente pronta, mas o botão fica desativado até que as credenciais sejam preenchidas.
Atualização da landing page.
