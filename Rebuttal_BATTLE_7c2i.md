Dear Reviewer,

Thank you very much for your constructive comments and suggestions on our manuscript titled "BATTLE: Unsupervised Bi-Level Optimization with Implicit Relationship Mining for Attributed Graph Anomaly Detection" with Submission ID 732. We have carefully considered each point and have made significant revisions to address your concerns. Below, we provide a detailed response to your questions.

**Question 1 Respond:**

Thank you for your question. Below, I will elaborate on the necessity of incorporating implicit relationship mining into anomaly detection:

1. **Providing a New Perspective**: Implicit relationship mining offers a new relational-level perspective for anomaly detection. In real-world attributed networks, there are not only explicit relationships (i.e., direct connections) but also implicit relationships (i.e., potential, indirect connections). These implicit relationships can reflect the underlying reasons for entities establishing explicit connections, which is crucial for understanding complex interactions within networks and identifying anomalous patterns.

2. **Revealing Underlying Motivations**: Implicit relationships can reveal the underlying motivations for entities to form connections. For instance, in social networks, two users might connect due to shared interests or mutual friendships; these implicit relationships are essential for differentiating the behavior patterns of normal users from malicious users, such as spammers or fraudsters.

3. **Enhancing Detection Accuracy**: By considering implicit relationships, anomaly detection models can more accurately capture anomalous patterns within networks. Many anomalous patterns, such as the spread of false information, are often created by malicious users lacking the genuine motivations and implicit connections that accompany legitimate interactions. Implicit relationship mining helps identify these anomalous patterns, thereby enhancing the accuracy of anomaly detection.

4. **Strengthening Model Robustness**: Implicit relationship mining allows models to better understand the complex interplay between network structure and node attributes, which helps improve the model's robustness in the face of various network attacks and anomalous situations.

5. **Compensating for the Insufficiency of Explicit Relationships**: In some cases, relying solely on explicit relationships may not fully capture all the important information within a network. Implicit relationship mining can supplement the information not covered by explicit relationships, providing a more comprehensive network representation, which is particularly important for anomaly detection.

6. **Addressing Real-World Challenges**: Real-world networks often contain complex and diverse implicit relationships that are not limited to specific types of implicit relationships (such as emotional relationships, advisor-advisee relationships, etc.). Designing a versatile method capable of mining various types of implicit relationships is key to solving the problem of anomaly detection in attributed networks.

**Question 2 Respond:** 

Thanks for your question. The primary contribution of BATTLE lies in incorporating implicit relationship into anomaly detection. Regarding the complexity of BATTLE, which comes mainly from the attention network learner, sparsification, graph convolutional network layer, contrastive loss, and implicit relationship prediction. We calculated the complexity of BATTLE as 

$$
O\left(nd(L_1 + b_1) + md_1L + nd_2^1L + nd_2^2L + m^2 + (2 + R)n^2\right)
$$

where \( n(m) \) is the number of nodes(edges), \( b_1 \) is the batch size of attention network. \( d/d_1/d_2 \) are the dimension of node attribute/representation/projection, \( L_1/L \) is the layer number of attention network and GCN, and \( R \) is the number of graph samples. Compared to ANEMONE (with complexity 

$$
O\left(md_1L + nd_2^1L + nd_2^2L + 6n^2\right)
$$

), CoLA (with complexity 

$$
O\left(md_1L + nd_2^1L + nd_2^2L + 4n^2 + c^2nR\right)
$$

), and Sub-CR (with complexity 

$$
O\left(md_1L + nd_2^1L + nd_2^2L + 5n^2 + n^2R\right)
$$

), our method does not have significantly higher complexity (all four methods are in \( O(n^2) \) order of magnitude), yet we achieve notable performance improvement. We will present the computational details of model complexity in the final version. Additionally, we conducted experiments comparing BATTLE with other baselines regarding computational efficiency. As shown in Table 1, the experimental results demonstrate that BATTLE does not significantly increase training time compared to several other methods. However, we achieve notable performance improvement.

**Table 1:** Training time for different methods

