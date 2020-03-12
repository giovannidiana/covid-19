---
layout: default
mathjax: true
---

{% include mathjax.html %}

### Authors
 Giovanni Diana and Diana Passaro

***

* [Summary of the outbreak by country](current_state))
* [Supplementary Methods](SM)


***

## Introduction 
Spread of COVID-19 in early 2020 has raised important concerns about the ability of national health systems to detect the positive cases, as well as the intervention rate a country is expected to put in place to contain the infection.

  Using data on disease spread and containment through the public repository [CSSEGISandData/COVID-19](https://github.com/CSSEGISandData/COVID-19) we established a predictive model to estimate the outbreak of the infection in a given population. 

In some regions of China the infection rate has significantly decreased compared to the initial exponential spread of the infection. This information can be used to build predictive models which can help other countries to estimate the extent of the outbreak.  
First we modeled the number of infections over time based on the disease outbreak in China. Our model captures the initial exponential phase of the outbreak and the effect of the external intervention to contain the infection. Thus the exponential phase termimnates with a peak of maximum rate of new infected cases and then is followed by a critical period where the number of infections can decrease if the intervention remains stable.  

<figure>
   <center><img src="Figures/Figure_stat_1.png"/>
   <figcaption> Fig. 1: Dynamics of the infection in Italy (last update 12/02/2020) </figcaption></center>
</figure>

## The model
We used a generative model to describe the dynamics of the infected population in a given geographic area. Our model takes into account the effect of the local interventions by coupling the average number of infections $$X(t)$$ with a dynamical variable $$A(t)$$ which acts against the spread. The observed number of cases is then generated from a Poisson distribution with rate $X(t)\cdot p(t)$, where $p(t)$ is a fraction of the true number of infections which varies over time (due for instance to the increased number of tests during the acute phase).

The model is defined by two differential equations for the size of the infected population $X(t)$ and the action $A(t)$ and a probabilistic rule on the observed infections 

$$
\begin{align*}
\frac{dX}{dt} &= (\lambda-A(t))\cdot X(t),\quad X(t_0) = X_0\\
\frac{dA}{dt} &= h X(t)p(t),\quad A(t_0)=0\\
n(t) & \sim \mathrm{Poisson}(X(t) \cdot p(t)),\quad p(t)=\frac{1}{1+\left(\frac{k}{X(t)}\right)^g}
\end{align*}
$$ 

where $\lambda$ is the (observed) infection rate and $h$ quantifies the effect of local interventions.  
$X_0$ is the average number of infections at the initial time (22/01/2020).
The lack of observations at early stages is captured by the factor $p(t)$ which depends directly on the daily infection rate. The assumption being that the more infections are detected the more tests and controls are put in place to monitor the infected population. Note however that this is not sufficient to estimate the true number of infected individual, but to accommodate the low number of observations when the epidemics reaches a given country.



Figure 2 illustrates the typical dynamics of the model.

<figure>
<img src="Figures/Figure_1.png"/>
<figcaption> Fig. 2: Dynamics of the number of infections</figcaption>
</figure>

|name |	description|
|:----|-----------:|
|$h$ |	intervention coefficient|
|$k$ |	Hill scale|
|$g$ |	Hill shape|
|$\lambda$ |	global infection rate|
|$X_0$ |	initial infected|
|$X(t)$ |	infected at time t|
|$A(t)$ | intervention strength|
|$p(t)$ |	observed fraction of infected individuals|

Table 1: Summary of parameters and variable used in the model.



Of particular importance, the intervention coefficient h and strenght A(t) describe the investment the country is putting in place to contain the epidemy (plotted in supplementary Figure 3 and 4). Linked to these parameter, the time to intervention can be calculated for each country (supplementary figure 5), and together with the hill scale k (supplementary figure 6) is indicative of the readiness of the country to put in place a containment programme.  


## Statistical inference and model predictions
By using the available daily reported cases in the public repository [CSSEGISandData/COVID-19](https://github.com/CSSEGISandData/COVID-19) we can estimate the parameters of the model from the data for each country/region affected by the infection (see current state [here](current_state)). Knowing the model parameters allow us to draw predictions on how the epidemics will evolve. For this analysis we assume that the rate of infection $\lambda$ is the same for all countries whereas all the other model parameters are country-dependent. This allows us to exploit the worldwide data to strengthen the predictive power of the model.

The framework of statistical inference allows us to estimate the model parameters and make predictions while taking into account statistical uncertainties derived from the data and the prior uncertainty. We performed a global analysis on 404 areas included in the CSSE dataset [1].

The interactive chart below gives an overview of the course of the infection for each country. Data are updated every day.

<center><iframe width="800" height="400" frameborder="0" scrolling="no"
src="notebooks/plotly_chart.html"></iframe>
</center>

While China is now at the final stage of the spread, several countries in Europe are now facing the exponential phase. By quantifying the number of infected individual at the peak predicted by our model we found that Italy, Germany, France and Iran are at high risk of pandemic spread. During the exponential phase it is really hard to draw reliable estimates of when the diffusion of the virus will start displaying a reduction, therefore it is extremely important for these countries to strenghten the interventions to contain the eponential increase of new cases.  

<figure>
<center><img src="Figures/Figure_ranking_peak.png"/>
<figcaption> Fig. 3: Top 10 countries for maximum number of infected individuals</figcaption>
</center>
</figure>

<figure>
<center><img src="Figures/Figure_ranking_duration.png"/>
<figcaption> Fig. 4: Top 10 countries by duration of the outbreak</figcaption>
</center>
</figure>

## References
1. Dong, Ensheng, Hongru Du, and Lauren Gardner. "An interactive web-based dashboard to track COVID-19 in real time." The Lancet Infectious Diseases (2020).   
