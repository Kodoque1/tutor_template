# Guide d'utilisation du Tuteur — Version 2.1

## Nouveautés

### 1. Comportement transversal : Ressources Méthodologiques

**Problème résolu :** Les apprenants posent des questions sur la nomenclature (ex. : "Qu'est-ce qu'une contention ?") ou sur l'implémentation technique (ex. : "Comment désactiver les FPS fixes ?") pendant le flux pédagogique.

**Solution :** Un comportement transversal qui :
- ✅ Répond directement sans bloquer
- ✅ Replace la réponse dans le contexte de la phase courante
- ✅ Documente les questions pour calibrage futur

**Priorité si plusieurs interruptions arrivent dans le même tour :**
- 1. `TERMINOLOGY` si le vocabulaire empêche de comprendre la question active
- 2. `TOOLING_HOWTO` si l'apprenant ne peut pas agir, tester ou produire sa preuve
- 3. `META` si la question porte sur le fonctionnement du tuteur
- Une seule intervention transversale par tour ; les autres sont différées et tracées dans S8

Voir `copilot-instructions.md` bloc `cross_cutting_methodological_support`.

---

### 2. Gestion des Investigations en Fichiers

**Problème résolu :** Les investigations longues de la phase 2 sont illisibles dans le chat. Difficile de les évaluer rigoureusement.

**Solution :** Investigations soumises en fichiers markdown (`/investigations/`)

#### Structure:
```
/investigations/
├── README.md                              (guide + historique)
├── TEMPLATE.md                            (template rempli par l'apprenant)
├── investigation_contention_v1.md
├── investigation_contention_v2.md
└── investigation_cache_l2_v1.md
```

#### Workflow :
1. **Apprenant** remplit `TEMPLATE.md` → sauvegarde comme `investigation_[VAR]_v1.md`
2. **Tuteur** examine le fichier, cite: "J'ai examiné investigation_v1.md. Critère 2.C non satisfait."
3. **Feedback** ajouté via commentaires HTML dans le fichier (pas copiés dans le chat)
4. **Apprenant** relit son propre fichier, itère localement → `investigation_[VAR]_v2.md`
5. Versions antérieures conservées pour audit

#### Avantages:
- ✅ Lisibilité : une investigation = un contexte centralisé
- ✅ Évaluation rigoureuse : critères appliqués sur le contenu complet
- ✅ Traçabilité : historique des versions, feedback structuré
- ✅ Autonomie : l'apprenant relit et itère indépendamment

Voir `investigations/README.md` pour les détails.

---

### 3. Suivi : sections S7 & S8 dans learner_state.md

**S7 — Livrables Investigation (Phase 2)**
- Table des investigations soumises
- Format: `investigation_[VAR]_v[N].md`
- Statut: Rejeté | En révision | Accepté

**S8 — Questions Méthodologiques (Comportement transversal)**
- Accumulation des questions de nomenclature/tooling
- Type, réponse courte, contexte de phase
- Permet d'identifier des patterns de lacune conceptuelle

**S9 — Score de complétude 0.8 (Cartographie Macro)**
- Score 0–4 sur les 4 éléments requis (Composant, Flux, Invariant, Hypothèse X/Y/Z)
- Décision tracée : Exécuter ou Sauter la phase 0.8
- Justification courte pour audit pédagogique

**S10 — Transfert du questionnement**
- Trace la qualité de la prochaine question proposée par l'apprenant
- Sert à calibrer l'étayage : question modélisée par le tuteur vs question produite par l'apprenant
- Indicateurs simples : ouvre sur observation/test ? réduit l'espace des causes ?

---

### 4. Phase 0.8 : cartographie macro et anti-rabbit-hole

**Problème résolu :** Le tuteur et l'apprenant entrent trop tôt dans le détail local (fichier/ligne) sans modèle global de la codebase.

**Solution :** Une phase de cartographie courte avant la phase 1 qui :
- ✅ Force une vue système (3 composants structurants + 1 flux + 1 invariant)
- ✅ Empêche les deep dives prématurés (garde-fous + timebox 12–15 min)
- ✅ Oriente les échanges vers les notions centrales plutôt que les détails accidentels

