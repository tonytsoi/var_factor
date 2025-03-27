# Overview of Risk Factor Models and Its Application in VaR Estimation
In my previous article, I have discussed the basic estimation methods of Value at Risk (“VaR”) and Expected Shortfall for a simple equity portfolio. As the number of assets increases in a portfolio, the number of observations required would also increase to achieve reliable estimates of the covariance matrix and any subsequent risk metrics.

This becomes more of an issue when some stocks in the portfolio might only have started trading recently and are not having a sufficiently long enough price history.

One way to address this issue is to incorporate a Risk Factor Model as an intermediate step in the VaR estimation. In addition, any common sources of risk can also be identified using a risk factor model.

In this <a href=https://medium.com/@tsoiyingkit/overview-of-risk-factor-model-and-its-application-in-var-estimation-8eff11f90bef>article</a>, I first provide a brief overview of some of the foundational theories related to risk factor model, including the Modern Portfolio Theory (“MPT”), the Capital Asset Pricing Model (“CAPM”) and the Arbitrage Pricing Theory (“APT”).

I then discuss how fundamental factors are constructed in the Fama-French Factor Model as well as the Barra Factor Model.

I also discuss how statistical factors are generated using Principal Component Analysis (“PCA”), and illustrate with the US Treasury yield curve data as an example on how the level, slope and curve factors can be identified. 

Lastly, I set out how VaR and Expected Shortfall can be calculated with a Risk Factor Model using the Parametric Method.

![ff_fac](https://github.com/tonytsoi/var_factor/blob/main/ff_fac.png?raw=true)

