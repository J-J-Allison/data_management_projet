# Projet Data Management : Morbidité hospitalière en France métropolitaine (2018–2022)

Analyse et visualisation du taux de recours aux établissements de soins de courte durée (MCO) selon le sexe, l'âge et la pathologie, à l'échelle départementale.

## Source des données

Les données proviennent de l'**Agence Technique de l'Information sur l'Hospitalisation (ATIH)**, publiées par la **DREES** (Direction de la Recherche, des Études, de l'Évaluation et des Statistiques).

- **Tableau 1** (`tableau_1.csv`) — ~3,9M observations : taux de recours par pathologie, sexe, âge, année et zone géographique (national, régional, départemental)
- **Tableau 2** (`tableau_2.csv`) — ~1M observations : durée des séjours hospitaliers

## Traitement des données

Le notebook `traitement.ipynb` effectue les opérations suivantes :

1. **Filtrage géographique** — séparation des niveaux national / régional / départemental, exclusion des DOM-TOM
2. **Construction de deux dataframes** à partir du tableau 1 :
   - `df_tot_age` — tous âges confondus, différenciation par sexe (H/F)
   - `df_tranch_age` — par tranche d'âge, sexe « Ensemble »
3. **Imputation des valeurs manquantes** — les valeurs `ND` sont remplacées par la moyenne calculée par département, sexe/tranche d'âge et pathologie sur les années disponibles
4. **Création de variables dérivées** — codes départementaux, noms de pathologies épurés, total des cas par groupe, ratios (par sexe, par tranche d'âge)
5. **Transformation du tableau 2** — passage du format large au format long (`df_sejour`), ajout d'un ratio et d'une variable numérique pour la durée

## Application Streamlit

L'application interactive (`app.py`) propose :

- **Carte choroplèthe** animée du taux de recours par département et par année
- **Détails par département** — répartition par sexe et par tranche d'âge
- **Analyse de la durée de séjour** — histogramme de distribution et courbe de distribution normale (µ, σ)
- **Analyses supplémentaires** — répartition par sexe / âge à l'échelle nationale, total par département
- **WordCloud** — text mining d'un article de presse lié à la morbidité hospitalière (anonymisation, lemmatisation via spaCy, visualisation des mots-clés)

### Lancer l'application

```bash
streamlit run app.py
```

## Stack technique

| Outil | Usage |
|---|---|
| Python / Pandas | Traitement et manipulation des données |
| Plotly Express | Visualisations interactives (cartes, histogrammes, courbes) |
| Streamlit | Interface web interactive |
| SciPy | Distribution normale (durée de séjour) |
| spaCy (`fr_core_news_md`) | NLP — anonymisation, POS tagging, lemmatisation |
| WordCloud / Matplotlib | Visualisation text mining |

## Structure du projet

```
├── traitement.ipynb          # Notebook de traitement des données
├── app.py                    # Application Streamlit
├── tableau_1.csv             # Données brutes — taux de recours
├── tableau_2.csv             # Données brutes — durée des séjours
├── df_tot_age.csv            # Données traitées — par sexe
├── df_tranch_age.csv         # Données traitées — par tranche d'âge
├── df_sejour.csv             # Données traitées — durée des séjours
├── departements.geojson      # GeoJSON des départements français
└── README.md
```

## Auteur

Projet réalisé dans le cadre du DU Data Analytics — Université Paris 1 Panthéon-Sorbonne.
