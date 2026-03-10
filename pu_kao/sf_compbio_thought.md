# Compbio

## 1. In words, describe the two input data files? What is the format of each?

### Answer

The first dataset: Based on the filename - BRCA(Breast Cancer-related gene) and column name - `ER_STATUS`, `HER2_STATUS`, this is a breast tumor gene sequencing data from METABRIC cohorts, [conducted from Nature 2012 & Nature Communication 2016](https://www.cbioportal.org/study/summary?id=brca_metabric).

```
Total Length:2509 
Columns: 'PATIENT_ID''SAMPLE_ID''CANCER_TYPE''CANCER_TYPE_DETAILED''ER_STATUS''HER2_STATUS''GRADE''ONCOTREE_CODE''PR_STATUS''SAMPLE_TYPE''TUMOR_SIZE''TUMOR_STAGE''TMB_NONSYNONYMOUS'
```

The format is tab separated text files with one row per patient and mutliple clinical variables as columns


The second dataset: Based on the column name - `Hugo_Symbol`, `Entrez_Gene_Id`, `NCBI_Build`, `Transcript_ID`, `RefSeq`, `Protein_position` and many kinds of variants reported on multiple transcripts with aggregated mutation information, this is a [Mutation Annotation Format (MAF)](https://docs.gdc.cancer.gov/Data/File_Formats/MAF_Format/).


```
Total Length: 17272
Columns: 'Hugo_Symbol''Entrez_Gene_Id''Center''NCBI_Build''Chromosome''Start_Position''End_Position''Strand''Consequence''Variant_Classification''Variant_Type''Reference_Allele''Tumor_Seq_Allele1''Tumor_Seq_Allele2''dbSNP_RS''dbSNP_Val_Status''Tumor_Sample_Barcode''Matched_Norm_Sample_Barcode''Match_Norm_Seq_Allele1''Match_Norm_Seq_Allele2''Tumor_Validation_Allele1''Tumor_Validation_Allele2''Match_Norm_Validation_Allele1''Match_Norm_Validation_Allele2''Verification_Status''Validation_Status''Mutation_Status''Sequencing_Phase''Sequence_Source''Validation_Method''Score''BAM_File''Sequencer''t_ref_count''t_alt_count''n_ref_count''n_alt_count''HGVSc''HGVSp''HGVSp_Short''Transcript_ID''RefSeq''Protein_position''Codons''Hotspot'
```

The format is Mutation Annotation Format (MAF) is also  a tab-delimited text file  with one mutation record observed in one tumor sample.




## 2. In words, describe what the two code blocks below do?

### Answer

``` R
raw.mutations %>%
  filter(Variant_Classification != 'Silent') %>%
  group_by(Hugo_Symbol) %>%
  summarize(n = n_distinct(Tumor_Sample_Barcode)) %>%
  slice_max(order_by = n, n = 10)
```

The first block is to remove silent mutations, group by `Hugo_Symbol` and summarize the number of each unique tumor samples (`Tumor_Sample_Barcode`). It returns the top 10 gene counts.


``` R
raw.mutations %>%
  filter(Variant_Classification != 'Silent') %>%
  inner_join(
    raw.samples %>%
      select(SAMPLE_ID, CANCER_TYPE_DETAILED),
    by = c('Tumor_Sample_Barcode' = 'SAMPLE_ID')
  ) %>%
  group_by(Hugo_Symbol, CANCER_TYPE_DETAILED) %>%
  summarize(n = n_distinct(Tumor_Sample_Barcode), .groups = 'drop') %>%
  group_by(CANCER_TYPE_DETAILED) %>%
  slice_max(order_by = n, n = 1)
```

The second block is to remove silent mutation, join each mutations to the sample's detailed cancer subtypes. It counts how many tumor samples have mutations in each gene within each subtype (`CANCER_TYPE_DETAILED`), summarize the number of each unique tumor samples (`Tumor_Sample_Barcode`), then remove the ungrouping. It returns the most frequent mutated gene for each detailed subtypes.

## 3. Based on this code, how would you preprocess the data to reduce code redundancy? How would you test your preprocessing?

### Answer
Both code blocks have the repeated preprocessing steps, so 
I would like to customize a preprocessing function or an intermediate cleaned table. The preprocessing should be able to exclude slient mutation, standarize `SAMPLE_ID` before any summaries or joins and keep only relavant columns (i.e. `Tumor_Sample_Barcode` , `SAMPLE_ID`). That way, it would make the analysis easier to integrate into a reproducible pipeline and improve testability. We can create a dummy table/data and test it with several edge cases.


## 4. How would you test if the different breast cancer subtypes differ in their TP53 mutation rate?

### Answer

We reduce the data to one row per tumor sample and define a binary question to check if TP53 is mutated. We then use statistical analyses to compare that across breast cancer subtypes based on the counts. Pearson's chi-squared test of independence, t-test or Kruskal–Wallis test were used when appropriate to test association between different variables. If the subtype counts are too small, we can collapse rare cancer subtypes or choose Fisher's exact test instead. Effect sample size, p-value and mutation proportions can be used to decide if the overall comparison is significant. In addition, more specific comparison can be run on different subsets of groups.   For instance, we can take out the silent mutation to compare more functional mutational events. 
[Reference - statistical analysis](https://watermark02.silverchair.com/3569.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAA0gwggNEBgkqhkiG9w0BBwagggM1MIIDMQIBADCCAyoGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQML9zrwYaSIQ79W27vAgEQgIIC--Cd1hrvKu0usDZxw7jbbmT2yWA6g9mWpjszOdU31BjeFMl1PSWYJVpDhBHBTcdt62Bd38IJhzItsvrxu44OORR2gc4p5_eLQSysjtp8LtS1K2zm4hjJBGYTAyZYGPCHy1zOFD4XTnz475O9CCtPnXUov-zrItZSUk5hSGCDRUxcCdbONG4f0hWRwBffoui4zhG4fIwuuQzrB2elBh1M9obT7nYLGN8gNgWqK2PYXMfGcFpwL4bGdfdt4kqDl3BYKeMIWR_0Bjm9BnIoJiCQpFfKnaYqKyCbM_8UYTWPJ61JDHgM_S2ky6SxsiPIGEi9v0XR3bIIc2pmPpuA5xeSgjtZEPUUF-ooDTNjQH84_qdqpmT1sc_6yz3CiToFyri8fWPQM40-e3NLEqYYWHbwGC_HwIROWANQeTbGrRB5OBR0hqsg9Bm89ckagm1ig0XENHCg9sjwdVUhHqaSOaD2UNR1joidYp1-AxEhbP9B9wH3CX2vpvsqYg2MfL9lG4VO6SR-VYmhAn_QyMbwzu4kjbtz5LC1z5qLQ4HaXER1ueAVMPeBShhQCIMsZquNDwGRG4R-qrx6j2Z7K2hyss5LgA0fhHqgEiY8sFSsJR80kqqKd1SfXw7HuKh4JPDNig6_SgjcLgctdhGlbn0jFGJJIKLj20Nz1kFN-pW6-JmxwRAEgE5X9qw6-Jg0DdAzWkfUX6XYeAEZMmRpNCsRSbtK-BxNAPmWhdfP11UEbC2SwdU55cG8MBasnHy2fJy87sIYL993gHKO6Yu1lcr3J4072QHxoJz-1mOcQv7Rm8htAPvo5gTEFAIiOlDmvU-g5Zor_YYRv-yPvlXXdl1qpndmS59qywj7BfZyx260M7bG9PqGYo_iUCVe-_045YeA81yAqrV-xLlcEQWAxjUKGCzfKc5-s5sMI3PZ12tHS6LIuxL1ctDzFFRPPzVulFN_HqnGa9xZKnTtmUJcmW2aeFvVZmNte5l5VJgtH7Ubb2vtTVLDNmI6yqdlTvP2KwA)



## 5. What would you change about this code? Do you see any bugs?

### Answer

- Repeated Preprocessing, which we already mention in the question 3. Preprocessing step can be done once and reproducible.
- Check the missing values before any joins or groupings.
- `inner_join()` would silently drop some unmatched `Tumor_Sample_Barcode` mutation rows from `raw.samples`. I would create another table to store the left-out data in case we need it afterwards.
- Naming can be more clearer. For instance, `n` can be replaced by `sample_counts`.
- The `with_ties` variables can be set to true in `slice_max` to help clarification.
- The second block has too many steps, which makes it hard to read. I would break it down into two or more steps for readbility.
