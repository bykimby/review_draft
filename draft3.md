# Response to  Reviewer 528S

We truly appreciate the reviewer’s thoughtful feedback. We carefully reviewed the reviewer’s comments and provided detailed responses below to address the concerns regarding topic granularity, keyword expansion distinctions, and evaluation validity. 

> There is no discussion on whether the granularity of the mined topics is appropriate, including whether there are any subsumption relationships among topics.

We provide a related analysis in Appendix B on topic coverage, and through qualitative analysis, we found that the mined topics generally reflect the diverse and domain-specific nature of the corpus. While it is difficult to quantitatively assess the appropriateness of topic granularity, the improved performance of topic augmentation over the baseline serves as indirect evidence of its effectiveness.

Building on this, we also appreciate the reviewer's constructive feedback regarding subsumption relationships across topics. We do observe some such relationships (e.g., between "computer science" and "machine learning"). However, we qualitatively found it challenging to define a strict hierarchy or consistently prefer more general topics, as both broader and narrower topics can be helpful depending on the specific query. Consequently, we chose not to prune topics solely based on coverage. While we acknowledge that the mined topics could be further optimized, potentially by exploring these hierarchical relationships, we're deferring this for future work.

> Furthermore, the paper does not evaluate whether the topic selected by the proposed method for a given query was actually appropriate.

We agree on the importance of evaluating the appropriateness of selected topics. To assess this, we conducted a human evaluation in which annotators rated the appropriateness of the selected topics for all questions in the News Articles dataset using a 5-point scale. The results yielded an average score of 4.76, indicating that the selected topics were generally appropriate. We will incorporate this into the final version.

> Keyword Expansion —is similar to Query Expansion in information retrieval, but the paper does not clarify the essential differences between the two.

The key difference lies in **domain-awareness**. Traditional query expansion methods suggest keywords based solely on an LLM's internal knowledge. In contrast, our approach extracts keywords **grounded in the target corpus** for global sensemaking by incorporating a detailed corpus description (generated as in Section 3.2.1) as domain-specific context alongside the user query.

> All evaluations rely solely on LLM-based assessments, without any discussion on the validity of this evaluation approach.

Following prior work (Edge et al., 2025; Zheng et al., 2024; RAGAS; Es et al., 2023), we employed LLM-based evaluation, which is now a common practice for complex tasks. To further validate it, we conducted a blind human evaluation comparing our model's predictions at Level 7 with the baseline on 50 randomly selected News Article samples. Similar to the LLM evaluator, human annotators were asked to assess comprehensiveness, diversity, and empowerment using the same scales. The table below presents the results, demonstrating consistent trends between human and LLM evaluation scores.

|        | Human Eval | LLM Eval |
|--------|------------|----------|
| Comp.  |    79      |   82     |
| Div.   |    77      |   81     |
| Emp.   |    75      |   71     |