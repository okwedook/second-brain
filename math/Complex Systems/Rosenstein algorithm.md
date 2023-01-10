## Алгоритм Розенштейна

- Было доказано, что если траектории циклично похожи, то можем считать по теореме Ляпунова, что у нас есть возбужденная и невозбужденная функции

$$
    \varepsilon e^{\lambda l \Delta t} \cong \varepsilon(k \Delta t) \rightarrow \hat{\lambda} = \frac{1}{k \Delta t} ln \frac{\varepsilon(k \Delta t)}{\varepsilon(0)}
$$

1. Для каждого $z_i = (x_i, x_{i + 1}, \dots, x_{i + l - 1})$ находим множество его ближайших соседей $C = \{z_{i*} | \rho(z_i, z_{i*}) < \varepsilon \text{ и } |i - i*| > p\}$
2. Вместе с $z_i$ и его множеством ближайших соседей рассматриваем $z_{i + k}$ и $C$, благодаря чему находим оценку 

$$
\hat{\lambda_i} = \frac{1}{|C|} \sum_{z_{i*} \in C} \frac{1}{k \Delta t} ln \frac{\rho (z_{i*+k}, z_{i + k})}{\rho({z_{i*}, z_i})}
$$

3. Тогда хорошим приближением показателя будет

$$
    \hat{\lambda} = \frac{1}{|T|} \sum_{i = 1}^T \hat{\lambda_i}
$$

#time-series 