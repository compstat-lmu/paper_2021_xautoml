# xAutoML - Explaining AutoML Systems 

## Use-Cases 

### (1) Tuning `xgboost` with `mlrMBO`

We use [`mlrMBO`](https://github.com/mlr-org/mlrMBO) to tune an `xgboost` classifier. 

We want to **minimize** the function f: X --> Y with the following specifications: 

* Search space X: 

```
ps = makeParamSet(
# do early stopping instead for the bigger datasets
    makeNumericParam("nrounds", lower = 0, upper = 12, trafo = function(x) round(2^x)), # 2^12 = 4096	
    makeNumericParam("eta", lower = -7, upper = 0, trafo = function(x) 2^x), # 2^(-7) = 0.007 < 0.01
    makeNumericParam("gamma", lower = -7, upper = 6, trafo = function(x) 2^x), 
    makeIntegerParam("max_depth", lower = 3, upper = 20),
    makeNumericParam("colsample_bytree", lower = 0.5, upper = 1),
    makeNumericParam("colsample_bylevel", lower = 0.5, upper = 1),
    makeNumericParam("lambda", lower = -10, upper = 10, trafo = function(x) 2^x),
    makeNumericParam("alpha", lower = -10, upper = 10, trafo = function(x) 2^x),
    makeNumericParam("subsample", lower = 0.5, upper = 1)
)
```
* Objective function f(x): 
    * A single function evaluation corresponds evaluating an `xgboost` model with a configuration `x` by a 3-fold cross-validation. 
    * Returned is the 3-fold cross-validated performance in terms of **mean misclassification error*. 


`mlrMBO` is configured as follows: 

|  |  |  |
| :---         |     :---:      |          ---: |
| Surrogate Model:         | Gaussian Process (default)        | Matérn3/2-kernel    |
| Infill Criterion         | Lower Confidence Bound (LCB) | lambda in {0.5, 1, 2}   |
| Infill Optimization      | Focus search (default)       |   /                     |
| Proposals per Iteration  |   1                          |   /                     |
| Termination Criterion    |   200 Evaluations of f(x)    |   /                     |


Datasets are taken from [AutoML Benchmark Data](https://openml.github.io/automlbenchmark/automl_overview.html): 

|  |  |  |
| :---         |     :---:      |          ---: |
| kc1         | :heavy_check_mark: |     |
| blood-transfusion-service center         | :heavy_check_mark: |  |
| numerai28.6      |   :heavy_check_mark:     |   /                     |
| phoneme  |    :heavy_check_mark:      |   /                     |
| sylvine    |  :heavy_check_mark:     |   /                     |


As "ground-truth", f was evaluated on a randomLHS, to evaluate the bias that arises from analyzing "optimization data" as compared to randomly sampled data. 


#### Description of the datasets

#### Status 


### Surrogate Benchmark: Pseudo-tuning MLP's with the help of a surrogate model created  a by on the [AutoML Benchmark Data](https://openml.github.io/automlbenchmark/automl_overview.html) with `mlrMBO`

#### Description of the "AutoML" System

#### Description of the datasets

#### Status 
