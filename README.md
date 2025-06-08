# Ancestry-ML

## What is this?

Ancestry analysis is a broad term that can describe many different analyses, but at the end of the day, there are people curious about possible ancestral mixtures in their genome and which areas of the world their genome can relate to. This repo aims to look at an individual's genome and ascertain which global populations does the whole genome or parts of the genome resemble the most. 

Until now, there are many researchers that have used many sophisticated techniques to do this kind of analysis. As a person interested in deep learning, I believe neural networks can constitute a simpler but just as effective replacement for the whole ancestry analysis workflow for customer genetics.

One important bias in ancestry analysis is the labelling of the initial ancestral populations. Populations of the same species diverge overtime seperated by geography, and this has happened to human species in the continental level the most. In contrast, there have been much little seperation in sub-continental level. If one wants to do ancestry analysis, they should classify the world populations into distinct groups, but this process is error-prone and affected by human biases greatly, as there is great cultural diversity between human groups even among the smallest distances. In my opinion, if possible, unsupervised learning techniques should be used to cluster human populations depending on their genetic similarity so we can remove the human bias.

As an input data, we will need phased VCF files that are converted to parquet format. Genotypes can be phased by BEAGLE. The phasing operation effectively doubles the amount of samples we have. VCF files can be converted to parquet files by [vcf2parquet](https://github.com/yusufozgur/vcf2parquet). To not deal with different genome build versions, the program will use rsids as indices. To reduce complexity of the model, program will keep only biallelic variants. [Allen Ancient DNA Resource (AADR)](https://reich.hms.harvard.edu/allen-ancient-dna-resource-aadr-downloadable-genotypes-present-day-and-ancient-dna-data) is a good source for the data.

A variational autoencoder will be trained on global genotypes. The lower dimensional representation of the human genomes can then be created by the results of the encoder, the latent space. Various clustering techniques can then be used on this latent space, I will use k-means in this repo. Then various evaluation metrics will be used to help choose optimal cluster number. This will be done in a marimo notebook, so user can interactively train and visualize clusters, choose optimal cluster number, name clusters, and then save sample-cluster pairs in an annotation file.

In my previous experiments, I have seen Multi Layer Perceptrons (MLPs) with a few hidden layers and ReLu activation functions have good performance. There will be a Global MLP classifier, taking the input snps for each phased 22 autosomes, and getting trained to predict probabilities of genome belonging to a population specified by the annotation file provided, which can be produced from variational autoencoder in a previous step.

As for the local ancestry inference, chromosomes will be divided into windows of a set variant count, and then an MLP with the same architecture of global ancestry inference system will be trained on each chromosome window. This can then be visualized in a karyotype plot or chromosomal position vs probability line graph.

## TODO

-   [ ] Variational Autoencoder
    -   [ ] Latent Space Clusterer
-   [ ] Global MLP Classifier
-   [ ] Local MLP Classifier

## Possible Considerations

-   [ ] Linkage disequilibrium based variant pruning