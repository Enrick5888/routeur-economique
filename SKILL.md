---
name: routeur-economique
description: Délègue la lecture de gros contenus (PDF longs, fils de mails, SharePoint, plusieurs pages web) à un agent Haiku et la production lourde (code, fichiers Excel/Word complets) à un agent Sonnet, pour économiser les tokens. À utiliser avant ces tâches, jamais pour une question simple.
---

# Routeur économique

Priorités, dans l'ordre : **1. économie · 2. qualité · 3. vitesse.**

Principe : l'agent bon marché sert de **filtre**. Il lit le contenu brut et ne renvoie qu'un condensé. Le modèle principal ne lit jamais le brut. Déléguer une petite tâche coûte PLUS cher (l'agent démarre à froid) : ne déléguer que le volumineux.

## 1. Décider

| Situation | Action |
|---|---|
| Question simple, contenu court (< ~5 pages ou ~3 000 mots) | Répondre directement. Aucun agent. |
| Gros contenu à lire, résumer ou fouiller | **Agent Lecteur** (haiku) |
| Production volumineuse : code > ~50 lignes, fichier Excel/Word/PPT complet | **Agent Exécutant** (sonnet), puis relecture par le modèle principal |
| Contenu à enjeu juridique ou financier (contrat, litige, mise en demeure, chiffrage contesté) | Lire en direct. **Pas de Haiku** : la qualité prime. |
| Plusieurs grosses sources indépendantes | Un Lecteur par source, lancés en parallèle dans le même message |

En cas de doute entre direct et délégation : direct.

## 2. Agent Lecteur

Outil `Agent`, `model: "haiku"`, `subagent_type: "general-purpose"`. Prompt type :

```
Tu es un agent de lecture. Source : <chemin, URL, ou requête mail/SharePoint>.
Objectif : <ce que l'utilisateur veut savoir, en une phrase>.
Contexte utile : <2-3 lignes max>.
Rends AU MAXIMUM 300 mots :
- Réponse à l'objectif.
- Chiffres, montants, dates, noms, références : recopiés EXACTEMENT.
- Passages clés : citation courte + localisation (page, mail du JJ/MM, section).
- "Incertitudes" : ce qui est ambigu, manquant ou contradictoire.
Ne reformule pas les chiffres. Ne conclus pas au-delà de la source.
```

## 3. Agent Exécutant

Outil `Agent`, `model: "sonnet"`, `subagent_type: "general-purpose"`. Prompt type :

```
Tu es un agent d'exécution. Produis : <livrable précis>.
Spécification complète : <entrées, format, contraintes, langue, ton>.
Écris le résultat dans : <chemin du fichier>.
Rends en 5 lignes max : chemin, ce qui est fait, ce qui reste douteux.
```

Ensuite, le modèle principal relit le fichier, corrige, puis livre à l'utilisateur.

## 4. Règles d'économie

- L'agent démarre sans contexte : lui donner **tout le nécessaire, rien de plus**.
- Ne pas relire soi-même ce que le Lecteur a lu. Exception : vérification ciblée d'un chiffre décisif.
- Ne jamais recoller du contenu brut dans la réponse.
- Échec de l'agent : une relance maximum, puis faire soi-même.
- Pas de narration des délégations à l'utilisateur ; livrer le résultat.

## 5. Limites

- Cette skill ne change pas le modèle de la conversation principale : c'est l'utilisateur qui le choisit (Sonnet par défaut recommandé, Opus pour les sujets à enjeu).
- Elle nécessite l'outil `Agent` avec le paramètre `model`. Sans cet outil, appliquer seulement la section 4.
- Haiku peut perdre des nuances : en cas de doute sur une information du condensé, vérifier à la source.
