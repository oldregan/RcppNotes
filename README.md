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


## 2. setdiff function by Armadillo

RcppArmadillo package itself doesn't implement setdiff function between two vectors. A robust solution is list as below:

```

vec arma_setdiff(arma::vec x, arma::vec y){
  vec x1 = arma::unique(x);
  vec y1 = arma::unique(y);
  //Rcout<<"x1:"<<x1<<endl;
  //Rcout<<"y1"<<y1<<endl;
  vec x_ret = x1;
  arma::uvec q1;

   if(x.is_empty() && y.is_empty()){
     return(x);
   }else{
     for (size_t j = 0; j < y1.n_elem; j++) {
       if(x_ret.is_empty()){
         break;
       }else{
         q1 = arma::find(x_ret == y1(j));
         if(q1.is_empty()){
           break;
         }
       }
       x_ret.shed_row(q1(0));
     }
     return(x_ret);
   }
}

```

## 3. rgamma function

```

double R::rgamma	(	double 	a,
double 	scl
)
```
In the above definition, the first paramter is actually shape.

A good reference [usage of rgamma generator]: url "http://gallery.rcpp.org/articles/gibbs-sampler/"


## 4. debug Rcpp and RcppArmadillo program

[a good reference see here(english)](http://kevinushey.github.io/blog/2015/04/13/debugging-with-lldb/)

## 5. size() function

size member function behave differently in Rcpp and RcppArmadillo package.

1. Rcpp: `.size()` function is a member function of `NumericalVector`. It returns the length of the vector and return type is integer
2. RcppArmadillo: If we want to get the length of `vec`, we should use its attribute `n_elem`
3. RcppArmadillo: `size` function return the dimension not the vector length
