# Guide d'utilisation du Tuteur — Version 2.1

## Nouveautés

### 1. Comportement transversal : Ressources Méthodologiques

**Problème résolu:** Les apprenants posent des questions sur la nomenclature (ex: "Qu'est-ce qu'une contention ?") ou sur l'implémentation technique (ex: "Comment désactiver les FPS fixes ?") pendant le flux pédagogique.

**Solution:** Un comportement transversal qui:
- ✅ Répond directement sans bloquer
- ✅ Replaced la réponse dans le contexte de la phase courante
- ✅ Documente les questions pour calibrage futur

**Priorité si plusieurs interruptions arrivent dans le même tour:**
- 1. `TERMINOLOGY` si le vocabulaire empêche de comprendre la question active
- 2. `TOOLING_HOWTO` si l'apprenant ne peut pas agir, tester ou produire sa preuve
- 3. `META` si la question porte sur le fonctionnement du tuteur
- Une seule intervention transversale par tour ; les autres sont différées et tracées dans S8

Voir `copilot-instructions.md` bloc `cross_cutting_methodological_support`.

---

### 2. Gestion des Investigations en Fichiers

**Problème résolu:** Les investigations longues de Phase 2 sont illegibles inline. Difficile d'évaluer rigoureusement via chat.

**Solution:** Investigations soumises en fichiers markdown (`/investigations/`)

#### Structure:
```
/investigations/
├── README.md                              (guide + historique)
├── TEMPLATE.md                            (template rempli par l'apprenant)
├── investigation_contention_v1.md
├── investigation_contention_v2.md
└── investigation_cache_l2_v1.md
```

#### Workflow:
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

### 3. Tracking: Sections S7 & S8 dans learner_state.md

**S7 — Livrables Investigation (Phase 2)**
- Table des investigations soumises
- Format: `investigation_[VAR]_v[N].md`
- Statut: Rejeté | En révision | Accepté

**S8 — Questions Méthodologiques (Comportement transversal)**
- Accumulator des questions de nomenclature/tooling
- Type, réponse courte, contexte de phase
- Permet identifier patterns de lacune conceptuelle

**S9 — Score de complétude 0.8 (Cartographie Macro)**
- Score 0–4 sur les 4 éléments requis (Composant, Flux, Invariant, Hypothèse X/Y/Z)
- Décision tracée : Exécuter ou Sauter la phase 0.8
- Justification courte pour audit pédagogique

---

### 4. Phase 0.8 : Cartographie Macro & Anti-Rabbit-Hole

**Problème résolu:** Le tuteur et l'apprenant entrent trop tôt dans le détail local (fichier/ligne) sans modèle global de la codebase.

**Solution:** Une phase de cartographie courte avant Phase 1 qui:
- ✅ Force une vue système (3 composants structurants + 1 flux + 1 invariant)
- ✅ Empêche les deep dives prématurés (garde-fous + timebox 12–15 min)
- ✅ Oriente les échanges vers les notions centrales plutôt que les détails accidentels

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
└── S9 — Score complétude 0.8 [NOUVEAU]

investigations/ [NOUVEAU]
├── README.md
├── TEMPLATE.md
└── [livrables apprenants]
```

---

## Pour les tuteurs

### Script d'ouverture — Phase 0.8 (5 questions max)

Utiliser ce script quand l'apprenant démarre sur une nouvelle codebase ou une nouvelle zone fonctionnelle.

1. **Architecture** — "Quels sont les 3 composants sans lesquels ce comportement ne peut pas exister ?"
2. **Flux** — "Décris un seul chemin de données/contrôle de l'entrée à la sortie, étape par étape."
3. **Invariant** — "Qu'est-ce qui doit rester vrai même quand le système se comporte mal ?"
4. **Classe de problème** — "Ton enjeu principal est plutôt: état, concurrence, I/O, cache, scheduling ou cohérence ? Pourquoi ?"
5. **Hypothèse testable** — "Formule une hypothèse en format: Hypothèse X / Observable Y / Invalidation Z."

**Règle de transition vers Phase 1:**
- Autoriser la suite uniquement si 1 composant prioritaire, 1 flux concret, 1 invariant et 1 hypothèse testable sont explicités.

**Quand sauter la Phase 0.8:**
- Saut autorisé si l'apprenant fournit ces 4 éléments en moins de 2 réponses (sans relance structurante).
- Si un seul élément est absent ou vague, ne pas sauter 0.8.

**Règle anti-rabbit-hole (en session):**
- Si l'apprenant plonge dans le détail fichier/ligne avant ces 4 éléments, recentrer avec la question 2 (Flux) puis la question 5 (Hypothèse testable).
- Timebox locale de 12–15 min; sans signal fort, retour au modèle global: "Qu'est-ce que cette piste change dans ton modèle du système ?"

### Intégrer une réponse Méthodologique (transversal)

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
- [ ] Lire `learner_state.md` pour restaurer l'état et patterns RCA
- [ ] Décider Exécuter/Sauter Phase 0.8 selon les critères de complétude (4 éléments)
- [ ] Initialiser `investigations/TEMPLATE.md` si première session Phase 2
- [ ] S'approprier le comportement transversal méthodologique + les phases 0.8 + 1–4 selon le contexte de l'apprenant

---

Dernière mise à jour: **2026-03-11**