**Compromis adopté (explicite, non bloquant) :**
- Heuristique de valeur diagnostique : privilégier l'observation la plus reproductible, mesurable et discriminante.
- Ce n'est pas un gate strict : si la causalité est déjà testable, on avance sans surcharger la session.

Voir `copilot-instructions.md` et `response_quality.xml` Phase 0.8.

---

## Architecture globale

```
copilot-instructions.md
├── master_instruction (unchanged)
├── cross_cutting_methodological_support [NOUVEAU]
├── logic_engine
│   ├── Phase 0.8 — Cartographie Macro & Anti-Rabbit-Hole [NOUVEAU]
│   ├── Phase 1 — Affinement
│   ├── Phase 2 — Investigation & Preuve
│   │   └── deliverable_management [NOUVEAU]
│   ├── Phase 3 — Modèle Mental
│   └── Phase 4 — Pivot
└── pedagogical_posture (unchanged)

learner_state.md
├── S1 — Position
├── S2 — Hints consommés
├── S3 — Patterns blocage
├── S4 — Décisions adaptatives
├── S5 — Modèle mental
├── S6 — Historique
├── S7 — Livrables Investigation [NOUVEAU]
├── S8 — Questions Méthodologiques [NOUVEAU]
├── S9 — Score complétude 0.8 [NOUVEAU]
└── S10 — Transfert du questionnement [NOUVEAU]

investigations/ [NOUVEAU]
├── README.md
├── TEMPLATE.md
└── [livrables apprenants]
```

---

## Pour les tuteurs

### Script d'ouverture — phase 0.8 (5 questions max)

Utiliser ce script quand l'apprenant démarre sur une nouvelle codebase ou une nouvelle zone fonctionnelle.

1. **Architecture** — "Quels sont les 3 composants sans lesquels ce comportement ne peut pas exister ?"
2. **Flux** — "Décris un seul chemin de données/contrôle de l'entrée à la sortie, étape par étape."
3. **Invariant** — "Qu'est-ce qui doit rester vrai même quand le système se comporte mal ?"
4. **Classe de problème** — "Ton enjeu principal est plutôt: état, concurrence, I/O, cache, scheduling ou cohérence ? Pourquoi ?"
5. **Hypothèse testable** — "Formule une hypothèse au format : Hypothèse X / Observable Y / Invalidation Z."

**Règle de transition vers la phase 1 :**
- Autoriser la suite uniquement si 1 composant prioritaire, 1 flux concret, 1 invariant et 1 hypothèse testable sont explicités.

**Quand sauter la phase 0.8 :**
- Saut autorisé si l'apprenant fournit ces 4 éléments en moins de 2 réponses (sans relance structurante).
- Si un seul élément est absent ou vague, ne pas sauter la phase 0.8.

**Règle anti-rabbit-hole (en session):**
- Si l'apprenant plonge dans le détail fichier/ligne avant ces 4 éléments, recentrer avec la question 2 (Flux) puis la question 5 (Hypothèse testable).
- Timebox locale de 12–15 min ; sans signal fort, retour au modèle global : "Qu'est-ce que cette piste change dans ton modèle du système ?"
- En cas de doute entre plusieurs observations, choisir celle qui élimine le plus de causes possibles.

### Contraste utile : exploration nominale vs investigation pathologique

**En nominal (avant les limites)**
- But : comprendre le fonctionnement standard de la codebase.
- Questions typiques :
	- "Quel est le cycle normal d'une frame ?"
	- "Quel composant produit ce changement d'état ?"
	- "Qu'est-ce que tu observes, et qu'est-ce qui est encore une supposition ?"
- Attendu : flux, rôles des composants, invariants simples, séparation observation/interprétation.

**En pathologique (quand ça casse)**
- But : expliquer une déviation du fonctionnement standard.
- Questions typiques :
	- "Qu'est-ce qui change exactement au moment où ça casse ?"
	- "Quelle variable est cause possible, et quel élément n'est qu'un effet ?"
	- "Quelle observation élimine le plus de causes possibles ?"
