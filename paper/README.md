# Abstract
Large language models (LLMs) profoundly influence diverse domains, including research and development, journalism, and beyond. This influence raises signifiacnt converns regarding the issues of authencity and the potential for misinformation. 
SemEval-2024 Task 8 aims to address these concerns bz developing a benchmark for detecting human-written text (HWT) vs. machine-generated text (MGT) across various domains. The task involves a binary classification problem where participants are required to classify text as either human-written or machine-generated. 
We participated in Subtask A (Monolingual), which focuses on the binary classification of text in a single language, specifically English. 

We propose **Liberta**, a **hybrid transformer-linguistic model** that integrates deep contextual embeddings from RoBERTa with linguistic features and token-level probability distributions. 
Our approach leverages **readability, stylistic regularity, and predictability**, which captures transformer representations and linguistic features to enhance the model's ability to distinguish between human-written 
and machine-generated text.

We evaluate the official dataset, finding that the integration of linguistic features results in an approximate 2% improvement over a baseline that exclusively utilizes RoBERTa. Moreover, our comprehensive hybrid system achieves an accuracy of 88.64% and an F1 score of 0.890, outperforming both transformer-only models and those enhanced with linguistic features.
These results demonstrate the effectiveness of combining deep contextual embeddings with linguistic features in addressing the challenges of text classification in the context of human-written and machine-generated text detection.

# Introduction 
