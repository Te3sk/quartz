# α-Conversione
```toc
```
##### 2022/09/22 #date
## Idea
Prendendo l'equazione $p(x)=x^2+3x-6$, fare $p(5)$ equivale a scrivere
$$(\lambda x.x^2+3x-6)5 \textcolor{#f77ead}{=}5^2+5\cdot 3-6 \textcolor{#f77ead}{=}25+15-6 \textcolor{#f77ead}{=}34$$
Quindi diciamo che $t[s/x]$ **sostituisce** tutte le occorrenze *libere* di $x$ in $t$ con $s$
> [!example] 
> $\rightarrow x[s/x]=s$     con $x\ne y$
> $\rightarrow y[s/x]=y$
> $\rightarrow (t,u) [s/x] = t[s/x]\:u[s/x]$
> $\rightarrow(\lambda x.t)[s/x]= \lambda x.t$
> $\rightarrow(\lambda y.t)[s/x]= \lambda y.t[s/x]$

Il **problema** è che $\lambda x.t \approx \lambda y.t[y/x]$ perché $(\lambda x.y)[y/x]= \lambda x.x$
- $(\lambda x.t)[s/x]= \lambda x.t$
- $(\lambda y.t)[s/x]= \lambda y.t[s/x]$    se $y \ne FV(s)$
- $(\lambda y.t)[s/x]=(\lambda z.t[z/y])[s/x]$   se $z\ne FV(t)$
-         $= z.t[z/y][s/x]$        $z \ne FV(s)$
> [!example] 
> - $(\lambda y.x)[x/y]=(\lambda z.y)[y/x]= \lambda z.y$
> - $\lambda y.x(\lambda w.vwx))[\lambda y.xy/x]$
>   $(\lambda y.x(\lambda x.x))[\lambda y.xy/x]$
>   $(\lambda x.xy)[uv/z]$

## Definizione
È la più piccola equivalenza tale che
$\approx _\alpha \subset (compreso/uguale) \Lambda \times \Lambda$.
$\Rightarrow \lambda x.t = \lambda y.t[y/4]$
$$
\begin{array}{ccccc}
\frac{ }{t=_\alpha t} &
\frac{t=_\alpha s}{s =_\alpha t} &
\frac{t=_\alpha s, s=_\alpha u}{t=_\alpha u} &
\frac{t=_\alpha t' s=_\alpha s'}{ts=_\alpha ts'}
\end{array}
$$
> [!example]
> $function succ(x)\{x+1\}=_\alpha function succ(y)\{y+1\}$
> $lim_{x\to\infty} e^{-x} =_\alpha lim_{y\to\infty} e^{-y}$
> 
> 
> $\lambda xy.x(xy)=_\alpha \lambda at.a(ab)$
> per dimostrarlo usiamo le regole di inferenza:
> $\lambda y.x(xy)=_\alpha \lambda b.x(xb)$
> siccome l'altra regola è una _congruenza_
> $\lambda x \lambda y.x(xy)=_\alpha \lambda x \lambda b.x(xb)=$
> $=_\alpha \lambda a \lambda b.a(ab)$
## Lemma
$$\frac{t=_\alpha t' \:\:\: s=_\alpha s}{t[s/x]=_\alpha \: t'[s/x]}$$

> [!warning] NB
> $F:\Lambda ^n \rightarrow \Lambda$
> questo, formalmente vuol dire che:
> $$t_1=_\alpha s_1 --- t_n =_\alpha s_n$$
> $$\Rightarrow F(t_1 ...t_n)=_\alpha(s_1 ...s_n)$$
> 