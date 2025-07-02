# Response to Reviewer EaXA
We thank the reviewer for the valuable feedback. Below, we provide point-by-point responses to the reviewer’s concerns and suggestions.

> Incremental Contribution: .... Considering that GraphRAG already incurs considerable computational cost during index construction the added topic-aware node and summary updates may further increase the overhead. … I recommend that the authors provide a comprehensive runtime comparison, considering index construction, topic-aware graph updates, and the prediction phase.

As noted in the limitations, graph indexing introduces overhead. Per request, we measured the wall clock time (using two NVIDIA H100 GPUs) for initial and topic-specific graph construction, as well as for inference, with results presented in the table below. While indexing time increases linearly with the number of mined topics, as expected, this cost is incurred only once and is amortized across subsequent queries. Crucially, topic augmentation improves both response quality (Figure 3) and inference efficiency (Table 4). Furthermore, when retrieval augmentation is incorporated into our full model, the inference time is significantly reduced by up to 90.3% on News Articles and 65.4% on Podcast (as shown below). This effectively transforms the impractically slow baseline into a practical system, thereby demonstrating a significant advancement by eliminating this bottleneck.

|                                      | Podcast    | News Articles |
|--------------------------------------|------------|---------------|
| Baseline Graph Indexing Time (sec)   | 5555.32    | 14322.35      |
| Additional Graph Indexing Time with Topic Augmentation (sec) | 178561.94  | 317092.25     |
| The Number of Topics in Topic Augmentation | 38         | 29            |
| Baseline Inference Time at Level 6 (sec) | 20.28      | 77.96         |
| ReTAG Inference Time at Level 6 (sec) | 7.02       | 7.56          |

> A lack of awareness about critical scientific issues Lack of Case Study: It would be helpful to include a detailed case analysis showing the **end-to-end** prediction process for a sample query, which can better illustrate the **effectiveness of the topic-enhanced components**.

We appreciate the reviewer's suggestion for including case studies of individual examples.

As described in Section 3.1.4, GraphRAG uses a single, general graph for the entire corpus. This approach inherently leads to a loss of detailed information in summaries, making it difficult to generate specific answers. In contrast, ReTAG can maintain these details because each graph is dedicated to a single topic.

This is evidenced by the following sample from News Articles, for instance: When answering a question about news coverage on the influence of sports on public health policy, the baseline fails to capture such coverage from the dataset because its general graph summary lacks this detailed information. In contrast, the topic augmentation in ReTAG allows for the capture of the related details within the indexed graph for the 'Sports' topic, which enables the model to generate an answer with these specifics. We will include this valuable discussion and exhibit additional evidencing examples in the final version.

|                                    | Baseline                                                                                                                                                       | ReTAG                                                                                                                         |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Question                           | In what ways do sports events influence public health policies, and how is this reflected in the news coverage across the sports and health categories?         | In what ways do sports events influence public health policies, and how is this reflected in the news coverage across the sports and health categories? |
| Selected Topic                     | X                                                                                                                                                              | Sports                                                                                                                        |
| Community Summary with Related Entities | The World Health Organization and the Centers for Disease Control and Prevention are also key entities in this community, working on issues such as antibiotic resistance and disease tracking. … | The CDC and WHO are collaborating to address the issue of antibiotic-resistant bacteria, which poses a significant **threat to public health**, including in **sports settings** where infections can spread rapidly… |
| Predicted Answer                   | The influence of sports events on public health policies and their reflection in news coverage across sports and health categories is **not directly addressed** in the provided reports… | Sports events **can significantly influence public health policies** in various ways, including promoting physical activity, healthy lifestyles, and disease prevention …         |

> Baseline Clarification: The baselines should be more clearly defined. Is the baseline the original GraphRAG, or a standard RAG model? The paper should also include a comparison with LightGraphRAG (https://arxiv.org/pdf/2410.05779), which offers a more efficient alternative.

Thank you for pointing this out. Our baseline is indeed the original GraphRAG. As requested, we conducted additional experiments with LightGraphRAG using the same LLM and retriever configurations on Podcast. The table below reports the winning rates of the proposed method over LightGraphRAG, as well as compares the inference time of the two methods. Note that a single LightGraphRAG prediction is compared with predictions for each level in ours. While LightGraphRAG is efficient, as the final answer is generated with a single LLM inference discarding contexts exceeding the maximum token length, its final answer quality suffers significantly from the lack of global context necessary for effective global sensemaking. Notably, at Level 1, our method achieves comparable inference time while significantly improving both comprehensiveness and diversity.

Winning rates of ReTAG over LightGraphRAG:
| Level | Comp. | Div. | Emp. | LightGraphRAG | ReTAG |
|-------|-------|------|------|---------------|-------|
|   1   | 66.0  | 61.6 | 49.6 |   1.52        | 1.98  |
|   2   | 88.8  | 85.6 | 76.4 |   1.52        | 7.02  |
|   3   | 86.8  | 82.0 | 70.4 |   1.52        | 7.09  |
|   4   | 86.8  | 82.0 | 74.8 |   1.52        | 7.06  |
|   5   | 88.0  | 82.0 | 68.4 |   1.52        | 7.07  |
|   6   | 92.7  | 84.3 | 68.5 |   1.52        | 7.02  |

> In Table 1, does "w/ TA" imply that topic information is used to update the node representations, thereby reducing the number of nodes and edges? Please clarify.

The term "w/ TA" in Table 1 stands for "with Topic Augmentation." In this model, topic-specific entity-relation graphs are built following Section 3.2.1. Since these graphs only preserve entities and relations related to a given topic, the numbers of nodes and edges are indeed reduced, as the reviewer correctly understood. We will clarify this in the final version.

## Response to the comments about the additional figure and typos.
Thank you for the constructive feedback. As suggested, we will add an additional illustrative figure, rename the dataset, and correct the typos.