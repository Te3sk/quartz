# β-riduzione
```toc
```
##### 2022/09/22 #date 
## Idea
Trovaimo il modo di calcolare $(\lambda x.t)s \rightarrow _\beta t[s/x]$. 
$$
\Large{\begin{array}{cccc}
\frac{\rightarrow _\beta \subseteq \Lambda \times \Lambda}{(\lambda x.t)_s \rightarrow _\beta t[s/x]} &
\frac{t \rightarrow _\beta s}{tu \rightarrow _\beta su} &
\frac{t \rightarrow _\beta s}{ut \rightarrow _\beta us} &
\frac{t \rightarrow _\beta s}{\lambda x.t \rightarrow _\beta \lambda x.s}
\end{array}}
$$
> [!example] 
> $(\lambda x.x(xy))t \rightarrow _\beta t(uy)$
> $(\lambda x(\lambda y.yx)z)v \rightarrow _\beta$ posso ridurlo in due modi:
> 		$\rightarrow _\beta (\lambda y.yv)z \rightarrow _\beta$
> 		$\rightarrow _\beta (\lambda x.zx)v \rightarrow _\beta$
> ed entrambi si riducono a $\rightarrow _\beta zv$ perché è **non deterministica**
### Forma Normale
> [!def] Forma Normale
> Un termine $t$ si una _forma normale_ se $t \not\rightarrow _\beta$ ($t$ non si riduce a nulla)
> $t$ ha una _forma normale_ se $\exists s \in NF:\rightarrow _\beta ^* s$

> [!exc] 
> $t,s=x|\lambda x.t|ts =_\alpha (\lambda x.t)_s \rightarrow _\beta c$
> * $succ = \lambda u. \lambda f. \lambda x.f(nfx)$  $\beta$-riduce $c_0, c_1, c_2
> $$\forall n. succ c_n \rightarrow _\beta ^* c_(qualcosa)$$
> * $add=\lambda n\lambda m.\lambda f.\lambda x.uf(mfx)$ calcolare $add c_2, c_3$
> * $|y|=0.5cm$ possiamo scrivere $t\in \Lambda , |t| \in 20cm$, $t$ ha forma normale, $|s|=10^10^10$ anni luce, $t=(\lambda x.xxxxx)c_{(10)}$

##### (2022/09/27) #date
## Definizione
Guardiamo il $\lambda$-calcolo come *un assembly* dei linguaggi funzionali.
- **grammatica:** $t,s::=x\,|\,\lambda x.t\,|\,ts$
- **$\beta$-riduzione con sostituizione:** $(\lambda x.t)s \rightarrow_\beta  t[s/x]$
- **alfa equivalenza:** $t=_\alpha s$
$$
\begin{array}{lcr}
\lambda x(\lambda y.y)x= \lambda z.z &
\text{dove } \lambda x(\lambda y.y)x \text{ e } \lambda z.z \text{ sono \textcolor{#f77ead}{l'identità}, e} &
\lambda x(\lambda y.y)x \rightarrow_\beta \lambda x.x =_\alpha \lambda z.z
\end{array}
$$
- **catena di riduzioni** per cui $t$ si converte in $s$ cioè sono **$\beta$-convertibile** $t=_\beta$ **iff** $t(\beta_\leftarrow u \rightarrow_\beta)^*s$
> [!def] $\beta$-riduzione
> $t$ ed $s$ si dicono $\beta-riducibili$ se posso fare *avanti e indietro con la computazione*
> $t=_{\beta} s$ **iff** $t({\beta}\leftarrow\cup\rightarrow_{\beta})s$
> quindi se $t\rightarrow_\beta t' \:_\beta\!\leftarrow t''\: _\beta\!\leftarrow s' \rightarrow_\beta s$
> 

Deve esserci quindi una catena di riduzione per cui $t$ si converte ad $s$, quindi sono $\beta$-convertibili
> [!example] 
> $ts \Rightarrow (\lambda x(\lambda x.x)x)=_\alpha(\lambda y(\lambda x.x)x =_\alpha (\lambda y.(\lambda z.z)y)x \rightarrow_\beta^* v$
> dato che $(\lambda x(\lambda x.x)x)\rightarrow_\beta w \:\: \Rightarrow w=_\alpha v$

> [!def] proposizione
> $t \rightarrow_\beta s \land t=_\alpha t' \implies\: \exists s'\,t'\rightarrow_\beta s' \land s=_\alpha s'$
### Proprietà della $\beta$-riduzione
1. $(\lambda x.(\lambda x.x)x)x =_\alpha (\lambda y.(\lambda x.x)y)x =_\alpha (\lambda y.(\lambda z.z)y)x \rightarrow_\beta^* v$, dato che $(\lambda x.(\lambda x.x)x)x \rightarrow_\beta^* w \implies w =_\alpha v$
2. La $\beta$-riduzione è **non deterministica** $\implies ((\lambda x.x)\,I\,)\;\Omega$
3. La $\beta$-riduzione può **non terminare**$$k= \lambda x.\lambda y.x$$
	Noi vogliamo avere programmi sequenziali e deterministici, quindi il punto 1. ci fa sorgere alcune domande
	* **Se riduco in modi diversi, arrivo a due risultati diversi?**
		* $KI\Omega =(\lambda xy.x)I\Omega=$ da qui posso avere due risultati:
			-  $\rightarrow_\beta I$
			- $\rightarrow_\beta KI\Omega$ e da qui posso ridurre a $I$, e arrivare al risultato sopra, o loopare su $\Omega$
			- Da entrambe però posso fare scelte intermedie e raggiungere $I$	$$t=_\beta s\:\:iff\:\:t \rightarrow_\beta^*\:\: _\beta^*\!\leftarrow s$$****