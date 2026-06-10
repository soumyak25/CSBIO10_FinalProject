# CSBIO10_FinalProject
Analysis of Gene Variants Associated with Anxiety Disorders in a GWAS Study

**Contributors**: Soumya Kalle, Ananya Atri, Yifan Zhuo, Julia Lohman, and Jacqueline Chavarry

**Professor**: Dr. Christy Lee @ UCLA

## **Research Question**

*Jackie: Research question*

## **Data Pre-Processing**

Size of original dataset — — —

* 1321 x 23
* 2.7 MB


Features of Interest — — —

* SNP
* Chromosome
* Base Pair Position
* Odds Ratio (protective vs risk-increasing variant)
* Standard Error Associated with the Odds Ratio

Each plot (see below) requires additional processing

## **Methodology**

### Manhattan Plot 

*Julia: 1) the inspiration + how you made your own, 2) next steps, 3) mention intermediary plots*
First, I subsetted the data so that we only had the columns SNP, CHR, BP, and P-values. The I made the chromosome number be recognized as a discrete number instead of continuous. This was so that I could arrange the base pairs to be matched with their corresponding chromosomes. Then I needed to make sure that the base positions weren't overlapping (because base position restarts with every chromosome), so I multiplied each chromosome number by 1,000,000,000 and added the base positions so that I could plot all of the SNPs on one continuous x-axis. Each color corresponds to a chromosome, and the y-axis is the -log10(p-values) so that it is easier to interpret. The dark blue line that goes across the graph, which signifies our significance level. I did do a facet wrap so that we could visualize each chromosome independently, however the result was cramped and looked rather unpleasent so I decided not to proceed in that direction.
***
### Funnel Plot    
#### **Motivation — — —**

Since we are focusing on Odds Ratios (OR)  so much, we should check that our data is not biased.
A common plot for meta-analyses is a simple Funnel Plot.

#### **Generating the Plot — — —**

A funnel plot requires each log(OR) to be plotted against its respective Standard Error (SE).
1. x-axis: log(OR)
2. y-axis: SE

To create a funnel shape, the y-axis is reversed

Next, plot the bounds of the funnel (we choose 95% confidence). If the plot is symmetric, the meta-analysis is unbiased. 

#### **Analysis — — —**

Our plot is symmetric and therefore unbiased. Thus, we can confidently use the OR feature for later analysis.

---

### Volcano Plot    

#### **Statistics — — —**
1. Add 2 columns for the p-values (P) adjusted for Benjamin-Hochberg (BH) and Bonferroni (Bon), respectively.
2. Subset the dataset for  p-values < 0.05
   
   * The Bonferroni Correction subsetted away around 500 SNPs
3. The Odds Ratio (OR) is a score of whether a SNP has: 
   * higher odds (OR > 1),
   * lower odds (OR < 1),
   * or no association (OR = 1) 
   of the outcome being ANX.

   Comparing ORs in relation to Ps helps us understand the significance of the ORs.
  
#### **Generating the Plot — — —**

We choose to analyze the Odds Ratio to identify the most significant SNPs that increase the probability of protective or risk factors for Anxiety disorders. To do this, we plot the OR against the p-values.

1. x-axis: log2(OR)
2. y-axis: -log(P)

We can also check which of the SNPs pass only the BH correction by color coding the SNPs.

Finally, we can choose which SNPs we consider as most significant. We used unadjusted P < 1e-11.
1. "rs58825580"
2. "rs7110863"
3. "rs10959883"
4. "rs11241568"
5. "rs4976976"
6. "rs10476497"
7. "rs77960"

#### **Analysis — — —**

The Volcano plot visualizes the risk and protective factors of each SNP in the GWAS studies. The rightmost SNPs, have OR > 1, meaning they have a higher probability of being associated with ANX. The uppermost SNPs, have the lowest p-values, and thus, their OR scores are most significant. From this, we can deduce which gene variants are most associated with ANX. 


