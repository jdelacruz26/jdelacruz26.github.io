---
layout: post
comments: true
title:  "Mecánica de la Fractura y la Fatiga - Integral J"
desc: "Descripción del método de la integral J"
keywords: "Fracture"
date: 2017-11-25
categories: [Engineering]
tags: [Fracture Mechanics, Engineering]
icon: fa-pencil-square-o
---

La **integral J** es una integral de linea alrededor del frente de grieta (ver figura [1](#fig1)) y es independiente del camino de integración que se tome entorno a la grieta. La misma es de gran importancia en la Mecánica de la fractura elasto-plástica (válida también para MFEL). La integral de línea debe cerrar un circuito comprendido entre los labios superior e inferior de la grieta.

 La integral $J$ se define de la siguiente manera:
 <br>

 <a id="eq1">eq.1</a>
 $$
\begin{eqnarray}
J &=& \int W dy - \int \overline{T} \frac{\partial \overline{u}}{\partial x} ds
\end{eqnarray}
$$

<br>
En donde:
<br>
$$
\begin{eqnarray}
W &=&\frac{1}{2}(\sigma_{xx}\epsilon_{xx}+ \sigma_{yy}\epsilon_{yy}+\sigma _{xy} \gamma _{xy})\\
\epsilon_{xx} &=& \frac{1}{E}\left [ \left (1-\nu ^{2}\right )\sigma_{xx}-\nu (1+\nu)\sigma_{yy}\right ] \\
\epsilon_{yy} &=& \frac{1}{E}\left [ \left (1-\nu ^{2}\right )\sigma_{yy}-\nu (1+\nu)\sigma_{xx}\right ]\\
\gamma_{xy} &=& \frac{\sigma_{xy}}{G}
\end{eqnarray}
$$
<br>

 Mientras que:
 <br>
$$
\begin{eqnarray}
\int \overline{T} \frac{\partial \overline{u}}{\partial x} ds &=& \int_{\Gamma}\left ( \sigma_{xx} \frac{\partial u}{\partial x}+ \sigma_{xy} \frac{\partial v}{\partial x}\right )dy - \int_{\Gamma} \left ( \sigma_{xy} \frac{\partial u}{\partial x}+ \sigma_{yy} \frac{\partial v}{\partial x}\right )dx
\end{eqnarray}
$$
<br>
[<center><img src="/static/assets/img/blog/engineering/figj1.png" width="300px"/></center>](/static/assets/img/blog/engineering/figj1.pdf)

<center>
Fig. 1: Grieta de longitud $2a$ y camino de integración $\overline{ABCDEF}$. <a id="fig1"></a>
</center>
<br>
La integral $J$ es aplicada al caso de estudio mostrado en la figura  [1](#fig1). La tensión aplicada es de $\sigma=100MPa$, la placa consiste de un acero con módulo de elasticidad $E=210GPa$ y $\nu =0.3$. La longitud de la grieta es $2a=2.4cm$

De esta forma la integral $J$ está compuesta de la suma de la misma en cada uno de los distintos segmentos, esto es:
<br>
$$
\begin{eqnarray}
J &=&J_{AB}+J_{BC}+J_{CD}+J_{DE}+J_{EF}
\end{eqnarray}
$$
<br>

El cálculo de la integral $J$ a partir de la función $\phi=\sigma/\sqrt{1-\left (\frac{a}{z}\right )^{2}}$ en donde $z=x+iy$, se realizó utilizando un software de cálculo simbólico llamado **Maxima**. Los resultados para cada uno de los términos de la integral $J$ presentados en la ecuación [eq.1](#eq1) se muestran en las figuras [2](#fig2) y [3](#fig3). A partir de estas se evalúa la integral correspondiente a cada uno de los segmentos dando como resultado:
<br>

<a id="eq2">eq.2</a>
$$
\begin{eqnarray}
J_{AB} &=&-336.739 \nonumber \\
J_{BC} &=&-288.634 \nonumber \\
J_{CD} &=& -382.876 \nonumber \\
J_{DE} &=& -288.634 \nonumber \\
J_{EF} &=& -336.739 \nonumber \\
J &=& 1633.622Pa\sqrt{m} \label{result}
\end{eqnarray}
$$
<br>
Como sabemos que $J=\frac{K_{I}^{2}(1-\nu ^{2})}{E}$ (para MFEL), entonces:
<br>
$$
\begin{eqnarray}
J &=& \frac{(\sigma \sqrt{\pi a})^{2}(1-\nu ^{2})}{E} \nonumber \\
J&=&\frac{(100MPa\sqrt{\pi 0.012})^{2}(1-0.3 ^{2})}{210GPa} \nonumber \\
J&=&1633.628Pa\sqrt{m} \label{result2}
\end{eqnarray}
$$
<br>

Como se puede observar el resultado en [eq.2](#eq2)<sub id="an1">[1](#fn1)</sub> y \eqref{result2} coinciden, de esta forma se verifica el valor calculado a partir de la formulación presentada en [eq.1](#eq1).

[<center><img src="/static/assets/img/blog/engineering/figj2.png" width="300px"/></center>](/static/assets/img/blog/engineering/figj2.pdf)

<center>
Fig. 2: Valores para los segmentos: $a)A-B$, $b)E-F$, $c)B-C$ y $d)D-E$. <a id="fig2"></a>
</center>

<br>

[<center><img src="/static/assets/img/blog/engineering/figj3.png" width="300px"/></center>](/static/assets/img/blog/engineering/figj3.pdf)

<center>
Fig. 3: Grieta de longitud $2a$ y camino de integración $\overline{ABCDEF}$. <a id="fig3"></a>
</center>
<br>

<i class="fa fa-github"></i> [Código para la solución de las ecuaciones](https://github.com/jdelacruz26/misccode/blob/blog-code/integralJ.lsp)

<br>

---
<br>

<a id="fn1">1</a>: Se invierte el signo de la suma ya que se asumieron valores positivos en el sentido de las manecillas del reloj.[<i class="fa fa-reply"></i>](#an1)
<br>

---
<center>
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Licencia Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">Mecánica de la Fractura y la Fatiga - Integral J</span> por <a xmlns:cc="http://creativecommons.org/ns#" href="https://jdelacruz26.github.io" property="cc:attributionName" rel="cc:attributionURL">Jorge De La Cruz</a> se distribuye bajo una <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Licencia Creative Commons Atribución-CompartirIgual 4.0 Internacional</a>.
</center>
