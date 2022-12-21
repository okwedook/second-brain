**Энтропия динамической системы (Колмогорова-Синая)**

Считаем $H$ нашего распределения
- Разобъем пространство на непересекающиеся части $A_{i_1}, \dots, A_{i_k}$ диаметром $\varepsilon$
- Рассмотрим

$$
A_{i_1, i_2, \dots, i_k} = A_{i_1} \cap f^{-1}(A_{i_2}) \cap \dots \cap f^{-(k - 1)}A_{i_k}
$$

- Введем следующую величину

$$
    H_k = -\sum_{(i_1, \dots, i_k)} \mu(A_{(i_1, \dots, i_k)})\ ln\ \mu(A_{(i_1, \dots, i_k)})
$$

Тогда **энтропия Колмогорова-Синая** равна

$$
    KS = \lim_{\varepsilon \rightarrow 0} \lim{k \rightarrow \infty} [H_{k + 1} - H_{k}] = \lim_{\varepsilon \rightarrow 0} \lim{k \rightarrow \infty} \frac{H_k}{k}
$$

Обладает свойствами аналогичными показателю Ляпунова

$$
    \begin{cases}
        KS = 0, \text{ в регулярных системах} \\
        KS > 0, \text{ в хаотических системах}
    \end{cases}
$$

Это ещё один отпечаток хаоса, причем $KS \approx \lambda_1$

**Обобщенные энтропии**

#Enhance 