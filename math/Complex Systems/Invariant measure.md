**Мера** $\mu: A \rightarrow R$ — функция, обладающая следующими свойствами

$$
    \mu(a) \geq 0, \forall a \in A
$$

$$
    \mu(\varnothing) = 0
$$

$$
    A \cap B = \varnothing \implies \mu(A \cap B) = \mu(A) + \mu(B)
$$

Если $\mu(A) = 1$, то $\mu$ называется **вероятностной мерой**

Меры бывают трех видов
- абсолютно непрерывные $\implies \exists$ мера области
- дискретные, описывающиеся счетным числом точек
- сингулярные, не являются ни дискретными, ни непрерывными, ни их комбинациями

Именно сингулярные меры у хаотических систем

Например на совершенном канторовом множестве (фрактальный отрезок из середин) можно дать каждому отрезку меру $\frac{1}{3^k}$, где $k$ номер шага, тогда общая размерность $log_3 2$, а мера не является ни дискретной, ни непрерывной

С каждым инвариантным множеством динамической системы связана своя сингулярная мера вне зависимости от того, какое множество.  Если у системы бесконечное число множеств, то мы получаем кроме мер этих множеств получаем бесконечное число паразитных мер в виде их комбинаций

**Эргодическая мера** — инвариантная мера не представимая в виде комбинации нескольких различных инвариантных мер

**Теорема Крылова-Боголюбова** — для замкнутого, компактного, инвариантного множества $A$, тогда у системы существует эргодическая мера связанная с $A$. Более того, можно выбрать данную меру вероятностной.

**Эргодическая теорема**  
Пусть $\mu$ — инвариантная мера, $g: A \rightarrow R$, непрерывная измеримая функция, тогда

$$
\forall t \lim_{T \rightarrow \infty} \frac{1}{T} \int_{t}^{t + T} g(s)ds = \int_{A} g(x)\mu(dx) 
$$

Что дает нам упрощенное вычисление, если мы не знаем меру $\mu$

**Теорема Пуанкаре о возвращении**  
Для $\forall$ динамической системы и $\forall$ инвариантного множества $A$ системы верно

$$
    \exists t > 0: \mu(A \cap \phi^t(A)) > 0
$$

$$
    \forall x \in A, \varepsilon > 0\ \exists t: |\phi^t(x) - x| < eps
$$

Этой теоремой мы пользовались в [методе Розенштейна](Rosenstein%20algorithm.md)

**Оператор Перрона-Фробениуса**  
Пусть есть отображение $x_{n + 1} = f(x_n)$ и вероятностная мера $p_n(x)$. Тогда

$$
    p_{n + 1}(y) = \int \delta(f(x) - y)p_n(x)dx = L(p_n)
$$

Здесь $\delta$ — функция, которая везде 0, в одной точке не определена и $\int_{R^k} \delta(x) = 1$  
Этот оператор описывает изменение плотности вероятности в фазовом пространстве состояний динамической системы

**Уравнение Перрона-Фробениуса (уравнение инвариантной меры)**

#Enhance