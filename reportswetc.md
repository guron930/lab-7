---
# Front matter
lang: ru-RU
title: "Отчет по лабораторной работе №7: Эффективность рекламы"
subtitle: "*дисциплина: Математическое моделирование*"
author: "Швец С, НФИбд-03-18"


# Formatting
toc-title: "Содержание"
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
fontsize: 12pt
linestretch: 1.5
papersize: a4paper
documentclass: scrreprt
polyglossia-lang: russian
polyglossia-otherlangs: english
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase
indent: true
pdf-engine: lualatex
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Введение

## Цель работы

 Построить математическую модели для выбора правильной стратегии при решении задачи об эффективности рекламы.

## Задачи

Можно выделить три основные задачи данной лабораторной работы:
1. Изучить теоретическую часть модели эффективности рекламы.
2. Реализовать частные случаи модели.

# Терминология. Условные обозначения

## Описание модели эффективности рекламы

Предположим, что торговыми учреждениями реализуется некоторая продукция, о которой в момент времени $t$ из числа потенциальных покупателей $N$ знает лишь $n$ покупателей. Для ускорения сбыта продукции запускается реклама по радио, телевидению и других средств массовой информации. После запуска рекламной кампании информация о продукции начнет распространяться среди потенциальных покупателей путем общения друг с другом. Таким образом, после запуска рекламных объявлений скорость изменения числа знающих о продукции людей пропорциональна как числу знающих о товаре покупателей, так и числу покупателей о нем не знающих.  
Модель рекламной кампании описывается следующими величинами. Считаем, что $\frac{dn}{dt}$ - скорость изменения со временем числа потребителей, узнавших о товаре и готовых его купить, $t$ - время, прошедшее с начала рекламной кампании, $n(t)$ - число уже информированных клиентов. Эта величина пропорциональна числу покупателей, еще не знающих о нем, это описывается следующим образом: $a_1(t)(N - n(t))$, где $N$ - общее число потенциальных платежеспособных покупателей, $a_1(t) > 0$ - характеризует интенсивность рекламной кампании (зависит от затрат на рекламу в данный момент времени). Помимо этого, узнавшие о товаре потребители также распространяют полученную информацию среди потенциальных покупателей, не знающих о нем (в этом случае работает т.н. сарафанное радио). Этот вклад в рекламу описывается величиной $a_2(t)n(t)(N - n(t))$, эта величина увеличивается с увеличением потребителей узнавших о товаре. Математическая модель распространения рекламы описывается уравнением:  
$$
\frac{dn}{dt} = (a_1(t) + a_2(t) n(t))(N - n(t))
$$  
При $a_1(t) >> a_2(t)$ получается модель типа модели Мальтуса, решение которой имеет вид (рис. -@fig:001):

![График решения уравнения модели Мальтуса](1.png){ #fig:001 width=70% }  

В обратном случае, при $a_1(t) << a_2(t)$ получаем уравнение логистической кривой (рис. -@fig:002):

![График логистической кривой](2.png){ #fig:002 width=70% }

# Выполнение лабораторной работы

## Формулировка задачи:

**Вариант 7**

Постройте график распространения рекламы, математическая модель которой описывается следующим уравнением:  
1. $\frac{dn}{dt} = (0.81 + 0.0003 n(t))(N - n(t))$  
2. $\frac{dn}{dt} = (0.00008 + 0.8n(t))(N - n(t))$  
3. $\frac{dn}{dt} = (0.8sin(8t) + 0.8cos(t) n(t))(N - n(t))$  

При этом объем аудитории $N = 888$, в начальный момент о товаре знает 18 человек. Для случая 2 определите в какой момент времени скорость распространения рекламы будет иметь максимальное значение.

## Решение

*Код на Julia:*

```
using Plots
using DifferentialEquations
theme(:wong)


N = 888;
x0 = 18;


g(t) = 0.81;
v(t) = 0.0003;
fun(x,p,t) = (g(t)+v(t)*x)*(N-x)
tspan = (0,10);
pr = ODEProblem(fun, x0, tspan);
sol = solve(pr, timeseries_steps = 0.1);

pl1 = plot(sol,
label = false)


savefig(pl1,"11.png")

g(t) = 0.00008
v(t)=0.8
fun2(x,p,t) = (g(t)+v(t)*x)*(N-x)

tspan = (0,0.1);
pr2 = ODEProblem(fun2, x0, tspan);
sol2 = solve(pr2, timeseries_steps = 0.1);
pl2 = plot(sol2,
label = false)


savefig(pl2,"22.png")


    n = length(sol2.u)
    J = length(sol2.u[1])
    U = zeros(n, J)

    for i in 1:n, j in 1:J
        U[i,j] = sol2.u[i][j]
    end

a = 0;
b = -1;

for i in 1:(n-2)
    if U[i+1] - U[i] > a
        a = U[i+1] - U[i];
        b = i;
    end
end


sol2.t[b]


sol2.u[b]

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
## Построенные графики

Первый случай (рис. -@fig:003):

![График распространения информации о товаре с учетом платной рекламы и  сарафанного радио.  $\alpha 1 = 0.81$,  $\alpha 2 = 0.0003$](11.png){ #fig:003 width=70% }

Второй случай (рис. -@fig:004):

![График распространения информации о товаре с учетом платной рекламы и  сарафанного радио. $\alpha 1 = 0.00008$, $\alpha 2 = 0.8$](22.png){ #fig:004 width=70% }


Точка максимального распостранения рекламы достигается при $t = 0.0075$,$u = 421.881$



Третий случай (рис. -@fig:005):

![График распространения информации о товаре с учетом платной рекламы и сарафанного радио, точка максимальной скорости распространения.  $\alpha 1 = 0.8sin(t)$, $\alpha 2 = 0.8cos(t))$](33.png){ #fig:005 width=70% }

# Вывод

Мы усвоили основные приципы модели эффективности рекламы, а также провели реализацию данной модели.
