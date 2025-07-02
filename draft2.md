# Response to  Reviewer mpzG

We sincerely thank the reviewer for the valuable and detailed feedback. Below, we address each concern raised in the review.

> The author uses the winning rate to judge whether the proposed method has better performance on two datasets. However, such evaluation also requires reporting the p-value for verifying the statistical significance of the winning rate.

We appreciate the reviewer's comment on this important point. As requested, we've verified the statistical significance of our comparisons against the baseline. Due to the high cost of GPT-4o, we used GPT-4o-mini, which is widely accepted to provide consistent assessments with GPT-4o [1, 2]. The table below presents the assessments for Podcast, with all p-values below 0.05, indicating strong statistical significance (over 95% confidence). We observed similar trends for News Articles and will include statistical significance analyses for all other experiments in our final version.
|       |       | Comp. |       |       | Div. |       |       | Emp. |       |
|-------|------------|---------------|---------------|-----------|--------------|--------------|-----------|--------------|--------------|
| **Level** | **Mean** | **Z-value** | **p-value** | **Mean** | **Z-value** | **p-value** | **Mean** | **Z-value** | **p-value** |
|   1   |   74.08    |    -5.82      | 2.87e-08      |  57.92    |   -2.17      | 0.0299       |  64.96    |   -3.64      | 5.36e-04     |
|   2   |   87.92    |    -9.40      | 1.01e-19      |  74.16    |   -7.18      | 7.41e-12     |  81.92    |   -8.19      | 3.66e-15     |
|   3   |   81.92    |    -8.24      | 2.52e-15      |  72.32    |   -6.85      | 6.14e-11     |  77.44    |   -7.21      | 6.65e-12     |
|   4   |   82.00    |    -8.35      | 1.09e-15      |  72.88    |   -6.58      | 2.89e-10     |  75.56    |   -6.86      | 6.14e-11     |
|   5   |   81.60    |    -7.97      | 2.04e-14      |  68.48    |   -5.53      | 9.55e-08     |  72.48    |   -5.65      | 6.34e-08     |
|   6   |   84.68    |    -8.59      | 1.47e-16      |  73.31    |   -6.60      | 2.85e-10     |  77.26    |   -6.98      | 2.88e-11     |


* Mean is the average of the ReTAG’s winning rates.

[1] Belem, Catarina G., et al. "From single to multi: How llms hallucinate in multi-document summarization." *NAACL Findings* (2025).

[2] Guo, Zirui, et al. "Lightrag: Simple and fast retrieval-augmented generation."  (2024).

> In Table 4, the inference time on the Podcast dataset has less improvement than on the News Articles dataset. It is better to have more explanation on this phenomenon in Section 4.2.

Compared to News Articles, Podcast covers a relatively narrow domain. As a result, the mined topics present different characteristics, as shown in Appendix Table 7. Specifically, the topics in Podcast significantly overlap with each other, leading to less effective content filtering and consequently, smaller improvements in inference time. In contrast, topics in News Articles more disjointly cover the broad domain of the dataset, allowing for more effective content filtering. Consequently, indexed graphs across topics in News Articles effectively filter out unrelated concepts, resulting in reduced graph sizes and ultimately leading to higher efficiency improvements. We will include this analysis in the final version.

> The author proposed to use "keyword expansion" to improve the RAG system. However, I found neither details of it in the paper nor a prompt in the appendix. In the appendix, I found a section repeated twice, "Question Keyword Extraction Prompt". Is "extraction" a typo for "expansion"? 

Thank you for pointing out the typo. The term in the appendix should indeed be "expansion," not "extraction." In practice, we prompt an LLM to generate keywords related to the query. To ensure these keywords are relevant to the target corpus, we embed a detailed global description of the corpus, which is generated as outlined in Section 3.2.1. These generated keywords are then concatenated with the original query to enhance the BM25-based retrieval process.

> In the "Method" section, the authors used more than one page to introduce GraphRAG and its limitations at the beginning. This is for sure important for the intuition of the proposed method. However, I consider it is better to have a shorter subsection for introducing GraphRAGThe limitation of GraphRAG can be moved to the introduction.

We appreciate your thoughtful suggestion and agree that reorganizing the "Method" section will allow us to better focus on our proposed approach. We'll implement this change in the camera-ready version, also making full use of the additional page.