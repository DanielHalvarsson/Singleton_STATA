# Singleton_STATA

How to locate singleton observations in a two-way fixed effects model. 

Essentially it amounts to dropping observations that with single observations at both levels of the fixed effects. With workers and firms, it means dropping individual workers with a single observation and individual firms also with a single observation. Say you start by dropping all workers with only one observation, it can in turn affect the number of firms with a single observation. By then dropping all firms with a single observation, it can result in additional workers with only one observation. Hence the problem is iterative and needs to be repeated until no single worker or firm observation remains. 

For any panel of individual workers (Person) and firms (Firm), singleton observations can be identified and dropped by the following simple iterative algorithm using Stata.

 
