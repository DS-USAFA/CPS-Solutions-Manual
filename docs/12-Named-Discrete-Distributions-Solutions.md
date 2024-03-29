# Named Discrete Distributions {#DISCRETENAMED}

## Objectives

1) Recognize and set up for use common discrete distributions (Uniform, Binomial, Poisson, Hypergeometric) to include parameters, assumptions, and moments.   

2) Use `R` to calculate probabilities and quantiles involving random variables with common discrete distributions.  


## Homework

For each of the problems below, **_1)_** define a random variable that will help you answer the question, **_2)_** state the distribution and parameters of that random variable; **_3)_** determine the expected value and variance of that random variable, and **_4)_** use that random variable to answer the question. 

We will demonstrate using 1a and 1b. 

### Problem 1 

The T-6 training aircraft is used during UPT. Suppose that on each training sortie, aircraft return with a maintenance-related failure at a rate of 1 per 100 sorties. 

a. Find the probability of no maintenance failures in 15 sorties. 

$X$: the number of maintenance failures in 15 sorties. 

$X\sim \textsf{Bin}(n=15,p=0.01)$

$\E(X)=15*0.01=0.15$ and $\Var(X)=15*0.01*0.99=0.1485$. 

$\Prob(\mbox{No maintenance failures})=\Prob(X=0)={15\choose 0}0.01^0(1-0.01)^{15}=0.99^{15}$

```r
0.99^15
```

```
## [1] 0.8600584
```

```r
## or 
dbinom(0,15,0.01)
```

```
## [1] 0.8600584
```

This probability makes sense, since the expected value is fairly low. Because, on average, only 0.15 failures would occur every 15 trials, 0 failures would be a very common result. Graphically, the pmf looks like this: 


```r
gf_dist("binom",size=15,prob=0.01) %>%
  gf_theme(theme_classic())
```

<img src="12-Named-Discrete-Distributions-Solutions_files/figure-html/hw8b-1.png" width="672" style="display: block; margin: auto;" />

b. Find the probability of at least two maintenance failures in 15 sorties. 

We can use the same $X$ as above. Now, we are looking for $\Prob(X\geq 2)$. This is equivalent to finding $1-\Prob(X\leq 1)$:

```r
## Directly
1-(0.99^15 + 15*0.01*0.99^14)
```

```
## [1] 0.009629773
```

```r
## or, using R
sum(dbinom(2:15,15,0.01))
```

```
## [1] 0.009629773
```

```r
## or
1-sum(dbinom(0:1,15,0.01))
```

```
## [1] 0.009629773
```

```r
## or
1-pbinom(1,15,0.01)
```

```
## [1] 0.009629773
```

```r
## or 
pbinom(1,15,0.01,lower.tail = F)
```

```
## [1] 0.009629773
```

c. Find the probability of at least 30 successful (no mx failures) sorties before the first failure.

$X$: the number of maintenance failures out of 30 sorties.

$X\sim \textsf{Binom}(n=30,p=0.01)$, and $\E(X)=0.3$ and $\Var(X)=0.297$. 

$\Prob(\mbox{0 failures})=\Prob(X=0)=0.99^{30}$

```r
0.99^30
```

```
## [1] 0.7397004
```

```r
##or 
dbinom(0,30,0.01)
```

```
## [1] 0.7397004
```

Using negative binomial, which was not in the reading but you can research:

$Y$: the number of successful sorties before the first failure. 

$Y\sim \textsf{NegBin}(n=1,p=0.01)$, and $\E(X)=99$ and $\Var(X)=9900$. 

$\Prob(\mbox{at least 30 successes before first failure})=\Prob(Y\geq 30)$

```r
1-pnbinom(29,1,0.01)
```

```
## [1] 0.7397004
```

d. Find the probability of at least 50 successful sorties before the third failure. 

Using a binomial random variable, we have 52 trials and need at least 50 to be a success. The random variable is $X$ the number of successful sorties out of 52.


