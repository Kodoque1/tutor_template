# Guide d'utilisation du Tuteur — Version 2.3

## Architecture centrale : phases et traçabilité

Le tuteur commence chaque session par une question d'intention pour choisir le flux :

> "Qu'est-ce qui t'a amené ici ?"

| Réponse | Flux activé |
|---|---|
| Symptôme, bug, comportement inattendu | Standard : 0.8 → 1 → 2 → 3 → 4 |
| Explorer / comprendre sans problème défini | Exploratoire : E1 → E2 [×N] → E3 |
| Préparer une présentation ou contribution rapide | Allégé : 0.8 + 1 seulement |
| Symptôme **ET** volonté de comprendre l'architecture | Hybride : pré-phase E1 (15 min), puis Standard (E1 remplace MAP si complet) |

**Règle** : ne jamais inférer l'intention depuis la formulation de la question — la demander si ambiguë.

Le flux standard progresse en 5 phases (0.8 → 1 → 2 → 3 → 4) avec trois mécanismes transversaux :

1. **Ressources méthodologiques** — interruptions sans blocage (terminologie, tooling, méta-questions)
2. **Questions progressives** — transfert de la capacité à générer des questions utiles
3. **Efficacité d'investigation** — T1 avant T2 avant T3, coût explicite avant engagement

Chaque session est tracée dans `learner_state.md` (sections S1–S10) ; investigations longues (Phase 2) en fichiers séparés.

---

## 1. Interruptions méthodologiques (transversal)

Les apprenants posent des questions hors-phase sur la nomenclature, l'implémentation technique ou le processus tuteur. Le système les traite sans bloquer le flux.

**Ordre de priorité** (une seule par tour; les autres sont différées à S8) :
1. **TERMINOLOGY** — vocabulaire qui empêche la compréhension active
2. **TOOLING_HOWTO** — action ou preuve bloquée (ex. désactiver FPS fixes)
3. **META** — question sur le processus tuteur lui-même

**Protocole** : Répondre directement, puis recontextualiser ("Comment cela s'articule avec ta Phase [N] ?"). Documenter dans S8 pour audit.

Détail complet : `copilot-instructions.md` → `cross_cutting_methodological_support`

**Régression de phase** : si en Phase N une découverte invalide un modèle construit en Phase M, nommer explicitement la régression ("Ce que tu viens de montrer contredit ton modèle de Phase [M]. On revient en Phase [M] avec cette nouvelle donnée.") et retourner à Phase M — pas à 0.8. Tracer dans S5.

---

## 2. Investigations Phase 2 : fichiers et feedback

**Raison du changement** : Les investigations texte-brut dans le chat sont illisibles et difficiles à évaluer rigoureusement.

**Solution** : Investigations soumises en fichiers markdown (`/investigations/investigation_[VAR]_v[N].md`) avec feedback intégré (commentaires HTML, non copiés dans le chat).

**Workflow** :
1. Apprenant remplit `TEMPLATE.md` → sauvegarde comme `investigation_[VAR]_v1.md`
2. Tuteur vérifie critères 2.A–2.D, ajoute commentaires HTML
3. Apprenant relit, itère localement → `v2.md`, `v3.md`
4. Toutes versions conservées pour audit

**Avantages** : lisibilité centralisée, critères applicables sur le contexte complet, traçabilité, autonomie apprenant.

Détail : `investigations/README.md`

---

## 3. Traçabilité : S7–S10 dans learner_state.md

| Section | Contenu | Utilité |
|---|---|---|
| **S7** | Investigations soumises (statut: rejeté/révision/accepté) | Audit Phase 2 |
| **S8** | Questions méthodologiques (type, réponse, contexte) | Identifier patterns lacunaires |
| **S9** | Score complétude 0.8 (0–4) + décision Exécuter/Sauter | Décision objective phase 0.8 |
| **S10** | Qualité questions apprenant (ouvre sur observation ? réduit causes ?) | Calibrage étayage |
| **S11** | Intentions session suivante (2–3, format : phase courante + actions concrètes) | Reprise immédiate sans reconstruction à froid |

---

## 4. Phase 0.8 : cartographie macro et anti-rabbit-hole

**Objectif** : Construire une vue système avant d'ouvrir le détail. Éviter les deep dives non guidées.

**Livrable requis** (4 éléments) :
1. **Composant** — 3 éléments structurants
2. **Flux** — 1 chemin de données/contrôle de bout en bout
3. **Invariant** — 1 propriété qui reste vraie même si ça casse
4. **Hypothèse testable** — format : Hypothèse X / Observable Y / Invalidation Z

**Règle de transition** : Autoriser Phase 1 ssi 4 éléments explicités. Sauter 0.8 ssi fournis en < 2 réponses.

**Heuristiques non bloquantes** :
- **Discriminance** (G6) : privilégier l'observation qui réduit le plus l'espace des causes
- **Efficacité** (G7) : T1 (log/print) avant T2 (mini-test) avant T3 (refactoring)

Détail complet → `copilot-instructions.md` Phase 0.8, `response_quality.xml` Phase 0.8

---

## 5. Mode exploratoire : E1 → E2 [×N] → E3

**Quand l'activer** : l'apprenant veut comprendre une codebase sans problème terminal défini (onboarding, discovery, préparation à contribution).