| Methods       | BlogCatalog | Flickr    |
| ------------- | ----------- | --------- |
| ANEMONE(2021) | 10.07 min   | 22.40 min |
| CoLA(2022)    | 9.35 min    | 20.60 min |
| Sub-CR(2022)  | 10.90 min   | 23.62 min |
| BATTLE(Ours)  | 11.71 min   | 25.04 min |

Besides, validation on large-scale graphs is very important, and we have considered conducting such validations. However, our computational resources are limited and do not currently support the verification work required for large-scale graphs. While large graphs and the smaller graphs in our current datasets do not have any fundamental structural differences, large graphs do require more computational resources to complete experiments. Therefore, we believe that under conditions of sufficient computational resources, our BATTLE framework should also perform well on large graphs.

Your inquiry is very helpful to our work, and we will definitely take this into account in future studies, attempting to include validations for large-scale graphs.

**Question 3 Respond:**

Thank you for your question. Indeed, incorporating implicit relationships does alter the topology of the graph, but as demonstrated in our experiments, such changes lead to improved outcomes.

Our implicit relationship graph adds auxiliary edges to the original graph, which only contains explicit relationships, to represent the hidden, potential semantic relationships between entities. In other words, implicit relationships serve as auxiliary information, and we ultimately combine explicit and implicit relationships. In the experiments of Section 4.4, we designed three variants of BATTLE to compare their performance in anomaly detection tasks:

1. **BATTLE-Origin**: Uses only the original graph (i.e., explicit relationships) for anomaly detection.
2. **BATTLE-Mixed**: Uses both the original graph and the implicit relationship graph for anomaly detection.
3. **BATTLE-Implicit**: Uses only the implicit relationship graph for anomaly detection.

The results show that compared to BATTLE-Origin, BATTLE-Mixed improved the Precision@50 metric by 5.2% and 3.2% on two datasets, while BATTLE-Implicit improved the Precision@50 metric by 10% and 7.4% on the same two datasets. These results substantiate the effectiveness of constructing implicit edges. In other words, in response to the potential incoherent local structures or noise you mentioned, our BATTLE framework is indeed robust, as the inclusion of implicit relationships outperforms the original explicit relationships alone.

**Question 4 Respond:**

Below is a detailed response to the questions you've raised:

**Differences and Comparisons between BATTLE-Origin, BATTLE-Mixed, and BATTLE-Implicit**

1. **BATTLE-Origin**:
   - This variant uses only the original graph (i.e., explicit relationships only) for anomaly detection. It does not consider implicit relationships and relies solely on the direct connections in the network and node attributes.

2. **BATTLE-Mixed**:
   - This variant combines the original graph with the implicit relationship graph for anomaly detection. This approach aims to enhance the performance of anomaly detection by leveraging both explicit and implicit relationships.

3. **BATTLE-Implicit**:
   - This variant uses only the implicit relationship graph for anomaly detection. It entirely depends on the implicit relationship network obtained through graph contrastive learning and implicit relationship mining.

**Reasons for BATTLE-Mixed's Inferior Performance**

- **Complexity of Information Integration**: BATTLE-Mixed attempts to utilize both explicit and implicit relationships simultaneously, which may increase the model's complexity. If the integration of these two types of information is not well-coordinated, it could lead to a decrease in performance as the model might struggle to learn and utilize these different types of relationships effectively.

- **Weight Allocation Issues**: When combining explicit and implicit relationships, how to balance their contributions is a critical issue. If the model does not effectively allocate weights, it could result in the loss of important information or an increase in noise.

**Discussion on BATTLE-Joint and BATTLE-Alternative**

1. **BATTLE-Joint**:
   - This variant is trained using traditional joint optimization methods, which aim to optimize two complementary modules (implicit relationship mining and anomaly detection) simultaneously. However, this approach may not effectively capture the complex dependency between the two tasks, leading to suboptimal performance.

2. **BATTLE-Alternative**:
   - This variant is trained using a simpler alternating optimization strategy, where learning occurs at static positions between two tasks. This method may ignore the dynamic coupling relationships between tasks, thereby affecting performance.



We believe that these revisions have significantly improved the quality and clarity of our paper. We are confident that BATTLE provides a valuable contribution to the field and is worthy of presentation at WWW25.

Thank you once again for your thorough review and for giving us the opportunity to improve our work. We look forward to the possibility of presenting our research at WWW25.