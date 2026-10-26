# PageRank — du graphe aléatoire au crawl d'un site web

Projet personnel d'exploration de l'algorithme **PageRank** avec Python et NetworkX, en deux temps :

1. **Graphe aléatoire** — génération et visualisation d'un graphe orienté de type Erdős–Rényi.
2. **Cas réel** — crawl du site [ecogestion.univ-paris13.fr](https://ecogestion.univ-paris13.fr/) (UFR Économie-Gestion, Université Sorbonne Paris Nord), construction du graphe des liens internes et calcul du PageRank de chaque page.

Tout le code se trouve dans le notebook [`PageRank.ipynb`](PageRank.ipynb).

## Contenu du notebook

### Partie 1 — Graphe orienté aléatoire

- Tirage aléatoire du nombre de sommets `N ∈ [50, 100]` et de la probabilité d'arête `p ∈ [0.03, 0.07]`
- Génération avec `nx.gnp_random_graph(N, p, directed=True)`
- Statistiques (nombre de sommets, d'arêtes, densité) et liste des arêtes sous forme de DataFrame
- Visualisation avec `spring_layout`

### Partie 2 — Crawl et PageRank d'un site universitaire

| Étape | Description |
|-------|-------------|
| 0 | Imports (`requests`, `BeautifulSoup`, `networkx`, …) |
| 1 | Configuration : URL de départ, domaine autorisé, limite de pages (`MAX_PAGES = 120`), timeout, User-Agent explicite |
| 2 | Fonctions utilitaires : normalisation d'URL, filtre des liens internes, exclusion des `mailto:`/`tel:` et des fichiers lourds (PDF, images, archives…) |
| 3 | Téléchargement des pages avec un délai de politesse aléatoire entre chaque requête |
| 4 | Extraction des **liens de contenu** uniquement : header, footer, menus et widgets sont retirés pour éviter que la navigation commune à toutes les pages domine le graphe |
| 5 | Crawl en largeur (BFS) et construction d'un `DiGraph` pondéré |
| 6 | Calcul du PageRank pondéré (`alpha = 0.90`) et top 10 des pages |
| 7 | Visualisation du graphe du site |

## Exemple de résultat

Sur une exécution (31 pages, 54 liens), la page la plus centrale ressort nettement :

```
Top 10 PageRank (interne) :
0.425593  https://ecogestion.univ-paris13.fr/candidatures
0.021701  https://ecogestion.univ-paris13.fr/masters-formation-initiale-fi
0.021701  https://ecogestion.univ-paris13.fr/licence
0.021701  https://ecogestion.univ-paris13.fr/masters-formation-apprentissage-fa
...
```

Les résultats peuvent varier d'une exécution à l'autre selon l'évolution du site.

## Installation

Python 3.10 ou plus récent est requis.

```bash
git clone https://github.com/filou337/page-ranking.git
cd page-ranking
python -m venv .venv
source .venv/bin/activate   # Windows : .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook PageRank.ipynb
```

## Crawl responsable

Le crawler est volontairement limité :

- il reste sur un seul domaine et s'arrête après 120 pages ;
- il attend entre 0,3 et 0,8 s entre deux requêtes ;
- il s'identifie avec un User-Agent explicite renvoyant vers GitHub ;
- il ignore les fichiers lourds (PDF, images, vidéos, documents Office).

Si vous l'adaptez à un autre site, vérifiez son `robots.txt` et ses conditions d'utilisation.

## Technologies

Python · NetworkX · pandas · Matplotlib · Requests · BeautifulSoup · NumPy · Jupyter

## Licence

Distribué sous licence MIT — voir [LICENSE](LICENSE).
