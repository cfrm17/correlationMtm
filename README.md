# Correlation MTM Procedure

The model for valuation of CDOs is well documented. An essential input into the model is the correlation between the obligors in the collateral pool. Historically, this correlation has been derived using a KMV based approach.

In an attempt to make the valuation of these trades more consistent with market conditions (ref., https://finpricing.com/lib/EqBarrier.html), a new method for implying these correlations from the tracer market has been proposed. The methodology involves two steps: implying a correlation from the tracer market and then mapping the actual trades to these tranches to determine the implied correlation used to value these.

Specifically, we examined the proposed procedure for mapping tracer tranches to non-index CDO tranches in a portfolio. The goal of the procedure is to be able to use the tracer market to imply a correlation suitable for marking the market of the CDOs.

We first need to determine an market implied correlation for the tracer tranches. In order to do so, each tranche is valued at various constant correlations ranging from 0 to 0.7 in increments of 0.1. The implied correlation for each tranche is then found by linear interpolation by matching the model price with the market price. Note that this approximation might not be ideal if the market value versus correlation is not linear.

The figure 1 shows the various values for the tracer tranches using the model under various (flat) correlations. Note that there are two potential sources of difficulty or error that may arise in implying a correlation:
•	The values are not linear versus correlations, especially for the mezzanine tranches. Therefore a non-linearity error will occur with varying degrees of materiality depending on where the market is trading. Note that this error is purely a function of efficiency, as this could be reduced with increased granularity (assuming there is only one solution, see point below). 
•	There may be two possible solutions of an implied correlation, especially for example the 3% − 7% tranche. This is a significant difficulty, and is one of the primary motivations for the methodology proposed by JPMorgan to imply correlations from the tracer market.

We are told that Middle Office will add additional checks to insure that the nonlinearity will not be material misstatement in P/L. Furthermore, Middle Office assured us that when there are two solutions for the implied correlations, the more conservative of the two correlations will be used (i.e. the correlation that provides the lower valuation). This will be done on a trade-by-trade basis.

 

Figure 1: Value of tracer tranches under various choices of input correlation in the GCP
model.


In order to use these implied correlations from the tracer market, the various CDO trades in the portfolio need to be mapped to these tracer tranches. While there might be an obvious approach to mapping simple CDOs to tracers (i.e. pick the tracer tranche that most closely resembles the specific GCP trade), this is not true for more complicated CDOs. The method chosen to do this mapping is to choose, for each trade, the “best-hedge” tracer tranche under varying correlations.

In order to implement, one needs to define exactly what is meant by “best-hedge”. Let V m(_) correspond to the value of the mth GCP trade using a constant correlation _, and V tn(_) correspond to the value of the nth IBOXX tracer tranche using a constant correlation _. For a given correlation _, define _mn(_) as

 			(1)

It should be noted that the value of _ = 0.3 is chosen as it corresponds closest to the KMV correlations for most trades.

For each tracer tranche n, we then solve for the weight !n that minimizes the function

 				(2)
for
 			(3)

The “efficiency ratio” for a particular tranche n and particular trade m is then defined as

 	(4)

where the maximum is taken over all values of _ = 0, 0.1, 0.2, ..., 0.7. This is then repeated for all tracer tranches n, and the “best-hedge” is defined as the tracer tranche with the highest efficiency ratio. There are, of course, a number of methodologies one can use to define what is meant by the “best hedge”, and the methodology presented is just a particular choice.


