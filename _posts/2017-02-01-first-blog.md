---
layout: post
comments: true
title:  "Método de Newton-Raphson"
desc: "Entrada de prueba de mi blog"
keywords: "code,scilab,matlab"
date: 2013-11-01
categories: [Code]
tags: [blog]
icon: fa-code
---

Tal vez, dentro de los métodos para determinar el valor de las raíces de una función ya sea lineal o no lineal, el método de Newton es uno de los más utilizados. Si el valor inicial con el que se aproxima  la raíz es $x_i$, entonces se puede extender una tangente desde el punto $[x_i,f(x_i)]$ en donde $i=0,1,2,..,n$. De esta forma el punto de la tangente que corta al eje $x$ representa una aproximación mejorada de la raíz (ver la figura).


![Newton-Raphson](/static/assets/img/blog/code/newton-raphson.png)


El método de Newton-Raphson se puede deducir sobre la base de una interpretación geométrica (un método alterno basado en la serie de Taylor). Como podemos apreciar en la figura, la derivada de $x$ es equivalente a:

$$f'(x_i)=\frac{f(x_i)-0}{x_i-x_{i+1}}$$

Que se puede ordenar para obtener:

$$x_{i+1}=x_i-\frac{f(x_i)}{f'(x_i)}$$

Esta es la formula utilizada en el método de Newton-Raphson

**Estimación del error**

Para poder especificar un criterio de paro en el método de Newton-Raphson se suele utilizar la siguiente expresión para calcular el error en cada iteración:

$$error=\left | \frac{x_{i+1}-x_i}{x_{i+1}}\right | $$

Por otra parte si desarrollamos el modelo por medio de la serie de Taylor, esto nos proporciona una base teórica firme para poder estimar la velocidad de variación del error en cada iteración y así poder verificar la convergencia del método. Veamos:

La expansión de la serie de taylor se expresa de la siguiente manera:

$$f(x_{i+1})=f(x_i)+f'(x_i)(x_{i+1}-x_i)+\frac{f''(\xi)}{2!}(x_{i+1}-x_i)^2+... +\frac{f^n(\xi)}{n!}(x_{i+1}-x_i)^n\label{ref3}$$

En donde $\xi$ se encuentra en alguna parte en el intervalo entre $x_{i+1}$ y $x_i$. Si truncamos la serie de Taylor después del segundo termino derivado obtenemos la siguiente aproximación:

$$f(x_{i+1}) \cong f(x_i)+f'(x_i)(x_{i+1}-x_i)$$

En a intersección con el eje $x$ debemos tener $f(x_{i+1}))=0$, por lo tanto:

$$0 = f(x_i)+f'(x_i)(x_{i+1}-x_i)$$

Por lo que obtenemos la formula utilizada en el Método de Newton-Raphson

\begin{equation}
x_{i+1}=x_i-\frac{f(x_i)}{f'(x_i)} \label{ref1}
\end{equation}

Ahora si analizamos el error. Para esto hacemos $x_{i+1}=x_r$ en la ecuación \ref{ref3}, en donde $x_r$ representa el valor real de la raiz y a la vez $f(x_r)=0$ :

$$0=f(x_i)+f'(x_i)(x_r-x_i)+\frac{f''(\xi)}{2!}(x_r-x_i)^2 \label{ref4}$$

y truncando  la ecuación \ref{ref3} hasta el tercer termino:

$$f(x_{i+1}) \cong f(x_i)+f'(x_i)(x_{i+1}-x_i)+\frac{f''(\xi)}{2!}(x_{i+1}-x_i)^2 \label{ref2}$$

Luego restamos \eqref{ref4} de \ref{ref2}:


$$0=f'(x_i)(x_r-x_{i+1})+\frac{f''(\xi)}{2!}(x_r-x_i)^2 \label{ref5}$$

Ahora sabiendo que el error entre la aproximación del método y el valor real es igual a la direfencia entre $x_{i+1}$ y $x_r$:

$$E_{t,i+1}=x_r-x_{i+1}$$

entonces:

$$0=f'(x_i)E_{t,i+1}+\frac{f''(\xi)}{2!}E_{t,i}^2 \label{ref6}$$

Si se supone que el método efectivamente converge a una solución, se podría decir entonces que tanto $x_i, \xi \longrightarrow x_r$, de esta manera la ecuación \ref{ref6} quedaría así:

$$E_{t,i+1}=\frac{-f''(x_r)}{2f'(x_r)}E_{t.i}^2 \label{ref7}$$



Por lo que podemos ver en la ecuación \eqref{ref7} el error es proporcional al cuadrado del error anterior, este es uno de los motivos por los cuales en el caso de que la solución converja lo hace de una manera bastante rápida, sólo después de un par de iteraciones. A este comportamiento se le conoce como convergencia cuadrática.

A continuación les dejo un código que hice en MATLAB con la implementación del método.


```
%%%/////////////////////////////////////////////////
% /          Función Newton-Raphson            ///
%/        Desarrollada por Jorge De La Cruz     ///
%/                                             ///
%////////////////////////////////////////////////
% La función 'fun' debe ser introducida como un string
% x: representa el valor inicial necesario para iniciar
% las iteraciones.
% tol: La tolerancia deseada.
% imax: máximo número de iteraciones, evita que el
% programa se quede en un bucle infinito en caso de que
% el método no converja.
function [xr]=newton(fun,x,tol,imax)
err=100;
n=1;
while (tol&lt;=err)& & (n&lt;imax)
    xr = x-eval(fun)/(eval(diff(sym(fun))));
    err=abs((xr-x)/xr);
    error=err*100;
    x=xr;
    fprintf('Iteración: %d, Error: %f, Raiz: %f\n',n,error,xr)
    n=n+1;
end
x=-2:0.001:xr+1;
y=eval(fun);
plot(x,y)
title('METODO DE NEWTON-RAPHSON')
xlabel('$x$','interpreter','latex','fontsize',18)
ylabel('$y$','interpreter','latex','fontsize',18)
grid on
end
```

---

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Licencia Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">Método Newton-Raphson</span> por <a xmlns:cc="http://creativecommons.org/ns#" href="https://jdelacruz26.github.io" property="cc:attributionName" rel="cc:attributionURL">Jorge De La Cruz</a> se distribuye bajo una <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Licencia Creative Commons Atribución-CompartirIgual 4.0 Internacional</a>.
