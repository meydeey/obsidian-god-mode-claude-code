# KNOWLEDGE VAULT GENERATOR, PILOT (1 Cluster, Checkpoints Humains)

> Version pilote du générateur. Objectif : calibrer la méthode sur un périmètre réduit (30-50 pages, 1 cluster) avant de scaler à 500+ pages. Budget cible : 1-2h de run, 10-30€ en tokens. Résultat : un prompt de production validé empiriquement.

---

## VARIABLES À REMPLIR AVANT D'ENVOYER

```
SUJET              : [Exemple : "Psychologie"]
CLUSTER_CHOISI     : [Exemple : "Biais cognitifs"]
DESCRIPTION_CLUSTER: [Exemple : "Erreurs systématiques de jugement documentées par la recherche en psychologie cognitive, de Kahneman/Tversky à aujourd'hui. Inclure biais classiques, biais débattus, meta-biais."]
CHEMIN_VAULT       : [Exemple : "/Users/meydeey/Obsidian/pilot-psycho-biais"]
NB_PAGES_CIBLE     : 40 (fourchette acceptable : 30 à 50)
```

---

## MISSION

Tu es l'agent principal chargé de construire un **vault pilote** sur un seul cluster du sujet **{SUJET}** : le cluster **{CLUSTER_CHOISI}**. Objectif : 30 à 50 pages denses, interconnectées, en français, avec wikilinks et graphe Obsidian navigable.

Ce run est un **test de calibration**. À la fin, un vault de production beaucoup plus gros sera construit. Ta qualité et ton respect des conventions ici déterminent la suite.

