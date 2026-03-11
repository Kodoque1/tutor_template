# Investigations — Phase 2

## But

Ce dossier centralise les investigations soumises pour la **Phase 2 : Investigation & Preuve**.

Chaque investigation est un fichier markdown indépendant qui documente:
- **ARG** : Argumentation (lien code → hypothèse)
- **CODE** : Script de test minimal
- **CRITIQUE** : Auto-critique du protocole de test

## Nommage

```
investigation_[NOM_VARIABLE_PILOTE]_v[VERSION].md
```

Exemples:
- `investigation_contention_verrou_v1.md`
- `investigation_cache_l2_v2.md`
- `investigation_gc_overhead_v1.md`

## Versionning

Chargue itération (pushback) génère une nouvelle version:
- `_v1` : Première soumission
- `_v2` : Après premier pushback
- `_v3` : Après deuxième pushback
- etc.

Garder les versions précédentes pour audit.

## Feedback

Le tuteur ajoute des commentaires HTML directement dans ton fichier:

```markdown
<!-- [2.C] Critique insuffisante : tu identifies le GC mais pas la borne d'erreur. -->
```

## Structure (template)

```markdown
# Investigation: [Variable Pilote]

## ARG — Argumentation

**Hypothèse:** [Une phrase claire]

**Lien code:**
- Fichier: `path/to/file.c`
- Ligne: XXX
- Extrait: [code snippet]

**Logique:** [Pourquoi cet ancrage code supporte l'hypothèse]

---

## CODE — Script de test

\`\`\`c
// Ton script minimal ici
\`\`\`

**Isolation:** [Quelles variables tu isoles ? Lesquelles tu élimines ?]

---

## CRITIQUE — Auto-critique

**Biais identifiés:**
1. [Biais 1] → Correction: [comment tu le gères]
2. [Biais 2] → Borne d'erreur: [±X%]

**Overhead protocolaire:** [Quoi dans TON TEST pourrait invalider tes résultats même si ton hypothèse est correcte ?]
```

---

## Historique

| Fichier | Statut | Tour | Notes |
|---------|--------|------|-------|
| — | — | — | — |
