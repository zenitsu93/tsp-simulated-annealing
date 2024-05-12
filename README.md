# Voyageur de commerce par recuit simulé

Trouver le plus court circuit passant une fois par chaque ville et revenant au départ. Le problème est NP-difficile : au-delà d'une vingtaine de villes, énumérer tous les circuits est hors de portée. On cherche donc une bonne solution, pas la meilleure.

Le **recuit simulé** s'inspire du refroidissement des métaux. À température élevée, l'algorithme accepte volontiers des circuits plus longs que le courant — ce qui lui permet de sortir d'un minimum local. À mesure que la température baisse, il devient exigeant et ne garde plus que les améliorations.

C'est le cœur de la méthode : **accepter temporairement de faire pire pour finir mieux**. Une descente pure se bloque au premier creux venu.

## Le protocole

1. **Générer des configurations de villes** — disposées en carré, en cercle, ou au hasard.
2. **Partir d'un circuit quelconque.**
3. **Perturber** — échanger deux villes, ou inverser un segment du parcours.
4. **Accepter ou refuser** — toujours si le circuit raccourcit ; avec une probabilité qui décroît avec la température sinon.
5. **Refroidir** progressivement et répéter.

## Résultats

Les configurations géométriques servent de contrôle : sur un cercle, on connaît la solution optimale — c'est le tour du cercle. Elles permettent de vérifier que l'algorithme converge vers ce qu'il devrait, avant de le lâcher sur des dispositions aléatoires où l'optimum est inconnu.

![Configuration en carré](results/image.png)

![Évolution du circuit](results/image-2.png)

![Décroissance de la longueur](results/image-4.png)

![Résultat final](results/image-7.png)

## Contenu du dépôt

| Chemin | Rôle |
| --- | --- |
| `Traveling_Salesman_Problem.ipynb` | Génération, recuit simulé, visualisations |
| `results/` | Figures produites par le carnet |

## Exécution

```bash
pip install numpy matplotlib jupyter
jupyter notebook Traveling_Salesman_Problem.ipynb
```

## Ce qui gouverne la qualité du résultat

Trois réglages font tout, et ils se compensent mal.

La **température initiale** doit être assez haute pour que l'algorithme explore vraiment au départ. Trop basse, il se fige immédiatement dans le voisinage de son point de départ.

La **vitesse de refroidissement** est l'arbitrage principal : refroidir vite donne une réponse rapide et médiocre, refroidir lentement donne une bonne solution et coûte du temps.

Le **type de perturbation** compte davantage qu'on ne le croit. L'inversion de segment est nettement plus efficace que l'échange de deux villes sur ce problème, parce qu'elle défait les croisements du circuit — qui sont précisément ce qui l'allonge.