From this plot, we reaffirm that all the SNPs used in this GWAS study were properly adjusted, as all p-values pass the Benjamin-Hochberg test. In addition, we can see that a majority of the SNPs also have a significant p-value when adjusted with the Bonferroni correction as well. From the Volcano-plot, we are able to visualize THE MOST significant SNPs. We can cross-reference these SNPs with those discovered as most significantly associated with ANX in the study.

#### **Next Steps — — —**
All the SNPs we found to be significantly associated in some capacity (protective or risk) are not only phenotypically linked to ANX but also to various other conditions, such as cognitive, psychiatric, and cardiometabolic traits.

I found the link between ANX and cardiometabolic health interesting. There is literature exploring how cardiovascular drugs can double as anxiety therapies:

    Repova, K., Aziriova, S., Krajcirovicova, K., & Simko, F. (2022). Cardiovascular therapeutics: A new potential for                                  anxiety treatment?. Medicinal research reviews, 42(3), 1202–1245. 
             https://doi.org/10.1002/med.21875
          
I tried long and hard to find a dataset of persons with ANX taking a CV medication vs a placebo and resultant ANX levels. I could not find such publicly available datasets. Further study of this or designing a similar experiment could be interesting for further analysis. 

---

### Bar graph    

#### **Generating the Plot — — —**

We are looking at the SNP pairs that are very close to each other in terms of distance. We want to see if the SNPs in the same pair are both significant and are on the same genes. If they are on the same genes and at least one of them is significant, they can be used as potential markers in future disease studies to study genes related to anxiety.

We made a bar graph of very close SNP pairs and their p-values, with colors to distinguish the pairs.


*Julia: Creating the distance table for chr1*
When doing the Manhattan Plot, I saw some overlap in the points that marked the SNPs of interest, and wondered if the distance between any of them were actually close enough to make an impact. What I ended up doing was making a table that listed the differences between each base position and then classified them by the amount of nucleotides they were away. The categories were: "very far, (distance over 1,000,000 bp away), "far" (1,000,000 - 100,000) "close" (20,000 - 100,000) and "very close" (less than 20,000). The categories represent, respectively: unrelated, on the same chromosome, genetic cluster/linkage disequilibrium, and inside the same gene. For simplicity, I only made a table of chromosome 1, however we eventually analyzed the remaining chromosomes as well.


*Jackie: Creating the distance table for all chromosomes*

*Subsetting all the tables and creating a list*: We subsetted the distance table and pulled out close pairs of SNPs; we added these pairs of SNPs, their p-values, and the genes they are located on to an R dictionary (list). We then created a data frame containing this information, and 
formatted it for ggplot2. 

*Intermediary plot*: We created an intermediary plot at this stage. It includes pair-wise graphing of 'Very Close' SNPs, separated by color.

*Further Work*: We continued to refine the data frame after this, with the goal of replacing SNP names with just the gene that they are associated with. We wanted each pair of SNPs to be marked on the x-axis by only its associated gene, and we wanted to change the legend to reflect this change as well.

*Others eventually: Write about plotting the bar graph and about the intermediary plots*

## **Citations**
GWAS Study and Dataset:

    Strom, N.I., Verhulst, B., Bacanu, SA. et al. Genome-wide association study of major anxiety 
       
           disorders in 122,341 European-ancestry cases identifies 58 loci and highlights 
       
           GABAergic signaling. Nat Genet 58, 275–288 (2026).
       
           https://doi.org/10.1038/s41588-025-02485-8

SNP Base Pair Position Pair Distance Classifications:

Average gene size - Human Homo sapiens - BNID 105336. (2026). Harvard.edu. https://bionumbers.hms.harvard.edu/bionumber.aspx?id=105336&ver=6Linkage Disequilibrium 101: What LD Measures and 
When It Matters. (2019). Cd-Genomics.com. https://www.cd-genomics.com/pop-genomics/resources/linkage-disequilibrium-overview.html

The Genes on Which the Significant SNPs Exist, According to the Volcano Plot:

*Eva: cite*