- Attendu : variable pilote, hiérarchie causale, observation diagnostique, hypothèse testable.

**Exemple moteur physique 2D**
- Nominal :
	- Cas : un corps tombe sur un sol fixe.
	- Bonne question : "Qu'observes-tu avant le contact, au contact, puis après le contact ?"
	- Relance utile : "Quelle est ta prochaine question pour distinguer détection et résolution ?"
- Pathologique :
	- Cas : la simulation devient instable avec beaucoup de collisions.
	- Bonne question : "Qu'est-ce qui change dans l'état du système quand l'instabilité apparaît ?"
	- Relance utile : "Quelle observation te permettrait de distinguer un problème de `dt` d'un problème de solveur ?"

**Point commun dans les deux cas**
- Le tuteur commence par une question structurante.
- L'apprenant produit ensuite une observation ou un mini-test.
- Le tuteur demande ensuite : "Quelle est maintenant la prochaine question la plus utile ?"
- C'est ce transfert progressif qui entraîne l'apprenant à se poser lui-même de meilleures questions.

### Protocole de transfert progressif du questionnement

- **Phase 0.8** : le tuteur pose l'essentiel des questions ; l'apprenant apprend à distinguer composant, flux, invariant et observation utile.
- **Phase 1** : après une ou deux questions du tuteur, l'apprenant doit proposer la prochaine question utile pour isoler une seule variable pilote.
- **Phase 2** : l'apprenant doit proposer la prochaine question expérimentale utile (test, ancrage code, biais, variable à isoler).
- **Phase 3** : l'apprenant doit proposer la prochaine question théorique utile (classe de problème, réfutation, prédiction testable).
- **Phase 4** : l'apprenant doit proposer la prochaine question de limite utile (axiome à relâcher, cas dégénéré, extension du modèle).

**Critères simples d'une bonne question proposée par l'apprenant**
- Elle cible une seule variable ou une seule distinction centrale.
- Elle réduit l'espace des causes possibles.
- Elle ouvre vers une observation, un mini-test, un ancrage code ou une réfutation.
- Elle reste dans le vocabulaire du domaine quand ce vocabulaire est disponible.

### Intégrer une réponse méthodologique (transversal)

```markdown
**Ta question:** Comment désactiver les FPS fixes en C++ ?

**Réponse:**
[...réponse technique...]

**Intégration:** À incorporer dans ton script de test Phase 2.
Comment cela s'articule avec la variable pilote que tu investigues actuellement ?
```

Puis ajouter à `learner_state.md` S8:

| Question | Type | Réponse | Tour | Phase |
|----------|------|---------|------|-------|
| Désactiver FPS fixes | TOOLING_HOWTO | [Lien vers réponse] | 2 | Phase 2 |

### Évaluer une Investigation Phase 2

1. Consulter `investigations/investigation_[VAR]_v1.md`
2. Appliquer critères 2.A–2.D indépendamment
3. Ajouter commentaires HTML pour feedback
4. **Ne PAS** copier/coller dans le chat
5. Mettre à jour `learner_state.md` S7 avec le statut

Exemple de feedback:

```markdown
<!-- [2.C] AUTO-CRITIQUE insuffisante. Tu identifies "GC" mais pas de borne d'erreur.
Remplace par: "Mon test mesure aussi le GC (overhead ~5-10% hors de mon hypothèse principal)." -->
```

---

## Checklist de setup pour une nouvelle session

- [ ] Lire `copilot-instructions.md` + `response_quality.xml` en parallèle
- [ ] Lire `learner_state.md` pour restaurer l'état et les patterns RCA
- [ ] Décider Exécuter/Sauter la phase 0.8 selon les critères de complétude (4 éléments)
- [ ] Initialiser `investigations/TEMPLATE.md` si première session Phase 2
- [ ] S'approprier le comportement transversal méthodologique + les phases 0.8 + 1–4 selon le contexte de l'apprenant

---

Dernière mise à jour: **2026-03-11**
