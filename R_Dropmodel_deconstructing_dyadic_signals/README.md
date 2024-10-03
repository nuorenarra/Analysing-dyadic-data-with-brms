
**README**

# Deconstructing dyadic signal using a drop-model 

## Overview

This is a run-through of the dropmodel method of deconstructing dyadic signals in the data to the level of units of the response variable. We have previousy made some dyadic (pairwise) data on social associations, spatial distances, temporal distances and microbiome similarities of pairs of mice (See: R_Making_Dyadic_data) and then constructed a dyadic multimembership betaregression model (See: R_Modeling_dyadic_data) to predict microbiome similarity (Jaccard index= proportion of shared gut microbial taxa among two individual mice) with their social assocoiation, spatial distance and other pairwise covariates. The purpose of such model is to explore if microbiome sharing is influenced by social contact and/or spatial proximity between individuals. This is a way of modeling social and environmental transmission pathways of microbiota. Our model shows that both social and spatial associations have a significant effect on microbiota sharing, but social effect is manifold stronger. The more socially associated the mice, the more similar microbiome they have. The further away they are of each other (the less they are exposed to the same environment), the less similar their microbiome is.


```
model<-readRDS("model1.rds")
summary(model)
```


These social and spatial signals in microbiome composition currently exist in the level of the whole microbiome. However, to see which specific microbial taxa are particularly socially or spatially transmitted, we now want to deconstruct these signals to the level of individual microbial taxa. An intuitive way to do this would be to predict sharing of each and every taxon across each and every pair of host individuals with the same social association and spatial distance. This is possible, but the model is computationally very heavy and since the taxa vary in their prevalence and variability among hosts, this kind of model would perhaps be influenced by technical aspects of the data much more than the biological differences between taxa. So, we will introduce another way of deconstructing the whole-community level signals to taxon-level: the drop-model.

We can quatify importance of each bacterial genus in driving each transmission signal by dropping each taxon in turn from the microbiome data, recalculating the microbiome similarity index (Jaccard) and re-running the above-described dyadic model. We can then calculate an “importance score” for each effect of interest (social association or spatial distance), reflecting the extent to which dropping a genus from the analysis **reduced the certainty of that effect estimate**. Specifically, the Importance of Taxon G for effect E can be calculated as the increase in the 95% credible interval width (CIw) when G is excluded (CIwexcl¬ -CIwincl¬) relative to the baseline credible interval width when G is included (CIwincl¬), divided by the square root of the number of ASVs (n.ASV) assigned to taxon G: 

〖Importance〗_GE=(〖(CI〗_excl-〖CI〗_incl)/〖CI〗_incl)/√(n.ASV)