**Mode d'opération** : tu travailles en autonomie sur les phases techniques, mais tu **t'arrêtes et demandes validation humaine à 3 checkpoints précis**. Pas d'autonomie totale. Pas de spawn multi-agent complexe : tu peux utiliser le Task tool pour des subagents focalisés (1 subagent à la fois max) uniquement quand c'est nécessaire (exploration parallèle d'un sous-thème, fact-check groupé). Sinon tu travailles en direct.

Langue : **français intégral** (contenu, titres, YAML, tags). Terminologie consacrée en anglais conservée dans le corps du texte uniquement quand c'est le nom propre d'un concept (ex : "Dunning-Kruger effect" dans le texte mais titre "Effet Dunning-Kruger").

---

## ARCHITECTURE DU VAULT PILOTE

```
{CHEMIN_VAULT}/
├── CLAUDE.md                   # Schéma du vault
├── README.md                   # Vue d'ensemble
├── _MOC.md                     # Map of Content du cluster (index navigable)
├── pages/                      # Toutes les pages du cluster
│   └── [pages .md]
├── 99-Meta/
│   ├── Schema.md              # Conventions YAML
│   ├── Plan.md                # Plan des pages à créer (Phase 2)
│   ├── Fact-Check-Log.md      # Traces WebSearch
│   ├── Audit.md               # Rapport qualité final
│   └── Debrief.md             # Retour d'expérience pour scaling
└── .obsidian/
    └── graph.json             # Config graph view colorisée par type
```

Structure plate volontairement : sur 40 pages, pas besoin de sous-clusters.

---

## PHASES AVEC CHECKPOINTS HUMAINS

### PHASE 0, BOOTSTRAP (5 min)

1. Créer la structure de dossiers.
2. Rédiger `CLAUDE.md` racine (bref, 1 page) avec :
   - Sujet général, cluster ciblé, périmètre
   - Conventions YAML et wikilinks
   - Règles d'édition future
3. Rédiger `99-Meta/Schema.md` avec le schéma YAML détaillé (voir section "Conventions").
4. Configurer `.obsidian/graph.json` : couleurs par type YAML (concept=bleu, personne=vert, expérience=orange, controverse=rouge, terme=gris, débat=violet).
5. Rédiger un `README.md` minimal (sera enrichi en Phase 6).

Puis continue directement vers Phase 1.

### PHASE 1, CARTOGRAPHIE DU CLUSTER (15-20 min)

Génère dans `99-Meta/Plan.md` une liste de **35 à 50 pages à créer** couvrant le cluster sous tous ses angles.

Pour chaque page, fournir :

```markdown
### N. Titre exact de la page
- **Type** : concept | théorie | personne | expérience | controverse | école | méthode | terme | débat
- **Priorité** : 1 (pilier) | 2 (standard) | 3 (deep cut)
- **Résumé** : 2 phrases
- **Wikilinks pressentis** : [[Page A]], [[Page B]], [[Page C]]
- **Niveau controverse** : low | medium | high
- **Fact-check requis** : oui/non (oui si type = personne/expérience/controverse OU chiffres précis impliqués)
```

Exigences sur le plan :
- **Équilibre des types** : au moins 4 types différents représentés, aucun type qui domine à plus de 50%.
- **Spectre consensus-controverse** : au moins 5 pages type `controverse` ou `débat`, au moins 3 théories débunkées ou non-répliquables.
- **Pages-ponts potentielles** : 3 à 5 pages qui connectent ce cluster à d'autres domaines (signalées dans le résumé).
- **Anti-redondance** : deux pages quasi-synonymes = fusionner ou différencier explicitement.

Une fois le plan rédigé, **avocat du diable interne** : relis-le toi-même et challenge :
- Quels angles morts ?
- Quels chevauchements ?
- Quelles pages semblent faibles ou redondantes ?
- L'équilibre des types est-il tenu ?

Corrige directement sans m'en parler. Puis :

**CHECKPOINT 1, VALIDATION HUMAINE**

Affiche dans la console :

```
─────────────────────────────────────────
CHECKPOINT 1 : Plan du cluster terminé
─────────────────────────────────────────
Fichier : 99-Meta/Plan.md
Nombre de pages planifiées : N
Répartition par type : [liste]
Répartition par priorité : [liste]
Pages à fact-checker : N

Merci de relire 99-Meta/Plan.md et de répondre :
  "GO" pour lancer la rédaction
  "CORRIGE : [instructions]" pour itérer
```

**Tu t'arrêtes et tu attends ma réponse.** Pas de Phase 2 avant validation.

### PHASE 2, RÉDACTION LOT PILOTE (20-30 min)

Une fois GO reçu, rédige **uniquement les 8 premières pages priorité 1** (pas tout le cluster).

Objectif : valider la qualité de rédaction sur un échantillon avant de produire le reste.

Pour chaque page :

1. **Si fact-check requis** :
   - WebSearch ciblé (2 requêtes max par page)
   - Consigner dans `99-Meta/Fact-Check-Log.md` : question, sources trouvées, statut (confirmé / douteux / introuvable)
   - Si introuvable après 2 recherches : marquer `statut: to-verify` dans YAML et mentionner dans le corps "information à confirmer"

2. **Rédaction** selon STRUCTURE IMPOSÉE :

```markdown
---
[YAML complet, voir section Conventions]
---

# Titre

> [!info] Résumé
> 1 à 2 phrases de pitch clair et précis.

## Définition
2 à 4 paragraphes denses. Pas de listing à puces gratuit.

## Contexte et origine
Qui, quand, où, dans quel cadre intellectuel. Paragraphes.

## Mécanismes / caractéristiques / détails
Le cœur. 3 à 6 paragraphes. Exemples concrets.

## Nuances, critiques, limites
OBLIGATOIRE sur toutes les pages. Pour les pages controversées : section "Controverses" dédiée plus fournie.

## Liens et implications
Wikilinks sortants contextualisés dans des phrases complètes. Pas de liste brute.

## Sources
[^1]: Référence 1
[^2]: Référence 2
```

**Standards qualité** :
- Longueur : **800 à 1500 mots** par page
- Minimum **7 wikilinks sortants** par page
- Minimum **2 sources externes** pour pages sensibles (personne/expérience/controverse), sinon `statut: à-sourcer`
- Français clair, précis, zéro verbosité
- Pas de tiret cadratin, utilise deux-points ou virgules
- Pas de "il est important de noter que", "cependant il faut garder à l'esprit", "en conclusion"
- Ton neutre mais tranché : si un truc est débunké, dis "débunké" et pourquoi

**CHECKPOINT 2, VALIDATION HUMAINE QUALITÉ**

```
─────────────────────────────────────────
CHECKPOINT 2 : 8 pages pilotes rédigées
─────────────────────────────────────────
Pages créées : [liste des 8 titres]
Moyenne wikilinks sortants : X
Moyenne longueur : X mots
Pages avec statut to-verify : N
WebSearch effectués : N

Merci de relire 2-3 pages au hasard et répondre :
  "GO" pour rédiger les 25-35 pages restantes selon le même standard
  "AJUSTE : [instructions]" pour corriger et re-rédiger les 8 pages
  "STOP" pour abandonner le pilote
```

**Tu t'arrêtes. Tu attends. Pas de rédaction massive avant mon feu vert.**

### PHASE 3, RÉDACTION COMPLÈTE DU CLUSTER (45-60 min)

Une fois GO, rédige les pages restantes en **batches de 8-10 pages**. Entre chaque batch, message console court :

```
Batch N terminé : X pages. Total à ce stade : X/Y. Poursuite automatique.
```

Pas de nouveau checkpoint humain entre les batches. Tu enchaînes.

Règle stricte : **si tu n'as pas fini le cluster complet en 2h de wall time total depuis le début**, arrête-toi et fais un rapport d'avancement. Mieux vaut un pilote de 35 pages excellentes qu'un pilote de 50 pages mal finies.

### PHASE 4, LINKING ET MOC (15 min)

1. Générer le `_MOC.md` du cluster :
   - Introduction de 3 paragraphes situant le cluster
   - Liste des pages groupées par type, avec 1 phrase descriptive chacune
   - Section "Pages-ponts" identifiant les connexions potentielles vers d'autres clusters
   - Section "Pages à approfondir" listant les to-verify et à-sourcer

2. Densifier les wikilinks :
   - Parcourir chaque page, vérifier qu'elle est bien backlinkée depuis les pages pertinentes
   - Cible : ratio moyen 8-12 wikilinks par page
   - Wikilinks fantômes : si moins de 5 fantômes, les laisser (deep cuts futurs). Si plus de 5, lister dans `99-Meta/Pages-Fantômes.md`

3. Mettre à jour `README.md` avec stats du vault.

### PHASE 5, AUDIT QUALITÉ (10-15 min)

Produire `99-Meta/Audit.md` avec :

1. **Conformité YAML** : toutes les pages ont les champs obligatoires, valeurs dans les énumérations autorisées.
2. **Doublons** : pages à fusionner (normalement 0 si Phase 1 propre).
3. **Contradictions** : page A dit X, page B dit non-X, liste.
4. **Pages orphelines** : 0 backlink entrant, liste avec hypothèse (deep cut légitime ou erreur).
5. **Pages anémiques** : moins de 600 mots, liste avec action recommandée.
6. **Pages survoltées** : plus de 2000 mots, liste (à découper ?).
7. **Sources manquantes** : pages sensibles sans footnote, liste.
8. **Tags cohérents** : détection variantes orthographiques (#biais-cognitif vs #biais-cognitifs).
9. **Total wikilinks** : compte et ratio.

Pour chaque problème : action (corrigé automatiquement / nécessite arbitrage humain).

### PHASE 6, DEBRIEF POUR SCALING (10 min)

**Livrable critique du pilote.** Produire `99-Meta/Debrief.md` :

```markdown
# Debrief du run pilote

## Stats finales
- Pages créées : X
- Temps total : Xh
- Tokens consommés estimés : X
- WebSearch effectués : X
- Wikilinks totaux : X
- Pages marquées to-verify : X

## Ce qui a bien marché
- [Liste honnête]

## Ce qui a mal marché
- [Liste honnête, avec exemples précis si applicable]

## Hallucinations détectées
- [Liste exhaustive des faits inventés que tu as toi-même corrigés
   ou que l'humain devrait vérifier]

## Ajustements recommandés pour le prompt de production (500 pages)
- Format YAML : [à garder / à modifier]
- Structure de page : [à garder / à modifier]
- Règles wikilinks : [à garder / à modifier]
- Règles fact-check : [à renforcer / à alléger / à cibler différemment]
- Règles multi-agent : [retour d'expérience sur ce qui est réaliste]
- Rythme : [temps réel par page sensible vs conceptuelle]

## Estimation pour scaling x10
- Temps estimé pour 500 pages : Xh (extrapolation linéaire + overhead coordination)
- Coût tokens estimé : X€
- Points de vigilance spécifiques à anticiper
```

**RAPPORT FINAL AU USER**

```
─────────────────────────────────────────
RUN PILOTE TERMINÉ
─────────────────────────────────────────
Pages créées : X
Wikilinks : X
Graph Obsidian : prêt à visualiser
Pages à vérifier manuellement : X (voir 99-Meta/Audit.md)
Debrief pour scaling : 99-Meta/Debrief.md

Pour visualiser : ouvre {CHEMIN_VAULT} dans Obsidian puis Graph view.
Prochaine étape recommandée : relis Debrief.md puis ajuste le prompt de production.
```

---

## CONVENTIONS STRICTES

### YAML frontmatter obligatoire

```yaml
---
titre: "Nom exact de la page"
type: concept | théorie | personne | expérience | controverse | école | trouble | méthode | terme | débat | œuvre | institution | événement
cluster: "{CLUSTER_CHOISI}"
statut: verified | to-verify | débattu | débunké | hypothétique | stub | à-sourcer
controverse: low | medium | high
importance: pilier | standard | deep-cut
source_knowledge: internal | web-checked | mixed
sources_count: N
tags: [#tag1, #tag2]
créé: AAAA-MM-JJ
liens_forts: ["[[Page1]]", "[[Page2]]"]
liens_opposition: ["[[Page3]]"]
---
```

### Nommage fichiers

- Espaces autorisés (Obsidian gère).
- Titres en français naturel avec accents.
- Pas de préfixes numérotés sur les pages.

### Tags

- Minuscules, français, préfixe thématique (#concept/biais, #personne/psychologue).
- Max 5 tags par page.

### Wikilinks

- `[[Page exacte]]` ou `[[Page|alias]]`.
- Chaque wikilink justifié par la phrase qui l'entoure, jamais en listing brut hors MOC.

### Sources (footnotes Markdown)

```markdown
Fait sourcé[^1].

[^1]: Auteur, *Titre*, Éditeur, année, URL si dispo.
```

---

## RÈGLES ANTI-HALLUCINATION (NON-NÉGOCIABLES)

1. Aucun nom propre inventé. Si WebSearch ne trouve pas : la page n'existe pas ou statut `to-verify`.
2. Aucune date fabriquée. Si incertitude : mention "date exacte à confirmer" dans le corps.
3. Aucun chiffre statistique non sourcé.
4. Aucune citation directe inventée. Paraphrase neutre sinon.
5. Théories débunkées : dire "débunké", "non-réplicable", "considéré comme pseudoscience par le consensus actuel". Pas de neutralité complice par omission.
6. Reconnaître les biais WEIRD quand la recherche citée est principalement occidentale.
7. **Obligation de transparence** : toute hallucination détectée par toi-même en cours de route doit être loggée dans `99-Meta/Fact-Check-Log.md` section "Auto-corrections".

---

## DÉMARRAGE

Quand tu as lu et intégré ce prompt :

1. Confirme brièvement le sujet, le cluster, et le chemin vault.
2. Lance Phase 0 directement.
3. Enchaîne jusqu'au Checkpoint 1. Puis arrête-toi et attends.

Go.

---

## Crédits

Prompt créé par **[LE LABO IA](https://lelaboia.com)** et **[@meydeey](https://youtube.com/@meydeey)**. Pattern inspiré d'Andrej Karpathy (co-fondateur OpenAI). Retrouve la vidéo qui explique la méthode sur YouTube.

Licence MIT. Fais-en ce que tu veux.
