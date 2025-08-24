# Abstract
With the rapid advancement of large language models (LLMs) capable of generating text highly resembling human writing, reliable detection has became increasingly crucial. SemEval-2024 Task 8 addresses this challenge through binary classification of human-written (HWT) versus machine-generated text (MGT). 
We participated in Subtask A (Monolingual) with Liberta, a hybrid system combining RoBERTa embeddings with linguistic features and token-level probability distributions and leveraging **readability, stylistic regularity, and predictability**. 
On the official dataset, our system achieved 88.64% accuracy and an F1 score of 0.890, outperforming a RoBERTa-only baseline. These results confirm that integrating neural and linguistic features enhances robustness in machine-generated text detection.

# 1 Introduction 

Large language models (LLMs) are having a significant influence on our daily tasks across a wide range of domains, from education and research to content creation and communication. Since their impact on society is not minor but rather deeply shaping how we live and work, the potential risks associated with them are equally substantial. These include issues such as hallucination, or misuse of generated content for monetization, misinformation, and academic dishonesty. Machine-generated text (MGT) detection help mitigate such risks. 
Human and machines can produce text in different style subtly differing in linguistic and statistical cues. By detecting these differences help distinguish human-written text and machine-generated text. Traditional detection methods rely on simple statistical patterns, which are insufficient to capture complex output of modern LLM, which closely mimics human writing style and structure (https://arxiv.org/html/2411.06248v1).
To address this challenge, we introduce **Liberta**, our system for SemEval-2024 Task 8 Subtask A (Monolingual). Liberta is a hybrid architecture that combines: 
1. RoBERTa embeddings
2. Readability and stylistic features, such as Flesch Reading Ease, model verb density, punctuation ratios, and rhetorical markers.
3. Token-level probabilistic features from GPT-2

This paper presents our methodology, experiments, and findings. In §2, we review related work and elaborate our prior research on machine-generated text detection. §3 delves into our system design and hydrid methodology. In §4, we describe the experiment alongside the dataset. §5 demonstrate the results of our experiments, comparing baselines with our proposed model. Finally, we close with a discussion of the broader implication of our findings, along with limitation and directions for future work.
# 2 Related Work/Background

# 3 Methodology/System Overview 
## 3.1 Exploratory Data Analysis and Feature Extraction
We began by systematically analyzing the properties of the data where human-written and machine-generated text differ, using exploratory data analysis (EDA) as a foundation to uncover linguistic and stylistic patterns. 
Our EDA covered text length distribution, sentence length, unique word counts, lexical diversity, word frequency analysis, POS-tag ratios, and sentence structure, including punctuation use and overall sentence complexity.

A central aspect of our analysis was readability. As shown by [Richter et al., 2023](#references), machine-generated texts tend to have lower Flesch Reading Ease scores, indicating that they are objectively more difficult to read.
However, participants often rate these texts as easier to understand, a discrepancy can be attributed to predictive fluency, which suggests that AI texts align with the brain's natural ability to anticipate upcoming words. 

Motivated by prior work, we extended standard readability metrics with additional custom modules:

- A readability and complexity module extended these analyses. 
- A content and persuasive structure module extracting argument indicators, persuasive word density, modal verbs, pronouns, emphasis markers, and punctuation-based features. 

In addition, we incorporated features reflecting grammatical error rates, stylistic repetition (n-gram entropy, bigram frequency), and sentence balance, which further capture the "cleanliness" and formulaic regularity of AI text compared to the variability of human writing. 
By integrating readability, stylistic, and rhetorical features, our feature extraction pipeline provided measurable and reliable indicators to differentiate machine-generated from human-written text. 

# 4 Experiments 
## 4.1 Dataset 
We used the SemEval-2024 Task 8 dataset, which constitute of 56,000 machine-generated texts and 63,000 human-written texts. The training set includes five domains: Wikipedia, WikiHow, Reddit, arXiv, and PeerRead, while the test set utilizes the OUTFOX domain. 

The machine-generated texts were produced by various large language models (LLMs), including GPT-3, GPT-3.5, ChatGPT, BLOOMZ, Cohere, Dolly-v2, and GPT-4.

To gain a deeper understanding of the dataset, we also analyzed the perplexity scores. As illustrated in Figure (see image poster for dataset), human-written texts displayed a significantly higher average perplexity of 88.72, compared to the mean of 53.10 for machine-generated texts. This difference indicates that human writing is generally more irregular and unpredictable, while AI-generated text tends to be more statistically probable and fluent (Richter et al.).
## 4.x Data Augmentation
During model training, we aim to improve the accuracy of machine-generated text detection through data augmentation. Overfitting is a common phenomenon, where the model achieves high accuracy on the training data but performs poorly on unseen test data. A simple yet effective approach to mitigate this issue is to enlarge the training dataset, thereby optimizing the final model by replacing or extending the original training samples.

To enhance the diversity of the training data, we applied data augmentation exclusively to human-written samples (label = 0) in the training set. The objective was to introduce linguistic variation into the human class, thereby reducing overfitting and improving the model’s ability to generalize in distinguishing human-written from machine-generated text.

For this purpose, we employed the **`ibm-research/qcpg-sentences`** paraphrasing model, a pre-trained sequence-to-sequence model designed for sentence-level rewriting. All human-written texts were first segmented into sentences using punctuation and line breaks as delimiters. We then compiled a set of unique sentences and paraphrased them in batches of 256 using the model on GPU. In total, the augmentation process produced a large collection of paraphrased human-written samples. This procedure effectively expanded the training set and increased the variability of human text without altering the machine-generated samples, thereby balancing the data in a way that favors improved generalization of the detection model.

In the preliminary evaluation using only the baseline RoBERTa model, the accuracy improved from 65.14% with the original training data to 68.10% after applying data augmentation. However, when the paraphrased sentences were incorporated into the dataset by replacing the original training samples and applied to the hybrid model, no improvement in accuracy was observed; the performance remained at only 75%. This indicates that simply enlarging the dataset does not necessarily lead to better model performance. A possible explanation is that the paraphrased sentences did not reinforce or enrich the discriminative features available to the model and may even have blurred the boundary between human-written and machine-generated text, since paraphrasing itself can be considered a form of assisted generation.

Therefore, we did not retain data augmentation in the final implementation. Nevertheless, this finding suggests that in future stages, data augmentation may be more effective if it relies on more “manually collected” data resources, ensuring that the distinctive features of human-written text are preserved, and by selecting augmentation strategies tailored to different model architectures.
# 5 Results 

# 6 Limitations and Future Work
Our work in the SemEval Task 8 Subtask A mainly focused on the detection of monolingual English, integrating transformer embeddings with readability, grammatical, and rhetorical features. We showed that hybrid systems can achieve effective performance in machine-generated detection. 

However, a significant limitation of our approach is its reliance on token-level probabilistic signals, which are not universally available across model providers. Many closed-source LLMs do not expose per-token log-probabilities or full next-token distributions. Vendors often restrict these outputs to mitigate risks of model extraction, distillation, or prompt inversion. This restricts our ability to compute key probabilistic features, reducing the generalizability of our detector in real-world deployments. Consequently, our results may overestimate performance when only models that expose probabilities are evaluated.

Even when token probabilities are available, probabilistic features are susceptible to straightforward evasion. Small textual modifications such as adjustments, paraphrases, synonym substitutions, or reordering can shift token likelihoods and entropy without altering the underlying semantics. Such edits can move a sequence from a “high-confidence, low-entropy” toward distributions that resemble human-written text, thereby lowering classifier reliability.

A key limitation of our approach is its reliance on formulas specific to the English language, which limits their cross-lingual applicability.

Future studies should therefore explore the design of multilingual linguistic features. [Ho and Chan, 2023](#references) indicates that cross-lingual transferability depends on linguistic similarity, context, and resource availability, with multilingual models like mBERT performing better on high-resource languages.

As suggested by [Peters et al., 2009](#references) language-specific readability indicies (e.g., LIX, Amstad) could be integrated with language-agnostic features such as sentence length variance, punctuation density, or lexical entropy. A promising direction is to combine cross-lingual embeddings (e.g., XLM-R, mBERT) with symbolic features, and to replace language-specific grammar tools with perplexity-based proxies capable of generalizing robustly across languages. 

Furthermore, rhetorical elements may be extended across languages by mapping language-independent discourse categories ([Sharma & Agrawal, 2019](#references)). Discourse conventions differ across lingustic and cultural contexts. 


## References

Richter, J., Haugen, J., Paparoidami, V., & Reber, R. (2023).  
*Persuasive messages written by generative AI are easier to read, liked better, and perceived as more probably true than messages written by humans.*  
University of Oslo & Norwegian Institute of Public Health.  
[https://doi.org/10.31234/osf.io/3t8fn](https://doi.org/10.31234/osf.io/3t8fn)


Proceedings of the International AAAI Conference on Web and Social Media, 17(1), 652–663.  
[https://doi.org/10.1609/icwsm.v17i1.22121](https://doi.org/10.1609/icwsm.v17i1.22121)  

Peters, C., Clough, P., Gey, F., Karlgren, J., Magnini, B., Oard, D. W., … & Womser-Hacker, C. (2009).  
*Experimental IR Meets Multilinguality, Multimodality, and Interaction: Proceedings of the CLEF 2009 Workshop.*  
Lecture Notes in Computer Science, vol 6241. Springer.  
[https://doi.org/10.1007/978-3-642-15751-6](https://doi.org/10.1007/978-3-642-15751-6)  



Sharma, D., & Agrawal, A. (2019).  
*Intelligent Human Computer Interaction.*  
Proceedings of the 11th International Conference on Intelligent Human Computer Interaction (IHCI 2019).  
Springer.  
[https://doi.org/10.1007/978-3-030-50936-1](https://doi.org/10.1007/978-3-030-50936-1)  






we included both linguistic and statistical features. which is