**Différence clé avec le flux standard** : pas de variable pilote à isoler, pas de reject_triggers actifs — toute observation qui structure la compréhension a de la valeur.

| Phase | Durée indicative | Objectif | Critère minimal |
|---|---|---|---|
| **E1** — Cartographie libre | 15–20 min | Identifier premiers chemins et composants structurants | 1 chemin + 1 composant central + 1 question à approfondir |
| **E2** — Excavation ciblée | 10–15 min (×3–5) | Approfondir une arête choisie par l'apprenant | 1 observation + 1 mini-modèle local |
| **E3** — Synthèse | 10–15 min | Nommer le pattern central et la philosophie architecturale | Pattern nommé + 2 comportements E2 expliqués par ce pattern |

**Questions types du tuteur en E2** :
- "Qu'observes-tu au moment où [X] se produit ?"
- "Ce comportement est-il une contrainte de conception ou une conséquence accidentelle ?"
- "Où ce composant transmet-il la responsabilité à un autre ?"
- "Quelle est la prochaine arête que tu veux explorer depuis ici ?"

**Bascule vers flux standard** : si en E3 l'apprenant détecte un comportement anormal, basculer vers Phase 0.8 avec la carte E1/E2 comme point de départ — elle remplace le MAP_1/MAP_2/MAP_3 qui auraient été construits normalement.

**Traçabilité allégée** : S1 (position) et S5 (modèle mental) seulement. Pas de S9 ni S10.

---

## Pour les tuteurs

### Phase 0.8 : script d'ouverture (5 questions max)

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

### Protocole de transfert progressif du questionnement

Progression par phase de la responsabilité apprenant pour générer la prochaine question utile :

- **Phase 0.8** : tuteur pose questions ; apprenant apprend à distinguer composant, flux, invariant
- **Phase 1–4** : apprenant génère la prochaine question (isoler variable pilote → expérimentale → théorique → limite)

**Critères d'une bonne question** : cible une seule variable, réduit l'espace des causes, ouvre sur observation/test/réfutation, formulée dans le vocabulaire du domaine, coût d'investigation minimisé (T1→T2→T3).

### Clôture de session : protocole S11

En fin de session (annonce de départ ou 60+ min sans nouveau deliverable) :

1. Résumer en 1 phrase ce qui a été établi (phase atteinte, modèle courant)
2. Formuler 2–3 intentions concrètes pour la session suivante (ex: "Phase 2, tester l'hypothèse H")
3. Écrire dans `learner_state.md` S11 : `S11 | [date] | Phase : [N] | Établi : [...] | Intentions : (a) [...] (b) [...] (c) [...]`

S11 est lu en premier à la session suivante, avant la question d'intention.

### Efficacité d'investigation : T1, T2, T3

Une question reste coûteuse si son investigation l'est. **Règle** : épuiser T1 (< 2 min : logs, print, compteur) avant T2 (5–15 min : mini-test ciblé) avant T3 (> 15 min : refactoring). Ne monter de tier que si le tier actuel est clairement non discriminant.

**Guidage tuteur** : " Existe-t-il déjà dans les logs (T1) ce qu'il te faudrait ? " ou " Quelle version minimale de ce test suffit ? "

**Exemple** : "Je vais mesurer l'énergie sur 10k frames" (T3 direct) vs "D'abord : le `dt` affiché est constant ?" (T1) → mini-test fixe (T2) → banc complet (T3).

---

## Procédures pour le tuteur

### Répondre à une interruption méthodologique

Répondre directement ("D'abord, [réponse]"), puis recontextualiser la réponse dans la phase active. Documenter dans S8 (`learner_state.md`) :

```
| Désactiver FPS fixes | TOOLING_HOWTO | [résumé réponse] | Tour N | Phase M |
```

Exemple : "Comment désactiver les FPS fixes en C++ ?" → [réponse technique] → "À incorporer dans ton test Phase 2. Comment cela change ta variable pilote ?"

### Évaluer une Investigation Phase 2

1. Consulter `investigations/investigation_[VAR]_v[N].md`
2. Appliquer critères 2.A–2.D (hypothèse, observable, test, auto-critique)
3. Ajouter commentaires HTML (pas de copie chat)
4. Mettre à jour S7 (`learner_state.md`) : Rejeté | Révision | Accepté

Feedback compact :
```
<!-- [2.C] AUTO-CRITIQUE absente : mesure GC mais pas borne.
Reformule : overhead GC ~5-10% hors hypothèse principale. -->
```

---

## Checklist de setup pour une nouvelle session

- [ ] Lire `copilot-instructions.md` + `response_quality.xml` en parallèle
- [ ] Lire `learner_state.md` pour restaurer l'état et les patterns RCA
- [ ] Si S11 renseigné : lire S11 en priorité pour reprendre les intentions de la session précédente
- [ ] Poser la question d'intention : "Qu'est-ce qui t'a amené ici ?" → choisir flux (standard / exploratoire / allégé)
- [ ] Si flux standard : décider Exécuter/Sauter la phase 0.8 selon les critères de complétude (4 éléments)
- [ ] Si mode exploratoire : activer S1 + S5 seulement, lancer E1
- [ ] Initialiser `investigations/TEMPLATE.md` si première session Phase 2

---

Dernière mise à jour: **2026-03-11**
