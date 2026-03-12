<!--
TEMPLATE — Investigation Phase 2
==================================
Copie ce fichier et remplace les [PLACEHOLDERS] par tes réponses.
Sauvegarde sous : investigation_[VARIABLE_PILOTE]_v1.md
Puis soumet ce fichier au tuteur.
-->

# Investigation: [NOM_VARIABLE_PILOTE]

---

## ARG — Argumentation

**Hypothèse:**
[Énonce ton hypothèse en UNE SEULE phrase. Ex. : \"La contention sur le verrou mutex provoque l'augmentation du temps de réponse quand le nombre de threads dépasse 4.\"]

**Lien code:**
```
Fichier: [path/to/relevant/file.ext]
Ligne: [line_number]
Contexte: [3-4 lignes de code autour]
```

**Logique:**
[2-3 phrases expliquant pourquoi ce code supporte ton hypothèse. Sois concret, pas général.]

---

## CODE — Script de test minimal

```c
// Remplace [EXT] par ton langage (c, java, py, etc.)
// Ce script doit ISOLER ta variable pilote et rien d'autre.

[CODE DE TON TEST MINIMAL]
```

**Isolation:**
- Variables mesurées : [Liste ce que tu mesures]
- Variables qu'on peut modifier : [Liste les FREE PARAMETERS]
- Variables que tu fixes volontairement : [Liste ce que tu contrôles]

---

## CRITIQUE — Auto-critique

**Biais identifiés et gérés:**

1. **Biais n°1:** [Décris un biais que ton protocole introduit potentiellement]
   → **Gestion:** [Comment tu l'élimines ou tu le bornes ? Borne d'erreur : ±X% ?]

2. **Biais n°2:** [Autre biais]
   → **Gestion:** [Comment tu le gères ?]

**Overhead protocolaire:**
[Réfléchis : Qu'est-ce qui dans TON TEST MÊME pourrait invalider tes résultats si ton hypothèse est correcte ?
Ex: \"Mon benchmark mesure aussi le garbage collection\" ou \"Je crée une file d'attente artificielle en allocation mémoire\"
=> Sois précis et chiffré si possible.]

---

## Résultats (optionnel pour v1)

<!-- Remplis seulement après avoir lancé le test -->

```
[Résultats bruts ou synthèse]
```

---

<!-- Feedback tuteur ci-dessous (NE PAS REMPLIR) -->
## Feedback

<!-- [Critère] Commentaire du tuteur -->
