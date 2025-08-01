### 왼쪽 정렬
```LaTeX
\begin{flalign*}
// 수식 뒤에 &&
\end{flalign*}
```

$$
\left[
    \begin{matrix}
        0 & \hfil     1 & 5 & 2 \\
        1 & \ \llap{-}7 & 1 & 0 \\
        0 & \hfil     0 & 1 & 3
    \end{matrix}
\right]
$$

https://oeis.org/wiki/List_of_LaTeX_mathematical_symbols

$$
\frac {\partial{L}}{\partial{w}} =
\frac {\partial{L}}{\partial{a}} \cdot
\frac {\partial{a}}{\partial{z}} \cdot
\frac {\partial{z}}{\partial{w}}
\; \text{(chain rule)}
,\quad \;
a = \sigma(z), \quad
z = wx + b
$$

$$

$$

$$
\because \,
L = \frac{1}{n}\sum^n_1 (y - a)^2 ,\quad
\frac {\partial{L}} {\partial{a}} =
\frac{1}{n}\frac {\partial (y - a)^2 }{\partial{a}} =
\frac{1}{n} 2(a-y) \quad \therefore
\frac {2(a-y)}{n}
$$

$$
\because
a = \sigma(z) = \frac {1}{1+e^{-z}}, \;\;
\sigma^{\prime}(z) = \sigma(z)(1-\sigma(z)) \quad
\therefore
\frac {\partial a}{\partial z} = a\,(1-a)
$$

$$
\delta_{out} = \frac {\partial L}{\partial z}
= \frac {\partial L}{\partial a} \frac {\partial a}{\partial z}
$$

$$
\because z = a^{(h)}w + b \quad
\therefore
\frac {\partial z} {\partial w} = a^{(h)}
$$

$$
\frac {\partial L}{\partial w} =
\frac {\partial{L}}{\partial{a}} \cdot
\frac {\partial{a}}{\partial{z}} \cdot
\frac {\partial{z}}{\partial{w}} =
{\delta_{out}}^T
\frac {\partial{z}}{\partial{w}} =
{\delta_{out}}^T a^{(h)}
$$
$$
\frac {\partial L}{\partial b} =
\frac {\partial{L}}{\partial{a}} \cdot
\frac {\partial{a}}{\partial{z}} \cdot
\frac {\partial{z}}{\partial{b}} =
\delta_{out} \frac {\partial{z}}{\partial{b}} =
\delta_{out} \quad
\because
\frac {\partial{z}}{\partial{b}} =
\frac {\partial{(aw + b)}}{\partial b} = 1
$$

$$
\because z^{(out)} = wa^{(h)} + b ,\quad
\frac {\partial z} {\partial a^{(h)}} = w
$$

$$
\frac {\partial L} {\partial a^{(h)}} =
\frac {\partial L} {\partial z^{(out)}} \frac {\partial z^{(out)}} {\partial{a^{(h)}}} =
\delta_{out} \frac {\partial z^{(out)}} {\partial a^{(h)}} =
\delta_{out} w
$$

$$
\frac {\partial a^{(h)}} {\partial z^{(h)}} = a^{(h)}(1-a^{(h)})
$$

$$
\frac {\partial L} {\partial w^{(h)}} =
\frac {\partial L} {\partial a^{(h)}} \cdot
\frac {\partial a^{(h)}} {\partial z^{(h)}} \cdot
\frac {\partial z^{(h)}} {\partial w^{(h)}}
$$