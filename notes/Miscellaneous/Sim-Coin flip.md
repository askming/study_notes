# :bulb: Q: fair coin, # of flips to get two consecutive side (H or T)

1. the expected no. of flips to get two consecutive same sides (either H or T)
```R
num = function(p=0.5, nsim=1e6){
    n = rep(1, nsim)
    for (i in 1:nsim){
        side_pre = rbinom(1, 1, p)
        eql = FALSE
        while(eql==FALSE){
            n[i] = n[i] + 1
            side_post = rbinom(1, 1, p)
            eql = (side_pre == side_post)
            side_pre = side_post
        }
    }
    mean(n)
}

num()
```


2. expected no. of coin flip to get tow consecutive H
```R
num = function(p=0.5, nsim=1e3){
    n = rep(1, nsim)
    for (i in 1:nsim){
        side_pre = rbinom(1, 1, p)
        side_pre = ifelse(side_pre==1, 'H', 'T')
        eql = FALSE
        while(eql==FALSE){
            n[i] = n[i] + 1
            side_post = rbinom(1, 1, p)
            side_post = ifelse(side_post==1, 'H', 'T')
            eql = (side_pre == 'H' & side_post=='H')
            side_pre = side_post
        }
    }
    mean(n)
}

num()
```