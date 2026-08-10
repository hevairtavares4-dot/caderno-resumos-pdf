# Caderno de Resumos

Aplicação single-file (HTML/CSS/JS puro, sem build, sem framework) para organizar resumos de estudo para concursos públicos.

## Stack

- **Frontend**: HTML/CSS/JS puro (`index.html`), sem dependências de build
- **Banco de dados**: [Cloud Firestore](https://firebase.google.com/docs/firestore) (Firebase)
- **Autenticação**: Firebase Auth (login anônimo automático — sem senha)
- **Hospedagem**: [Vercel](https://vercel.com), com deploy automático a cada push neste repositório

## Configuração

Antes do app funcionar, é preciso criar um projeto no [Firebase Console](https://console.firebase.google.com) e colar as credenciais em `index.html`:

1. Crie um projeto no Firebase
2. Ative **Authentication → Sign-in method → Anonymous**
3. Crie um **Firestore Database** (modo produção)
4. Nas **Regras** do Firestore, cole:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /cadernos/{codigo}/resumos/{resumoId} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```
5. Em **Configurações do projeto → Seus apps**, crie um app Web e copie o objeto `firebaseConfig`
6. Cole esse objeto em `index.html`, procurando por `COLE_AQUI_SUA_API_KEY` perto do fim do arquivo

## Deploy

Este repositório está conectado à Vercel — qualquer `git push` na branch principal publica automaticamente uma nova versão. Não é necessário build step (é servido como site estático).

## Funcionalidades

- Editor rico (contenteditable) com formatação, checklists, tópicos/subtópicos, imagens
- Links internos entre resumos e links para questões externas (QConcursos, TecConcursos, etc.)
- Dashboard com métricas, gráfico por disciplina e "resumos esquecidos"
- Sumário em árvore (Disciplina → Assunto → Resumo) com busca
- Modo escuro/claro
- Sincronização em tempo real entre dispositivos via Firestore
- Backup: exportar/importar JSON
