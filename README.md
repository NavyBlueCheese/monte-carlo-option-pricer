# monte carlo options pricer

Monte Carlo pricing of European and arithmetic-average Asian call options in
C++ with the accompanying derivation in `notes.tex`.


## Results

 `S0 = K = 100`, `r = 5%`, `sigma = 20%`, `T = 1`, two million paths

```
European call
  Black-Scholes exact : 10.4506
  Monte Carlo estimate: 10.4520  +/- 0.0204
  Absolute difference : 0.0014
  Validation          : within 95% interval

Asian call, arithmetic average
  Monte Carlo estimate: 5.7661  +/- 0.0350
```

The Asian call prices at roughly half the European >> averaging along the path
damps the variance of the terminal quantity

Convergence is order `M^-1/2` >> halving the error costs four times the
paths. 
Also two million European paths run in about 0.15 seconds.


## Write-up

`notes.tex` contains the model, the estimators, and the results table
`notes.pdf` is the compiled version
