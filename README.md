# Rcpp and RcppArmadillo notes
This is personal notes for studying to write a R package by using Rcpp and RcppArmadillo

## 1. vec to uvec
```
vec b;
uvec a = as<uvec>(b);
```

The above type casting will note work. The correct conversion should use conv_to function like below:

```
vec b;
uvec a = conv_to<uvec>::from(b);

```


conv_to< type >::from( X )

---
>Convert from one matrix type to another (eg. mat to imat), or one cube type to another (eg. cube to icube)

>Conversion between std::vector and Armadillo matrices/vectors is also possible

>Conversion of a mat object into colvec, rowvec or std::vector is possible if the object can be interpreted as a vector
