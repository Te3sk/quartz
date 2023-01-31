# Teorema di Church-Rosset
```toc
```
##### 2022/09/29 #date 
## Teorema
$$
\begin{array}{ccccccc}
	t \rightarrow_\beta^* s_1 &
	\wedge &
	t \rightarrow_\beta^* s_2 &
	\implies &
	\exists\;u.\,s_1 \rightarrow_\beta^* u &
	\wedge &
	s_1 \rightarrow_\beta^* u
\end{array}
$$
### Corollario 1
Unicità di forme normali (modulo $=_\alpha$)
### Corollario 2
$t =_\alpha s \iff \exists u.t \rightarrow_\beta^*  u\: _\beta^*\! \leftarrow s$ 
**Dimostration:**
### Corollario 3
L'ordine delle $\beta$-riduzioni è irrilevante
## Effetti collaterali
Essendo la computazione una catena di $\beta$-riduzioni, qualsiasi cosa che esce o entra dalla catena è detto **effetto collaterale**. Si possono anche definire rispetto a quando ci sono, usando il concetto di **trasparenza referenziale**.
#### Trasparenza Referenziale
$$
\begin{array}{lr}
	c\,[e] &&
	e\rightarrow^*\;v \implies c\,[v]
\end{array}$$
Sostituire i valori uguali in espressioni uguali senza modificarne il risultato finale.