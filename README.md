# COPO

Maintenance des machines-outils pour ateliers d'usinage : signalement de pannes, entretien préventif du jour, suivi du liquide de coupe, étiquettes QR par machine, espace opérateur et espace responsable.

## Contenu

- `index.html` : l'application
- `config.js` : l'adresse et la clé publique (anon) du projet Supabase
- `supabase/schema.sql` : la base de données (version 2), à exécuter dans Supabase > SQL Editor. Peut être relancé sans risque.
- `supabase/functions/alerte-panne/index.ts` : envoi d'un e-mail aux responsables à chaque nouvelle panne (facultatif)
- `manifest.webmanifest`, `favicon.svg`, `icon-192.png`, `icon-512.png` : icônes et installation sur téléphone

## Mise en ligne

1. Supabase : exécuter `supabase/schema.sql` dans le SQL Editor.
2. `config.js` : URL du projet et clé `anon` (jamais la clé `service_role`).
3. Vercel : importer ce dépôt (type de projet : Other). Chaque modification poussée sur GitHub est mise en ligne automatiquement.
4. Supabase > Authentication > URL Configuration : mettre l'adresse du site dans Site URL.

## Alertes par e-mail (facultatif)

1. Créer un compte gratuit sur resend.com, puis une clé API (API Keys > Create API Key).
2. Supabase > Edge Functions > Deploy a new function > Via Editor. Nom : `alerte-panne`. Coller le contenu de `supabase/functions/alerte-panne/index.ts`, puis Deploy.
3. Dans les réglages de la fonction, désactiver « Verify JWT » (la fonction vérifie elle-même le secret `x-copo-secret`).
4. Supabase > Edge Functions > Secrets : ajouter `RESEND_API_KEY`, `COPO_WEBHOOK_SECRET` (un mot de passe au choix) et `COPO_SITE_URL` (l'adresse du site).
5. Supabase > Database > Webhooks > Create a new hook :
   - Table : `docs`, événement : Insert
   - Type : HTTP Request, méthode POST
   - URL : `https://<id-du-projet>.supabase.co/functions/v1/alerte-panne`
   - En-tête : `x-copo-secret` = la même valeur que `COPO_WEBHOOK_SECRET`
6. Tant qu'aucun nom de domaine n'est vérifié dans Resend, les e-mails ne partent que vers l'adresse du compte Resend. Pour prévenir d'autres responsables, vérifier un domaine dans Resend et renseigner `COPO_EMAIL_FROM` (ex. `COPO <alertes@mon-domaine.fr>`).
