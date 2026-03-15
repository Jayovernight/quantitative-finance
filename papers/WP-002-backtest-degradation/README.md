# WP-002 — "Votre backtest vous ment (probablement)."

**Auteur** : J. Jacques F.
**Date** : Mars 2026
**Statut** : Valide V32 (FINAL)
**Visuel** : [degradation.png](degradation.png)

---

Votre backtest vous ment (probablement).

La plupart des backtests qu'on voit passer sont faux, pas par malveillance, par construction. Et superposer deux indices sur 20 ans en disant « regardez, ça monte » n'a jamais été une stratégie.

Le walk-forward est la méthode la plus honnête pour valider une approche : on découpe l'historique, on optimise sur une fenêtre, on valide sur la suivante, on recommence. L'algorithme doit prouver qu'il généralise, pas qu'il récite. Et pourtant, même avec un walk-forward propre, +30 % annualisé en out-of-sample ne garantit rien. Petit rappel : SMH a fait +73 % en 2023 et QQQ +55 %, en buy and hold.

Plus encore, en live, chaque trade mange du spread, du slippage et de la latence. Plus on multiplie les entrées et sorties, plus ça se dégrade. Beaucoup de stratégies fonctionnent sur le papier, mais très peu survivent aux coûts réels. Et si l'edge ne tient pas en live, il n'y a pas de edge. Autant acheter un ETF et ne plus y toucher. Sérieusement. Mais bon, facile à dire après la fête.

Plus on teste de configurations, plus on overfitte. Bailey, Borwein et López de Prado l'ont mesuré : la probabilité d'overfitting augmente avec chaque paramètre ajusté, et elle est systématiquement sous-estimée.

De leur côté, les marchés ne sont pas stationnaires ; ce qui marche sur une période ne marche pas sur la suivante. Ça pousse à explorer d'autres approches, et la simulation de Monte Carlo devient le chemin naturel : tester sur des milliers de scénarios, pas sur un seul historique.

Finalement, l'objectif n'est pas tant de reproduire un backtest parfait en live, mais de construire quelque chose d'assez solide pour investir avec confiance dans le temps.

La prochaine fois que vous voyez un backtest avec une jolie courbe, posez une seule question : quelles sont les performances out-of-sample ?

S'il n'y a pas de réponse, il n'y a pas de stratégie.

---

**Source**
- Bailey, Borwein, López de Prado, Zhu — *The Probability of Backtest Overfitting*, Journal of Computational Finance, 2017.
  https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2326253
