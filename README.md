# monte-carlo-option-pricer

Monte Carlo pricing of European and arithmetic-average Asian call options in
C++, with the accompanying derivation in `notes.tex`.

## Approach

The European call is priced first, purely as a control. It has a closed form
under Black-Scholes, so simulating it gives a way to check the estimator
against a known answer. Only once the simulated price falls inside the
confidence interval around the analytic price is the Asian option priced,
where no closed form exists because a sum of lognormal variables is not
itself lognormal.

European paths jump straight to expiry in a single step, since only the
terminal price matters. Asian paths are stepped daily, 252 steps, because the
payoff depends on the average along the path.

## Results

With `S0 = K = 100`, `r = 5%`, `sigma = 20%`, `T = 1`, two million paths:

```
European call
  Black-Scholes exact : 10.4506
  Monte Carlo estimate: 10.4520  +/- 0.0204
  Absolute difference : 0.0014
  Validation          : within 95% interval

Asian call, arithmetic average
  Monte Carlo estimate: 5.7661  +/- 0.0350
```

The Asian call prices at roughly half the European. Averaging along the path
damps the variance of the terminal quantity, and less variance means less
optionality to pay for.

Convergence is order `M^-1/2`, so halving the error costs four times the
paths. Two million European paths run in about 0.15 seconds.

## Building

```
g++ -O2 -std=c++17 -static mc_pricer.cpp -o mc_pricer.exe
```

`-static` bundles the runtime libraries into the executable so it runs without
the compiler's DLLs on the PATH.

## Write-up

`notes.tex` contains the model, the estimators and the results table.
`notes.pdf` is the compiled version.
