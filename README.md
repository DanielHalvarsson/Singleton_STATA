# How to locate and remove singleton observations for a two-way fixed effect model. 

Essentially it amounts to dropping observations with single observations at both levels of the fixed effects. Note that this task is automatically implemented in models such as 'reghdfe'. However, to construct a connected data set, which is required for the identification of the fixed effects in the same model, singleton observations must be accounted for before hand.

With the levels of e.g. workers and firms, it means dropping individual workers with a single observation and individual firms also with a single observation. Say you start by dropping all workers with only one observation, it can in turn affect the number of firms that remains with a single observation. By then dropping all firms with a single observation, it can result in additional workers with only one observation. Hence the problem is iterative and a proceedure needs to be repeated until no single worker or firm observation remains. 

For any panel of individual workers (Person) and firms (Firm), singleton observations can effectively be identified and removed by the following simple iterative algorithm using Stata.

```Stata
* Remove singletons iteratively:
local r = 1
local s = 1
local singletons = 0 		// Iteratively counts the number of singletons
while (`r' > 0 & `s' > 0){
	cap drop n
	bys Person: gen n = _N // Number of worker obs
	count if n == 1		   // Number of singleton person observations
	local r = r(N)
	local singletons = `singletons' + r(N)
	drop if n == 1
	
	cap drop m
	bys Firm: gen m = _N  // Number of firm obs 
	count if m == 1       // Number of singleton firm observations
	local s = r(N)
	local singletons = `singletons' + r(N)
	drop if m == 1	
}
mat singletons = `singletons'
```
