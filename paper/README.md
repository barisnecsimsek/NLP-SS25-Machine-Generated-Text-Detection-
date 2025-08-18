# Abstract
With the rapid advancement of large language models (LLMs) capable of generating text highly resembling human writing, reliable detection has became increasingly crucial. SemEval-2024 Task 8 addresses this challenge through binary classification of human-written (HWT) versus machine-generated text (MGT). 
We participated in Subtask A (Monolingual) with Liberta, a hybrid system combining RoBERTa embeddings with linguistic features and token-level probability distributions and leveraging **readability, stylistic regularity, and predictability**. 
On the official dataset, our system achieved 88.64% accuracy and an F1 score of 0.890, outperforming a RoBERTa-only baseline. These results confirm that integrating neural and linguistic features enhances robustness in machine-generated text detection.

# Introduction 

Large language models (LLMs) are having a significant influence on our daily tasks across a wide range of domains, from education and reasearch to content creation and communication. Since their impact on society is not minor but rather deeply shaping how we live and work, the potential risks associated with them are equally substantial. These include issues such as hallucination, or misuse of generated content for monetization, misinformation, and academic dishonesty. Machine-generated text (MGT) detection help mitigate such risks. 
Human and machines can produce text in different style subtly differing in lingustic and statiscal cues. By detecting these differences help distinguish human-written text and machine-generated text. Traditional detection methods rely on simple statistical patterns, which are insufficient to capture complex output of modern LLM, which closely mimics human writing style and structure (https://arxiv.org/html/2411.06248v1).
To address this challenge, we introduce **Liberta**, our system for SemEval-2024 Task 8 Subtask A (Monolingual). Liberta is a hybrid architecture that combines: 
1. RoBERTa embeddings
2. Readability and stylistic features, such as Flesch Reading Ease, model verb density, punctuation ratios, and rhetorical markers.
3. Token-level probablistic features from GPT-2
This paper presents our methodology, experiments, adn findings. In §2, we review related work and elaborate our prior research on machine-generated text detection. §3 delves into our system design and hydrid methodology. In §4, we describe the experiment alongside the dataset. §5 demonstrate the results of our experiements, comparing baslines with our proposed model. Finally, we close with a disscussion of the broader implication of our findings, along with limitation and directions for future work. 


