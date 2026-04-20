# Guide d'utilisation

> Comment transformer Claude Code en générateur autonome de vault Obsidian, pas à pas. De l'installation jusqu'au run pilote fini.

---

## 1. Prérequis (5 min)

Tu as besoin de :

- **Obsidian** installé : [obsidian.md](https://obsidian.md) (gratuit)
- **Claude Code** installé : [claude.com/claude-code](https://claude.com/claude-code)
- Un compte Anthropic avec un peu de budget (un pilote coûte 10-30€ en tokens)
- 1h à 2h devant toi pour le run pilote

Tu n'as **pas besoin** de Dataview, de plugins payants, ou de config spéciale d'Obsidian. Le prompt génère un vault utilisable out of the box.

---

## 2. Choisir ton sujet (10 min de réflexion)

Le pilote teste la méthode sur **1 cluster** d'un domaine plus large. Avant de lancer, pose-toi ces questions :

**Quel est le sujet global que tu veux compiler ?**

Exemples : psychologie, marketing, crypto, philosophie, nutrition, développement perso, histoire de l'art, théorie économique, méthodes d'apprentissage.

**Quel cluster tu attaques en premier ?**

Le cluster doit être :
- Assez riche pour générer 30-50 pages (pas trop étroit)
- Assez cadré pour ne pas exploser en 200 pages (pas trop large)
- Un domaine où tu **sauras juger la qualité** à la relecture (sinon le checkpoint humain ne sert à rien)

Exemples de bons clusters pilote :
- Psychologie → Biais cognitifs
- Marketing → Psychologie d'achat
- Crypto → Consensus mechanisms
- Philosophie → Éthique des vertus
- Nutrition → Glucides et métabolisme

Exemples de mauvais clusters :
- Psychologie → Tout Freud (trop large)
- Crypto → Bitcoin (trop étroit, 10 pages max)
- Philosophie → La pensée (inquantifiable)

---

## 3. Remplir les variables (2 min)

Ouvre `PROMPT-PILOT.md`. Tout en haut, il y a un bloc "VARIABLES À REMPLIR". Voici comment les remplir :

### `SUJET`

Le domaine global. Une ou deux mots. Exemple : `"Psychologie"`.

Tu n'auras **pas** à le retaper 40 fois, c'est juste pour que Claude situe le périmètre.

### `CLUSTER_CHOISI`

Le cluster précis du run. Exemple : `"Biais cognitifs"`.

C'est ce qui apparaîtra dans le YAML de chaque page, dans le nom du MOC, et dans le graph view.

### `DESCRIPTION_CLUSTER`

2-4 phrases qui cadrent le périmètre. Dis explicitement :
- **Ce qu'il y a dedans** : quelles familles, quelles époques, quels auteurs
- **Ce qu'il y a à côté qu'il ne faut pas confondre**
- **Le niveau de rigueur attendu** (grand public, académique, recherche de pointe)

Exemple bien rempli :

```
DESCRIPTION_CLUSTER: "Erreurs systématiques de jugement documentées par la
recherche en psychologie cognitive, de Kahneman/Tversky à aujourd'hui.
Inclure biais classiques, biais débattus (réplicabilité incertaine),
meta-biais (biais sur les biais). Exclure les biais purement motivationnels
(freudiens, psychanalytiques) et les biais de perception sensorielle
(illusions d'optique). Niveau : recherche académique mais accessible."
```

### `CHEMIN_VAULT`

Le chemin absolu où le vault sera créé. Typiquement sous ton dossier Obsidian :

```
CHEMIN_VAULT: "/Users/tonuser/Obsidian/pilot-psycho-biais"
```

**Important** : le dossier doit **ne pas exister**, ou être vide. Claude Code va le créer et y écrire. Sinon il va s'arrêter ou écraser.

### `NB_PAGES_CIBLE`

Laisse `40` par défaut. La fourchette acceptable est 30-50.

- **Moins de 30** : pas assez dense pour un graph view intéressant
- **Plus de 50** : risque de dépasser les 2h de run, qualité qui baisse

---

## 4. Lancer Claude Code (30 sec)

Dans ton terminal, va dans ton dossier de travail puis :

```bash
claude
```

Attends que Claude Code démarre, puis colle tout le contenu de `PROMPT-PILOT.md` (avec tes variables remplies) dans la fenêtre.

Claude va :
1. Confirmer le sujet, le cluster, le chemin
2. Lancer Phase 0 (bootstrap du vault, 5 min)
3. Lancer Phase 1 (cartographie des pages, 15-20 min)
4. **S'arrêter au Checkpoint 1** et te demander de valider le plan

---

## 5. Checkpoint 1 : relire le plan (10-15 min)

Ouvre le fichier `99-Meta/Plan.md` que Claude vient de créer. Tu vas y voir 35-50 pages proposées avec :

- Titre
- Type (concept, personne, expérience, controverse, etc.)
- Priorité (1 = pilier, 2 = standard, 3 = deep cut)
- Résumé 2 phrases
- Wikilinks pressentis
- Niveau de controverse
- Fact-check requis oui/non

**Ce que tu dois vérifier** :

- [ ] Les **piliers incontournables** du cluster sont présents (ex : Dunning-Kruger, biais de confirmation, effet Barnum dans "biais cognitifs")
- [ ] Il y a des **pages controverses** (théories débunkées, débats en cours)
- [ ] Il y a des **pages-ponts** vers d'autres domaines (ex : un biais qui touche au marketing)
- [ ] L'**équilibre des types** tient (pas 40 concepts et 0 personne)
- [ ] **Aucune page redondante** évidente

**Ta réponse** :

- Si tout est bon : `GO`
- Si tu veux ajouter/retirer : `CORRIGE : [instructions précises]`. Exemple : `CORRIGE : ajoute une page sur l'effet Pygmalion, retire la page sur X qui fait doublon avec Y, et crée une page sur Kahneman en tant que personne`.

---

## 6. Phase 2 : les 8 pages pilotes (20-30 min)

Claude rédige les 8 premières pages priorité 1. Pour chaque page sensible (personne, expérience, controverse), il lance un WebSearch pour vérifier les faits.

Pendant ce temps, tu peux aller prendre un café.

---

## 7. Checkpoint 2 : relire 2-3 pages (15-20 min)

Ouvre **au hasard** 2-3 pages du dossier `pages/`. Pour chacune :

**Ce que tu cherches** :

- [ ] **Longueur** : 800-1500 mots
- [ ] **Wikilinks** : au moins 7 dans le corps du texte
- [ ] **Sections** : Définition / Contexte / Mécanismes / Nuances / Liens / Sources
- [ ] **Ton** : neutre mais tranché, pas de verbosité inutile
- [ ] **Français** : pas de tiret cadratin, accents complets
- [ ] **Sources** : footnotes en bas de page pour les faits non-évidents
- [ ] **Statut YAML** : `verified` si sourcé, `to-verify` si WebSearch n'a rien trouvé

**Test final** : est-ce que tu apprendrais quelque chose en lisant cette page si tu ne connaissais pas le sujet ? Est-ce que c'est plus dense qu'un article Wikipedia moyen ?

**Ta réponse** :

- Si la qualité est bonne : `GO`
- Si tu veux ajuster : `AJUSTE : [instructions]`. Exemples : `AJUSTE : les sections "Nuances" sont trop courtes, demande-leur 3 paragraphes chacune`, `AJUSTE : les wikilinks sont trop peu contextualisés, intègre-les dans des phrases complètes`
- Si c'est catastrophique : `STOP` et tu relances avec un prompt ajusté

---

## 8. Phase 3 : le reste du cluster (45-60 min)

Claude enchaîne les pages restantes par batches de 8-10. Pas de checkpoint humain, il roule.

Pendant ce temps, tu peux vaquer à d'autres occupations. **Garde un œil sur la console** pour voir s'il ne plante pas.

**Règle stricte** : s'il dépasse 2h de wall time total, il s'arrête et fait un rapport d'avancement. Mieux vaut 35 pages excellentes que 50 pages bâclées.

---

## 9. Phases 4-5-6 : linking, audit, debrief (30-40 min)

Claude :
1. Crée le `_MOC.md` (table des matières navigable)
2. Densifie les wikilinks (parcours toutes les pages)
3. Produit `99-Meta/Audit.md` (doublons, orphelines, anémiques, contradictions)
4. Produit `99-Meta/Debrief.md` (le livrable critique pour scaler ensuite)

---

## 10. Ouvrir dans Obsidian (1 min)

```
Ouvre Obsidian puis Open folder as vault puis sélectionne {CHEMIN_VAULT}
```

Puis dans Obsidian :
- **Graph view** : icône graph à gauche, ou Ctrl+G. Tu vois ton cluster en réseau.
- **Color by type** : ouvre `.obsidian/graph.json` déjà pré-configuré, sinon configure-le à la main (concept=bleu, personne=vert, expérience=orange, controverse=rouge)

**Premier test** : clique sur une page au hasard, regarde les backlinks. Si tu as ≥ 3 backlinks par page en moyenne, le compound commence.

---

## 11. Debrief et scaling (le vrai livrable)

Ouvre `99-Meta/Debrief.md`. C'est **le fichier le plus important** du run. Il contient :

- Stats finales (pages, temps, tokens, WebSearch, wikilinks)
- Ce qui a bien marché
- Ce qui a mal marché
- Hallucinations détectées (que toi ou Claude avez corrigées)
- Ajustements recommandés pour le prompt de production
- Estimation coût et temps pour scaler à 500 pages

C'est ce fichier qui te sert à **rédiger le prompt de production** (multi-agent, scaling x10) en l'adaptant à ton retour d'expérience.

---

## 12. Erreurs courantes

### "Claude a halluciné des noms propres"

Tu le verras dans `99-Meta/Fact-Check-Log.md` section "Auto-corrections". Si tu en vois beaucoup non corrigées, renforce la règle WebSearch dans ton prompt de production : **fact-check systématique pour tout nom propre**, pas seulement les personnes/expériences/controverses.

### "Le graph view est trop dense ou trop clairsemé"

Trop dense (pas de clusters visibles) : les tags manquent, ou les wikilinks sont trop génériques. Ajuste la règle "wikilinks justifiés par la phrase qui les entoure".

Trop clairsemé (pages isolées) : la Phase 4 n'a pas assez densifié. Relance-la manuellement en demandant à Claude d'ajouter 3-5 wikilinks par page.

### "Certaines pages sont courtes"

Vérifie la page `99-Meta/Audit.md` section "Pages anémiques". Pour chacune, demande à Claude de la re-rédiger en respectant la fourchette 800-1500 mots.

### "J'ai peur de brûler trop de tokens"

- Le pilote coûte 10-30€ selon la longueur des pages
- Si tu as peur, lance un sous-pilote avec `NB_PAGES_CIBLE = 20` d'abord
- Le scaling à 500 pages coûte 150-400€ selon le domaine

---

## 13. Adapter à n'importe quel domaine

La méthode marche sur **tout domaine documenté**, avec ces ajustements :

| Domaine | Ajustement |
|---|---|
| Psychologie | Fact-check renforcé sur réplicabilité (50% des études pré-2015 non-reproduites) |
| Crypto | WebSearch obligatoire sur prix/chiffres (ça bouge) |
| Marketing | Dates des études importantes (évolution des tactiques) |
| Philosophie | Moins de fact-check, plus de croisement entre écoles |
| Histoire | Double-sourcing systématique sur dates |
| Médecine | **NON RECOMMANDÉ** sans expertise médicale pour valider |
| Droit | **NON RECOMMANDÉ** sans expertise juridique (juridiction-dépendant) |

---

## 14. Next steps

Une fois ton pilote validé :

1. **Lis le Debrief.md** (15 min)
2. **Ajuste le prompt** en fonction de ton retour
3. **Rédige le prompt de production** (scaling à 500 pages, multi-agent)
4. **Lance le scaling** sur le sujet complet

Le prompt de production n'est **pas fourni ici volontairement** : il doit être calibré sur **ton** domaine, **ton** niveau d'exigence, **ton** budget. Le pilote te donne tout ce qu'il faut pour l'écrire intelligemment.

---

## Besoin d'aide ?

- Chaîne YouTube : [@meydeey](https://youtube.com/@meydeey)
- Skool gratuit : [skool.com/automatisations-ia-gratuit-1412](https://www.skool.com/automatisations-ia-gratuit-1412/about)
- Programme complet : [meydeey.com/labo](https://meydeey.com/labo)
