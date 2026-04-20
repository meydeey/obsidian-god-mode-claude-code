# Obsidian God Mode avec Claude Code

> Transforme Claude Code en générateur autonome de knowledge vault Obsidian. Tu donnes un sujet, il construit pour toi une base interconnectée de centaines de pages, avec clusters, wikilinks, fact-check WebSearch et graphe Obsidian navigable.

**Pattern Karpathy** : arrêter de refaire du RAG jetable à chaque requête. Laisser le LLM compiler une fois pour toutes un wiki structuré qui compound dans le temps.

---

## Le repo en 30 secondes

Tu as deux prompts dans ce repo :

1. **`PROMPT-PILOT.md`** : version pilote avec checkpoints humains. Tu lances sur **1 seul cluster de 30-50 pages** pour calibrer. Budget : 1-2h, 10-30€.
2. **`GUIDE.md`** : comment l'utiliser pas à pas, quelles variables changer, comment adapter à n'importe quel domaine (psychologie, marketing, crypto, philo, ce que tu veux).

Le prompt de **production** (scaling à 500+ pages avec multi-agent) est généré à la fin du run pilote, calibré sur ton propre retour d'expérience. C'est volontaire : pas de prompt de prod avant d'avoir validé ton setup.

---

## Pourquoi ça marche

La plupart des gens utilisent les LLMs en **mode RAG jetable** : chaque question, ils recontextualisent, re-embeddent, re-retrievent. Gaspillage de compute, pas de compound.

La proposition : **compiler une fois** un wiki Obsidian structuré. Ensuite :
- Le savoir est **local**, navigable en graph view
- Les **wikilinks** créent un réseau réutilisable
- Les **fact-check WebSearch** sont loggés et vérifiables
- Le vault **grossit** avec ton usage, il ne se perd pas

---

## Démarrer en 3 étapes

1. Ouvre [`GUIDE.md`](./GUIDE.md) et lis les 5 minutes de setup
2. Copie [`PROMPT-PILOT.md`](./PROMPT-PILOT.md)
3. Remplace les 5 variables (SUJET, CLUSTER, DESCRIPTION, CHEMIN_VAULT, NB_PAGES_CIBLE) puis envoie à Claude Code

Tu auras un vault pilote opérationnel en 1-2h, avec un rapport de calibration (`99-Meta/Debrief.md`) qui te sert à scaler à 500+ pages ensuite.

---

## Les 3 checkpoints humains

Le prompt pilote **s'arrête volontairement 3 fois** pour te demander validation :

| Checkpoint | Moment | Décision |
|---|---|---|
| 1 | Après le plan des pages | Valider la cartographie du cluster |
| 2 | Après 8 pages rédigées | Valider la qualité rédactionnelle |
| 3 | Après le run complet | Lire le debrief pour scaler |

C'est **lent par design**. Lancer en full auto sur 500 pages sans checkpoint, c'est 12h de compute brûlées et un vault plein d'hallucinations.

---

## LE LABO IA

Si tu veux apprendre à absorber l'IA dans ton business (ou à la facturer à des clients), j'ai monté **LE LABO IA** : un programme pour solo-preneurs et indépendants qui veulent transformer l'IA en avantage concurrentiel concret.

- Rejoindre : [lelaboia.com](https://lelaboia.com)
- Skool gratuit : [skool.com/le-labo-ia](https://www.skool.com/le-labo-ia)
- Chaîne YouTube : [@meydeey](https://youtube.com/@meydeey)

---

## Licence

MIT. Fais ce que tu veux avec ce prompt. Si tu l'utilises, tag [@meydeey](https://x.com/meydeey) sur X pour que je vois ce que tu construis.
