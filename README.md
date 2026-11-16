# routeur-economique

Skill Claude qui réduit la consommation de tokens en déléguant :

- **la lecture de gros contenus** (PDF, fils de mails, SharePoint, pages web) à un agent **Haiku**, qui renvoie un condensé de 300 mots maximum ;
- **la production volumineuse** (code, fichiers Excel/Word) à un agent **Sonnet**, relu ensuite par le modèle principal.

Les questions simples restent traitées directement : déléguer une petite tâche coûte plus cher qu'elle ne rapporte.

Priorités : économie > qualité > vitesse. Les contenus juridiques ou financiers sensibles ne sont jamais confiés à Haiku.

## Installation

- **claude.ai** : importer le dossier comme skill (Paramètres > Capabilities > Skills).
- **Claude Code** : copier le dossier dans `~/.claude/skills/routeur-economique/`.

Prérequis : un environnement où l'outil `Agent` accepte le paramètre `model`.

## Limites

La skill ne change pas le modèle de la conversation principale ; ce choix reste à l'utilisateur.

## Licence

MIT
