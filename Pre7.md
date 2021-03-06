---
## Front matter
lang: ru-RU
title: 'Модель "Эффективность рекламы"'
author: Швец С
date: 2021, 25 Marсh

## Formatting
toc: false
slide_level: 2
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
aspectratio: 43
section-titles: true
---

# Цель работы


 Построить математическую модели для выбора правильной стратегии при решении задачи об эффективности рекламы.


# Задачи

1. Изучить теоретическую часть модели эффективности рекламы.
2. Реализовать частные случаи модели

# Выполнение лабораторной работы

## Формулировка задачи

**Вариант 7**

Постройте график распространения рекламы, математическая модель которой описывается следующим уравнением:  
1. $\frac{dn}{dt} = (0.81 + 0.0003 n(t))(N - n(t))$  
2. $\frac{dn}{dt} = (0.00008 + 0.8n(t))(N - n(t))$  
3. $\frac{dn}{dt} = (0.8sin(8t) + 0.8cos(t) n(t))(N - n(t))$  

При этом объем аудитории $N = 888$, в начальный момент о товаре знает 18 человек. Для случая 2 определите в какой момент времени скорость распространения рекламы будет иметь максимальное значение.


## Решение: Коэффиценты

Максимальное количество людей, которых может заинтересовать товар:
N = 888;

Количество людей, знающих о товаре в начальный момент времени:
x0 = 18;

Функция, отвечающая за платную рекламу
g(t) = 0.81;

Функция, описывающая сарафанное радио:
v(t) = 0.0003;


## Решение: случай №1


$\alpha1 = 0.81$, $\alpha2 = 0.0003$

```
fun(x,p,t) = (g(t)+v(t)*x)*(N-x)
tspan = (0,10);
pr = ODEProblem(fun, x0, tspan);
sol = solve(pr, timeseries_steps = 0.1);
pl1 = plot(sol,
label = false)
savefig(pl1,"11.png")

```
## Решение: случай №2


$\alpha1 = 0.00008$, $\alpha2 = 0.8$

```
g(t) = 0.00008
v(t)=0.8
fun2(x,p,t) = (g(t)+v(t)*x)*(N-x)

tspan = (0,0.1);
pr2 = ODEProblem(fun2, x0, tspan);
sol2 = solve(pr2, timeseries_steps = 0.1);
pl2 = plot(sol2,
label = false)
savefig(pl2,"22.png")
```

## Решение: случай №2

Вычисление точки максимального распостранения рекламы:


Точка максимального распостранения рекламы достигается при $t = 0.0075$,$u = 421.881$

## Решение: случай №3

$\alpha1 = 0.00008$, $\alpha2 = 0.8$

```
g(t) = 0.8*sin(8t)
v(t)=0.8*cos(t)
fun3(x,p,t) = (g(t)+v(t)*x)*(N-x)
tspan = (0,2);
pr3 = ODEProblem(fun3, x0, tspan);
sol3 = solve(pr3, timeseries_steps = 0.1);
pl3 = plot(sol3,
label = false)
savefig(pl3,"33.png")
```


## Решение: график для случая №1

График распространения информации о товаре с учетом платной рекламы и  сарафанного радио.  $\alpha 1 = 0.81$,  $\alpha 2 = 0.0003$(рис. -@fig:001)

![Случай №1](11.png){ #fig:001 width=70% }


## Решение: график для случая №2

График распространения информации о товаре с учетом платной рекламы и  сарафанного радио. $\alpha 1 = 0.00008$, $\alpha 2 = 0.8$ (рис. -@fig:002)


![Случай №2](22.png){ #fig:002 width=70% }


## Решение: график для случая №3

График распространения информации о товаре с учетом платной рекламы и сарафанного радио, точка максимальной скорости распространения. $\alpha 1 = 0.8sin(t)$, $\alpha 2 = 0.8cos(t))$ (рис. -@fig:003)

![Случай №3](33.png){ #fig:003 width=70% }

# Выводы

Мы усвоили основные приципы модели эффективности рекламы, а также провели реализацию данной модели.
