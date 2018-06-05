
# *Simplified-InPheRNo* 
# A method for Inference of Phenotype-relevant Co-expression Networks of Transcription Factors and Genes
#### Amin Emad (email: emad2 (at) illinois (dot) edu)
#### KnowEnG BD2K Center of Excellence
#### University of Illinois Urbana-Champaign


## Motivation
This repository contains computational tool which is a simplified version of InPheRNo (https://github.com/KnowEnG/InPheRNo). While InPheRNo reconstructs ‘phenotype-relevant’ transcriptional regulatory networks, considering the effect of multiple transcription factors (TFs) on the genes, Simplified-InPheRNo only considers the Pearson's correlation of each (gene, TF) pair separately. Simplified-InPheRNo works by combining the p-value of association of (transcription factors, genes) and the p-value of association between (gene-phenotype) using Fisher's method.

## Requirements

In order to run the code, you need to have Python 3.5 installed. In addition, the code uses the following python modules/libraries which need to be installed (the number in brackets show the version of the module used to generate the results in the manuscript):
- [Numpy](http://www.numpy.org/) (version 1.13.0)
- [Scipy](https://www.scipy.org/) (verison 0.19.1)
- [Pandas](http://pandas.pydata.org/) (version 0.20.2)

Instead of installing all these libraries independently, you can use prebulit Python distributions such as [Anaconda](https://www.continuum.io/downloads), which provides a free academic subscription. 


## Description of required inputs:
#### Input1: A file containing the list of transcription factors (TFs):
This is a csv file in which rows contain the names of the regulators (e.g. TFs). The file should not have a header. As an example see the file "Data/TF_Ensemble.csv". 

#### Input2: A file containing p-values of gene-phenotype associations:
This is a (gene x phenotype) csv file (see "Data/Pvalue_gene_phenotype_interest.csv" as an example). The rows correspond to target genes of interest. The p-value for TF-phenotype should not be included in this file. The value assigned to each gene represents the p-value of association between the expression of that gene and the variation in the phenotype across different samples obtained using a proper statistical test (e.g. a ttest for binary phenotype or Pearson's correlation for continuous, etc.). The file is assumed to have a header. 

Example:

|  | Pvalue |
| :--- | :--- |
| gene1 | 1E-22 |  
| gene2 | 5E-14 |
| gene3 | 3E-10 |

#### Input3: A file containing gene and TF expression data:
This is a (gene x samples) csv file containing the normalized gene (and TF) expression profiles across different samples. This file must contain expression of target genes provided in Input2 and TFs provided in Input1. The file has a header representing sample names. See "Data/expr_sample.csv" as a sample input.  

Example:

|  | sample1 | sample2 | sample3 |
| :--- | :--- | :--- | :--- |
| TF1 | 0.1 | 0.9 | 0.5 |
| TF2 | -0.3 | 0.5 | -0.6 |
| gene1 | -1.1 | 0.6 | 1.4 |
| gene2 | 0.9 | -2.3 | -0.3 |
| gene3 | 0.4 | 0.8 | 1.5 |
 
## Description of outputs:
This script generates two output files that by default will be located in a directory called "Results" placed in the current directory. 

#### Output1: Network_pvalue.csv
If default parameters are used to run the tool, Output1 will be a file called "Network_pvalue.csv", placed in "./Results" by default. This is a (gene x TF) file representing the phenotype-relevant co-expression network in which the values represent the combined p-values (using Fisher's method) representing the significance that 1) the gene and TF's expression are highly correlated and 2) the gene's expression is highly associated with the phenotype variation. One can then threshold these p-values to obtain a final network. 

#### Output2: Network_statistic.csv
If default parameters are used to run the tool, Output2 will be a file called "Network_statistic.csv", placed in "./Results" by default. This is a (gene x TF) file representing the phenotype-relevant co-expression network in which the values represent the statistic of the combined p-values (using Fisher's method) representing the significance that 1) the gene and TF's expression are highly correlated and 2) the gene's expression is highly associated with the phenotype variation. One can then threshold these statistics (higher is better) to obtain a final network. 

## Running Simplified-InPheRNo:
#### With default settings
To Run the code with default parameters, place all the three input files above in one folder. Then specify the following four arguments:
- input_directory: address of the data directory containing the three input files (e.g. "./Data")
- input_tf: name of Input1 file containing the name of regulators (e.g. "TF_Ensemble.csv")
- input_gene_phenotype_interest: name of Input2 containing p-value of gene-phenotype association (e.g. "Pvalue_gene_phenotype_interest.csv")
- input_expression: name of Input3 containing the expression of genes and TFs (e.g. "expr_sample.csv")

The following line shows how to run InPheRNo using the sample files:
```
python3 InPheRNo_simplified.py --input_directory ./Data --input_tf TF_Ensemble.csv --input_gene_phenotype_interest Pvalue_gene_phenotype_interest.csv --input_expression expr_sample.csv
```

#### With advanced settings
In addition to the arguments above, one can use the following optional arguments to change the default settings.
- -od, --output_directory (string, default='./Results'): Address of the output directory
- -onp, --output_network_pvalue (string, default = 'Network_pvalue.csv'): Name of Output1 file
- -tns, --output_network_statistic (string, default = 'Network_statistic.csv'): Name of Output2 file


## Sample inputs and outputs:
A set of sample inputs and sample outputs are provided in this repository in the directories "Data" and "Results". 
