# Mes Outils d'Apprentissage

Ensemble d'outils d'apprentissage interactifs du Primaire au Lycee, deployes sur GitHub Pages. L'objectif est d'evaluer et renforcer les sujets faibles en croisant notes de controle et scores de QCM.

## Structure

```
learner/
├── index.html                              # Page d'accueil (onglets par niveau)
├── notes.html                              # Saisie des notes par matiere/chapitre
├── resultats.html                          # Bilan de progression (notes vs QCM vs examen)
│
├── primaire/
│   ├── maths/
│   │   ├── tables-multiplication.html      # Tables 1-12 + mode defi
│   │   ├── calcul-mental.html              # Calcul adaptatif CP→CM2
│   │   └── manifest.json
│   └── francais/
│       ├── homophones.html                 # a/à, et/est, on/ont, etc.
│       ├── conjugaison.html                # 4 temps, 25 verbes
│       └── manifest.json
│
├── 3eme/
│   ├── brevet-examen-500.html              # Grand examen 500 QCM
│   ├── maths/
│   │   ├── calcul-mental.html              # Fractions, puissances, racines
│   │   ├── equations.html                  # Equations 1er degre → produit nul
│   │   └── manifest.json
│   ├── francais/
│   │   ├── conjugaison.html                # 8 temps, 25 verbes, mode Brevet
│   │   ├── figures-de-style.html           # 14 figures, 60+ extraits
│   │   └── manifest.json
│   ├── anglais/
│   │   ├── verbes-irreguliers.html         # 10 sessions + quiz
│   │   ├── grammar-drills.html             # 6 points de grammaire, 90+ phrases
│   │   └── manifest.json
│   ├── histoire-geo/
│   │   ├── frise-chrono.html               # Evenements 1914-2002
│   │   ├── reperes-brevet.html             # Flashcards dates/geo/EMC
│   │   └── manifest.json
│   └── sciences/
│       ├── qcm-diagnostique.html           # 45 QCM : SVT + PC + Techno
│       └── manifest.json
│
├── 2nde/
│   └── maths/
│       ├── automatismes.html               # 5 themes, exercices flash
│       └── manifest.json
│
├── 1ere/
│   ├── maths/
│   │   ├── derivees.html                   # 3 niveaux, pas-a-pas
│   │   └── manifest.json
│   └── nsi/
│       ├── python-debug.html               # Trouver le bug, 25 scenarios
│       └── manifest.json
│
├── terminale/
│   └── philosophie/
│       ├── concepts.html                   # 17 notions, auteurs, citations
│       └── manifest.json
│
├── 6eme/, 5eme/, 4eme/                     # Dossiers prets (a venir)
└── .gitignore
```

## Activites par niveau

### Primaire (CP → CM2)

| Code | Activite | Matiere | Description |
|------|----------|---------|-------------|
| `p_tables` | Tables de multiplication | Maths | Tables 1-12 progressives + mode defi 50 questions |
| `p_calcul` | Calcul mental express | Maths | 4 niveaux adaptatifs (CP→CM), 20 questions |
| `p_homo` | Homophones battle | Francais | 80+ phrases, a/a, et/est, on/ont, etc. |
| `p_conj` | Conjugaison express | Francais | 25 verbes, 4 temps, saisie libre |

### College — 3eme (Brevet)

| Code | Activite | Matiere | Description |
|------|----------|---------|-------------|
| `exam500` | Examen Brevet 500 | Toutes | 500 QCM toutes matieres, chrono, anti-triche |
| `c_calcul` | Calcul mental chrono | Maths | Fractions, puissances, racines, 4 niveaux (6e→3e) |
| `c_equations` | Equations express | Maths | 3 niveaux : ax+b=c → produit nul |
| `c_conj` | Conjugaison battle | Francais | 8 temps du brevet, 25 verbes |
| `c_figures` | Figures de style | Francais | 14 figures, 60+ extraits litteraires |
| `vm3` | Verbes irreguliers | Anglais | 10 sessions progressives + quiz |
| `c_grammar` | Grammar drills | Anglais | 6 points de grammaire, 90+ phrases |
| `c_frise` | Frise chronologique | Histoire | Evenements 1914-2002, glisser-deposer |
| `c_reperes` | Reperes du brevet | Histoire-Geo | 60+ flashcards dates/geo/EMC |
| `c_sciences` | QCM diagnostique | Sciences | 45 QCM : SVT, Physique-Chimie, Techno |

### Lycee

| Code | Activite | Matiere | Niveau | Description |
|------|----------|---------|--------|-------------|
| `l_auto` | Automatismes maths | Maths | 2nde | 5 themes, exercices flash de debut de cours |
| `l_deriv` | Derivees express | Maths | 1ere | 3 niveaux, derivees de base → composees |
| `l_python` | Python debugger | NSI | 1ere | 25 bugs a trouver et corriger |
| `l_philo` | Concepts cles | Philo | Terminale | 17 notions du bac, auteurs, citations |

## Systeme de resultats

### Principe

Chaque activite genere un **code base64** a la fin d'un quiz. L'eleve copie ses codes et les colle dans `resultats.html` pour obtenir un bilan croise.

3 types de codes :

**Code QCM** (toutes activites) :
```json
{"v":"c_calcul","dt":"31/03/2026","score":"20/25","pct":80,"stars":2}
```

**Code Notes** (page notes.html) :
```json
{"v":"notes","dt":"31/03/2026","subjects":{"maths":{"avg":12.5,"chapters":[...]}}}
```

**Code Examen** (brevet 500) :
```json
{"v":"exam500","d":"31/03/2026","score":"380/500","pct":76,"hack":0,"subj":{...}}
```

### Bilan de progression (resultats.html)

Croise notes + QCM + examen et affiche :
- Carte radar superposant les 3 sources par matiere
- Tableau comparatif avec diagnostic (Acquis / A renforcer / Prioritaire)
- Detection des incoherences notes vs QCM
- Matieres et chapitres prioritaires
- Conseils personnalises
- Historique chronologique

## Ajouter une activite

1. Creer le fichier `.html` dans `{niveau}/{matiere}/`
2. Generer un code base64 a la fin :
   ```js
   const code = btoa(unescape(encodeURIComponent(JSON.stringify({
     v: 'identifiant', dt: new Date().toLocaleDateString('fr-FR'),
     score: correct+'/'+total, pct: Math.round(correct/total*100), stars: stars
   }))));
   ```
3. Ajouter dans `manifest.json` du dossier :
   ```json
   [{"title": "Nom affiché", "file": "fichier.html"}]
   ```
4. Ajouter dans `ACTIVITY_META` de `resultats.html` :
   ```js
   'identifiant': { name: 'Nom', subject: 'matiere' }
   ```

## Deploiement

GitHub Pages — pousser sur la branche principale.
