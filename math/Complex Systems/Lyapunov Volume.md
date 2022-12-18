[Показатель Ляпунова](Lyapunov%20exponent.md)

Кроме линейных показателей Ляпунова есть **объемные показатели Ляпунова**  
Пусть $u^{(1)}, u^{(2)}, \dots u^{(m)}$ — линейно независимые решения линеаризованной системы $\dot u = A(t)u$ .  
Если $m = n$, то это фундаментальная система $\rightarrow$ Вронскиан

$$
Vol(u^{(1)}, u^{(2)}, \dots u^{(m)}) = \sqrt{det \left( 
\begin{matrix}
(u^{(1)}(t), u^{(1)}(t)) & \dots & (u^{(1)}(t), u^{(m)}(t)) \\
\vdots & \ddots & \vdots \\
(u^{(m)}(t), u^{(1)}(t)) & \dots & (u^{(m)}(t), u^{(m)}(t)) 
\end{matrix}
\right)}
$$

Тогда объемный показатель Ляпунова равен

$$
\mathscr{H}_m = \lim_{t\rightarrow \infty} \frac{1}{t} ln |Vol(u^{(1)}, u^{(2)}, \dots u^{(m)})|
$$

Несколько свойств

$$
\begin{cases}
\mathscr{H}_m = \lambda_1 + \lambda_2 + \dots + \lambda_m \\
\lambda_i = \mathscr{H}_i - \mathscr{H}_{i - 1} \\
\mathscr{H}_n > 0 \rightarrow \text{система консервативна} \\
\mathscr{H}_n < 0 \rightarrow \text{система диссипативна (хаотична)} \\
\end{cases}
$$