```r
1-pbinom(49,52,.99)
```

```
## [1] 0.9846474
```

Or using a negative binomial, let

$Y$: the number of successful sorties before the third failure. 

$Y\sim \textsf{NegBin}(n=3,p=0.01)$, and $\E(X)=297$ and $\Var(X)=29700$. 

$\Prob(\mbox{at least 50 successes before 3rd failure})=\Prob(Y\geq 50)$

```r
1-pnbinom(49,3,0.01)
```

```
## [1] 0.9846474
```

Notice if the question had been exactly 50 successful sorties before the 3 failure, that is a different question. Then we could use either:


```r
dbinom(50,52,.99)*.01
```

```
## [1] 0.000802238
```

The $0.01$ is because the last trial is a failure.

Or 


```r
dnbinom(50,3,0.01)
```

```
## [1] 0.000802238
```



\newpage

### Problem 2  

On a given Saturday, suppose vehicles arrive at the USAFA North Gate according to a Poisson process at a rate of 40 arrivals per hour. 

a. Find the probability no vehicles arrive in 10 minutes. 

$X$: number of vehicles that arrive in 10 minutes

$X\sim \textsf{Pois}(\lambda=40/6=6.67)$ and $\E(X)=\Var(X)=6.67$. 

$\Prob(\mbox{no arrivals in 10 minutes})=\Prob(X=0)=\frac{6.67^0 e^{-6.67}}{0!}=e^{-6.67}$

```r
exp(-40/6)
```

```
## [1] 0.001272634
```

```r
##or
dpois(0,40/6)
```

```
## [1] 0.001272634
```

b. Find the probability at least 50 vehicles arrive in an hour. 

$X$: number of vehicles that arrive in an hour

$X\sim \textsf{Pois}(\lambda=40)$ and $\E(X)=\Var(X)=40$. 

$\Prob(\mbox{at least 50 arrivals in 1 hour})=\Prob(X\geq 50)$

```r
1-ppois(49,40)
```

```
## [1] 0.07033507
```

c. Find the probability that at least 5 minutes will pass before the next arrival.

$X$: number of vehicles that arrive in 5 minutes

$X\sim \textsf{Pois}(\lambda=40/12=3.33)$ and $\E(X)=\Var(X)=3.33$. 

$\Prob(\mbox{no arrivals in 5 minutes})=\Prob(X=0)=\frac{3.33^0 e^{-3.33}}{0!}=e^{-3.33}$

```r
exp(-40/12)
```

```
## [1] 0.03567399
```

```r
##or
dpois(0,40/12)
```

```
## [1] 0.03567399
```


\newpage

### Problem 3  

Suppose there are 12 male and 7 female cadets in a classroom. I select 5 completely at random (without replacement). 

a. Find the probability I select no female cadets. 

$X$: number of female cadets selected out of sample of size 5

$X\sim \textsf{Hypergeom}(m=7,n=12,k=5)$ and $\E(X)=1.842$ and $\Var(X)=0.905$. 

$$
\Prob(\mbox{no female cadets selected})=\Prob(X=0)=\frac{{7\choose 0}{12\choose 5}}{{19\choose 5}}
$$

```r
choose(12,5)/choose(19,5)
```

```
## [1] 0.06811146
```

```r
##or
dhyper(0,7,12,5)
```

```
## [1] 0.06811146
```


b. Find the probability I select more than 2 female cadets. 

Using the same random variable:
$$
\Prob(\mbox{more than 2 female})=\Prob(X>2)=1-\Prob(X\leq 2)
$$


```r
1-phyper(2,7,12,5)
```

```
## [1] 0.2365841
```

```r
##or
sum(dhyper(3:5,7,12,5))
```

```
## [1] 0.2365841
```



## [Textbook](https://ds-usafa.github.io/Computational-Probability-and-Statistics/DISCRETENAMED.html) {-}
