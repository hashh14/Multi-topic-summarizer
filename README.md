# Multi-topic-summarizer
The multi-topic summarizer can process PDF files or website URLs to generate concise topic wise summaries. The summarizer begins by extracting the text content from the provided input. For PDFs, the text is read directly from the file, while for website URLs, the textual content is scraped from the webpage, filtering out any non-text elements to focus solely on the relevant information.

The extracted text is preprocessed. This step involves cleaning the text to remove any unnecessary characters, symbols, or formatting issues that may interfere with further processing. After preprocessing, the clean text is tokenized into individual sentences. The tokenized sentences are needed for the embedding and clustering stages.

The tokenized sentences are then embedded using a sentence transformer model. This model converts each sentence into a high-dimensional vector that captures its semantic meaning. These embeddings allow clustering based on their semantic similarities.

KMeans clustering algorithm is applied to these sentence embeddings to group them into clusters. Each cluster represents a subtopic within the overall text, grouping sentences that share similar themes or topics. 

Finally, each cluster of sentences is fed into different summarization models to generate summaries for each subtopic. The models used for this purpose are facebook/bart-large-cnn, google-t5/t5-base, and google/pegasus-cnn_dailymail. The results of these different models are compared.

**Qwen2.5-0.5B-Instruct** fine-tuned with LoRA on 4,000 arXiv papers
([`ccdv/arxiv-summarization`](https://huggingface.co/datasets/ccdv/arxiv-summarization)) is also compared.

- `finetune_qwen_arxiv.ipynb`: data preparation, LoRA fine-tuning, and summary generation for all systems
- `evaluate_summaries.ipynb`: ROUGE and BERTScore evaluation, with bootstrap confidence intervals

Results:
| System | ROUGE-1 | ROUGE-2 | ROUGE-L | BERTScore F1 | Avg words |
|---|---|---|---|---|---|
| BART-large-cnn | 36.6 | 11.4 | 20.1 | 83.8 | 93 |
| T5-base | 28.8 | 7.3 | 16.9 | 82.9 | 77 |
| Pegasus-cnn | 35.6 | 11.2 | 20.2 | 84.1 | 89 |
| Qwen2.5-0.5B zero-shot | 36.5 | 9.6 | 19.7 | 83.2 | 150 |
| **Qwen2.5-0.5B fine-tuned** | **38.6** | **13.0** | **23.3** | **84.7** | 135 |


