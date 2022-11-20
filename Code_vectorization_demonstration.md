# A simple demonstration of the benefits of code vectorization

In the analysis of relatively small data sets, efficiency of code is not a major concern. Scaling up to larger data sets, CPU time and memory resources start to become a major concern. Whenever possible, we can apply the principal of code vectorization to dramatically speed up iterative processes. Users of the programming languages R and Python are constantly reminded that vectorized functions are far superior to traditional for-loops. However, many users without the experience of working with big data fail to understand the advantages and thus tend to write relatively inefficient code that scales poorly. Here is a simple demonstration which performs increasingly complex calculations using both vectorized and non-vectorized code, and then produces a figure showing the relative speed differences. 

````
################################################################################
#This script provides a simple demonstration of how vectorization can greatly
#reduce CPU cost relative to an equivalant for loop

#Written by Eric Dexter 20.11.2022
################################################################################
#Load required packages
library(ggplot2)

################################################################################
#For loop vs vectorization test
################################################################################
speedUp <- NULL #Initialize results vector
maxSize <- 27 #Maximum exponent size used in function A

for(i in 1:maxSize){
  
  A <- as.numeric(1:2^i)
  B <- as.numeric(1)
  C = 0
    #Here is the for loop version of the function
    start_time <- Sys.time()
    for(j in 1:length(A)) {
      C = C + A[j]*B[j]
    }
    end_time <- Sys.time()
    C_time <- as.numeric(end_time - start_time)

    #Here is the vectorized version of the function
    start_time <- Sys.time()
    D <- sum(A*B)
    end_time <- Sys.time()
    D_time <- as.numeric(end_time - start_time)

  speedDif[i] <- C_time - D_time
}

#Combine results into a dataframe
index <- 1:maxSize
df <- data.frame(index,speedDif)
################################################################################
#Plot results
ggplot(df, aes(y=speedDif, x=index)) +
  geom_point()+
  geom_line()+
  xlab("i value for function A*B^i") +ylab("Absolute (sec) speed increase by vectorization")
````

