# MyPage — prototype connecté

Ce dossier contient une première version serveur de MyPage : interface mobile sombre, création de compte, connexion, sessions serveur, publications, réactions, commentaires, recherche de profils et abonnements. La version locale d'origine est conservée dans `public/demo-local.html`.

## Prérequis
- Node.js 20 ou plus récent
- Un environnement serveur qui autorise les modules natifs (SQLite `better-sqlite3`)

## Lancer en local
1. Installer Node.js.
2. Dans ce dossier : `npm install`
3. Copier `.env.example` vers `.env` et définir `SESSION_SECRET` avec une longue valeur aléatoire.
4. `npm start`
5. Ouvrir `http://localhost:3000`.

## Déploiement
Déployer le dossier sur un hébergeur Node.js. Définir `NODE_ENV=production`, `SESSION_SECRET` (secret aléatoire d'au moins 32 caractères), et `DATABASE_PATH` sur un disque persistant. Le serveur suppose HTTPS en production pour le cookie de session `Secure`. Ne pas utiliser un stockage éphémère pour la base SQLite : les comptes et publications seraient perdus au redémarrage.

## Sécurité déjà prévue
- Hachage des mots de passe avec bcrypt (coût 12)
- Sessions aléatoires stockées sous forme de hash côté base de données, cookies `HttpOnly`, `SameSite=Lax` et `Secure` en production
- Limitation de débit globale et plus stricte sur l'authentification
- Validation des champs avec Zod, limites de taille JSON, Helmet et requêtes SQL paramétrées
- Suppression des sessions expirées

## Limites importantes
Ce projet est un **prototype de départ**, pas encore prêt pour une mise en production à grande échelle. Il manque notamment la vérification e-mail, la récupération sécurisée de compte, la gestion anti-CSRF renforcée, le processus de modération/signalement, la messagerie temps réel, le stockage média externe, les règles complètes de visibilité des profils privés, les notifications, les sauvegardes/restaurations et les tests automatisés. Le bouton « profil privé » est enregistré, mais l'ensemble des contrôles de visibilité n'est pas encore complet : ne l'utilisez pas pour des informations sensibles.

La base est créée automatiquement au premier démarrage dans `data/mypage.sqlite` (ou `DATABASE_PATH`). Faites des sauvegardes chiffrées et testez la restauration avant toute utilisation réelle.
