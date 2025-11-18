---
title: 'On Text Simplification Metrics and General-purpose LLMs for Accessible Health Information, and A Potential Architectural Advantage of The Instruction-tuned LLM Class'
tags: 
    - NLP 
    - Text simplification
    - Metrics correlation
    - LLMs  
date: Nov 2025 
---
# Summary


<p align="center">
  <img src="02-pvalz-summary.png" alt="Correlogram for readability metrics" width="720">
  <br>
  <em>Figure 1: Summary of mean scores</em>
</p>



**[Publication associated with this entry](https://arxiv.org/abs/2511.05080)**

*Title:* On Text Simplification Metrics and General-Purpose LLMs for Accessible Health Information, and A Potential Architectural 

*How to cite:*

```bibtex
@article{githinji2025textsimplificationmetricsgeneralpurpose,
      title={On Text Simplification Metrics and General-Purpose LLMs for Accessible Health Information, and A Potential Architectural Advantage of The Instruction-Tuned LLM class}, 
      author={P. Bilha Githinji and Aikaterini Meilliou and Peiwu Qin},
      year={2025},
      eprint={2511.05080},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2511.05080}, 
}
```



# Records
This GitHub repository hosts the analytical outputs from our study. Table 1 below lists the collection of CSV files. These data files support the findings discussed in our research and provide resources for reference. 

**Table 1: Overview of the CSVs with analysis output**
| File name                      | Description                             |   $n$ rows | Columns                                                                                                                                          |
|:-------------------------------|:----------------------------------------|:-----------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------|
| human_benchmark_thresholds.csv | Mean scores for human benchmark.        |               22 | Model Name, Metric Name, mean, se, ci (.95)                                                                                                      |
| welch_tests.csv                | Welch's t-test results                  |              109 | Model Name, Metric Name, Benchmark mean ($\mu_1$), Model mean ($\mu_2$), Difference between means ($\mu_1 - \mu_2$), p value, Benchmark $n$ obs, Model $n$ obs |
| metric_means_se.csv            | Means scores for each of the LLM models |              109 | Model Name, Metric Name, mean, se, ci (.95)                                                                                                      |
| reg_pca.csv                    | OLS regression results                  |               12 | Model, Metric Name, Adj. R-squared:, R-squared:, F-statistic:, No. Observations:                                                                 |
 


**Table 2: SARI Scores**
| Model Name       |   mean |   se | ci (.95)      |   $n$ Documents |
|:-----------------|-------:|-----:|:--------------|--------------:|
| Mistral - flexi  |  42.46 | 0.3  | 41.86 - 43.05 |           606 |
| Mistral - strict |  42.37 | 0.3  | 41.77 - 42.96 |           606 |
| QWen - flexi     |  38.38 | 0.05 | 38.28 - 38.47 |           569 |
| QWen - strict    |  37.84 | 0.35 | 37.16 - 38.52 |           443 |



**Table 3: Number of samples used in the analysis**
| Dataset                  | Simplification Model   | $n$ Documents   |   Task Completion Rate | $n$ Evaluations   |
|:-------------------------|:-----------------------|:--------------|-----------------------:|:----------------|
| Benchmark                | human                  | 748           |                   1    | 26,926          |
| Benchmark as control set | Mistral - flexi        | 606           |                   0.81 | 15,322          |
| Benchmark as control set | Mistral - strict       | 606           |                   0.81 | 15,288          |
| Benchmark as control set | QWen - flexi           | 569           |                   0.76 | 13,533          |
| Benchmark as control set | QWen - strict          | 443           |                   0.59 | 11,182          |
| Custom set               | Mistral - flexi        | 3,218         |                   0.85 | 78,030          |
| Custom set               | Mistral - strict       | 3,217         |                   0.85 | 77,994          |
| Custom set               | QWen - flexi           | 3,672         |                   0.97 | 69,336          |
| Custom set               | QWen - strict          | 2,453         |                   0.65 | 59,340          |


 

## Metrics in the analysis 
| Metric     |                                | Computation notes                                                                                                                              |
|-------------------------------------------|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| n words                                  |                                | $\mathbf{W}$                                                                                                                                   |
| n sentences                              |                                | $\mathbf{S}$                                                                                                                                   |
| n syllables in word                    |                                | $\mathbf{P}$  ;   Typically polysyllabic if $\mathbf{P}\ge 3$                                                                                 |
| avg words per sent                     |                                | $\mathbf{L} = \frac{ \mathbf{W} }{ \mathbf{S} }$                                                                                               |
| difficult words                          |                                | $\mathbf{V} = \frac{ \sum \mathbb{I}( ( w \notin \mathrm{DaleChallList}) |  (\mathbf{P} \ge 3))  }{ \mathbf{W} }  * 100$                     |
| sentences comp ratio                    | Compression or expansion ratio | $\frac{\mathbf{S} {\mathrm{simplified}}}{\mathbf{S} {\mathrm{source}}}$                                                                        |
| words comp ratio                        | Compression or expansion ratio | $\frac{\mathbf{W} {\mathrm{simplified}}}{\mathbf{W} {\mathrm{source}}}$                                                                        |
| vocab match                              | Terms (lemmatized)             | $\mathrm{Jaccard} ( \mathbf{T} {\mathrm{simplified}} ,   \mathbf{T} {\mathrm{source}} )$                                                       |
| Toxicity                                  | Content safety                 | Roberta-hate-speech-dynabench-r4                                                                                                               |
|**2. Discourse fidelity/Accuracy**|  | | 
| Semantic Similarity                       | QWen2.5 32B Embeddings         | $\cos ( \mathbf{Doc} {\mathrm{simplified}}, \mathbf{Doc} {\mathrm{source}} )$                                                                  |
| BERTScore~\cite{zhangbertscore2020}       | N-gram-based                   | F1 score value. (Roberta Large)                                                                                                                |
| ROUGE-L~\cite{linrouge2004}               | N-gram-based                   | Longest common subsequence. With stemming.                                                                                                     |
| SacreBLEU~\cite{postcall2018}             | N-gram-based                   | Defaults                                                                                                                                       |
| LDATopics                                 | Terms                          | $\mathrm{Jaccard} ( \mathbf{T} {\mathrm{simplified}} ,   \mathbf{T} {\mathrm{source}} )$                                                       |
|**3. Simplification and readability**| | | 
| SARI~\cite{xuoptimizing2016}              | System goodness, n-gram based  | $\frac{ \mathbf{F1} {add}  + \mathbf{F1} {keep} + \mathbf{Pr} {del} } { 3 }$ ;  $\mathbf{F1}$ score, $\mathbf{Pr}$ecision score               |
| SMOG~\cite{smog}                          | USA School Grade               | $1.0430  *   \sqrt{  (\sum {w} \mathbb{I}(\mathbf{P} \ge 3)  * \frac{30}{\mathbf{S}}   }  )  + 3.1291$                                       |
| Gunning Fog~\cite{gunningtechniquenodate} | USA School Grade               | $0.4 * (  \mathbf{V}   +    \mathbf{L}  )$                                                                                                     |
| ARI~\cite{smithautomated1967}             | USA School Grade               | $(4.71 * \frac{\mathrm{nCharacters}}{\mathbf{W}}) +  (0.5 * \mathbf{L})  - 21.43$                                                             |
| Dale-Chall                                | USA School Grade               | $(0.1579 * \frac{  \sum \mathbb{I}( w \notin \mathrm{DaleChallList})    }{\mathbf{W}} * 100)   +  (0.0496 * \mathbf{L})  \space [+ 3.6365 ]$ |
| FKGL~\cite{kincaidelectronic1988}         | USA School Grade               | $-15.59  +  (11.8 * \frac{\sum {w}(\mathbf{P})}{\mathbf{W}})    + (0.39 *  \mathbf{L})$                                                     |
| Flesch Ease~\cite{klareautomation1969}    |                                | $206.835 - (84.6 * \frac{\sum {w}(\mathbf{P})}{\mathbf{W}} )   - (1.015 *  \mathbf{L} )$                                                     |
| New Dale-Chall                           | For context                    | $(0.1579 * \mathbf{V})   +  (0.0496 * \mathbf{L})  \space [+ 3.6365 ] $                                                                       |



## Additional plots

<p align="center">
  <img src="08b-correlo-readability.png" alt="Correlogram for readability metrics" width="720">
  <br>
  <em>Figure 2: Correlation pair plots for readability formulas</em>
</p>



<p align="center">
  <img src="08b-correlo-accuracy.png" alt="Correlogram for accuracy metrics" width="720">
  <br>
  <em>Figure 3: Correlation pair plots for accuracy metrics</em>
</p>

<p align="center">
  <img src="08b-correlo-distribz.png" alt="Correlogram for foundational metrics" width="720">
  <br>
  <em>Figure 4: Correlation pair plots for other metrics</em>
</p>