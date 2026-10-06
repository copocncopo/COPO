# COPO

Maintenance des machines-outils pour ateliers d'usinage : signalement de pannes, entretien préventif, suivi du liquide de coupe et étiquettes QR par machine.

## Contenu

- `index.html` : l'application
- `config.js` : l'adresse et la clé publique (anon) du projet Supabase
- `supabase/schema.sql` : la base de données, à exécuter une fois dans Supabase (SQL Editor)
- `manifest.webmanifest`, `favicon.svg`, `icon-192.png`, `icon-512.png` : icônes et installation sur téléphone

## Mise en ligne

1. Supabase : exécuter `supabase/schema.sql` dans le SQL Editor.
2. Renseigner `config.js` avec l'URL du projet et la clé `anon` (jamais la clé `service_role`).
3. Importer ce dépôt dans Vercel (type de projet : Other, aucun réglage de build).
4. Supabase > Authentication > URL Configuration : mettre l'adresse Vercel dans Site URL.
