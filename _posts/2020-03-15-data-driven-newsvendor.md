---
layout: post
title:  "Data-driven Newsvendor - Is the Estimate-then-optimize Method Really Bad?"
date:   2020-03-15 21:24:11 -0500
categories: supply_chain 
comments: true


---

The classical newsvendor problem deals with the optimal order quantity problem when the demand is random. Consider the situation that the demand is unknown but we have certain amount of historial data. One approach is called "estimate-then-optimize", which fits the data to a demand distribution and then optimize the objective under the estimated distribution. Some literature argues this two-step approach is not optimal and produce inferior solution. But is it real that this method is bad?

#### Basic newsvendor setting 

We use $D$ to denote the random demand and $q$ the order quantity. Denote the items' cost to be $c$ and selling price to be $s$. Suppose the items do not have salvage value. From now on we assume the demand follows exponential distribution with unknown parameter $\lambda$. We have $n$ i.i.d. observations of the demand. 

The random profit if the newsvendor orders $q$ units and under uncertain demand $D$ is 

[[\phi(q,D)=s\min\{q,D\}-cq.]]

The objective of the newsvendor is maximize the expected future profit. 

[[\max_q \psi(q)=\mathbb E_{D}[\phi(q,D)].]]

#### Estimate and then optimize

When we fit the data $\bold d$ to a unknown parametric distribution $F$, we get $\hat F$ as well as the distribution of $F$ in the functional space. For example, in our case, the MLE of exponential distribution is $\hat \lambda =n/\sum d_i=1/\bar d$, which is consistant. But we also have $\bar d\sim Gamma(n,n\lambda)$, which means $\lambda \sim Gamma(n,n\bar d)$. Hence we not only have a point estimate $\hat \lambda$, but also distribution of $\hat \lambda$. 

The objective of the newsvendor is maximize the following expected profit under uncertain demand $D_\lambda$and uncertain parameter $\lambda$. 

[[\max_q \psi(q)=\mathbb E_\lambda\left[\mathbb E_{D_\lambda}[\phi(q,D_{\lambda})\mid\lambda]\right].]]

Plug in $\phi(q,D_\lambda)$, 

[[\begin{aligned}
\psi(q)&=\mathbb E(\int_0^qsx dF_D(x;\lambda)+\int_q^\infty sqdF_D(x;\lambda))-cq&bsol;&bsol;
&= s[\int_0^\infty \int_0^qx dF_D(x;y)dF_{\lambda}(y)+\int_0^\infty\int_q^\infty qdF_D(x;y)dF_\lambda(y)]-cq&bsol;&bsol;
&= s[\int_0^\infty \int_0^qx dF_D(x;y)dF_{\lambda}(y)+q\int_0^\infty \bar F_D(q;y)dF_\lambda(y)]-cq.
\end{aligned}]]

One can verify the above function of q is concave by taking the second derivative and see if it is less than 0. Hence the optimal order quantity $q^*$ can be obtained by the First Order Condition (FOC). Here we take derivative respect to q,

[[\begin{aligned}
\frac{d\psi}{dq}&= s[q\int_0^\infty  f_D(q;y)dF_{\lambda}(y)+\int_0^\infty \bar F_D(q;y)dF_\lambda(y)-q\int_0^\infty f_D(q;y)dF_\lambda(y)]-c\\
&= s\int_0^\infty \bar F_D(q;y)dF_\lambda(y)-c.
\end{aligned}]]

FOC is $d\psi/dq=0$, 

[[\int_0^\infty \bar F_D(q^*;y)dF_\lambda(y)=c/s.]]

where $dF_\lambda(y)=\frac{(n\bar d)^n}{(n-1)!}y^{n-1}\exp(-n\bar dy)dy$, and $\bar F_D(q;y)=\exp(-qy)$. Then 

[[
\int_0^\infty \bar F_D(q^*;y)dF_\lambda(y)&=c/s \\\
\frac{(n\bar d)^n}{(n-1)!}\int_0^{\infty}y^{n-1}\exp(-n\bar dy-q^*y)dy&=c/s\\\
\frac{(n\bar d)^n}{(n\bar d+q^*)^n}&=c/s.
]]

Finally,

[[q^*=n[(s/c)^{1/n}-1]\bar d.]]


Here we replicate the operational statistics solution introduced in Liyanage and Shanthikumar (2005). 

#### Normal distribution

Suppose we restrict the distribution to a parametrized family, e.g. normal $N(\mu,\sigma^2)$. Then we know the MLE is $\hat \mu =\bar d$ and $\hat \sigma^2=\sum(d_i-\bar d)^2/n$. The MLE for variance is consistent but biased. The theorem says the sample mean and sample variance are independent with $\sqrt{n} \bar d\sim N(\mu,\sigma^2), n(\bar{d^2}-(\bar d)^2)/\sigma^2\sim \chi^2_{n-1}$. 