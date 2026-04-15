# Literature Review

## 1. Introduction

Recent advances in large language models (LLMs) have significantly improved performance in natural language understanding and generation tasks. However, when applied to domain-specific settings such as finance, LLMs often struggle with factual accuracy and hallucination. Retrieval-Augmented Generation (RAG) has emerged as a promising approach to mitigate these issues by incorporating external knowledge sources during inference.

This literature review examines four key areas relevant to this thesis: financial question answering (QA) benchmarks, retrieval-augmented generation, dense retrieval methods, and domain shift in QA systems. These areas collectively motivate the design and evaluation of RAG systems under limited-data financial settings.

---

## 2. Financial Question Answering Benchmarks

Financial QA is a challenging task due to domain-specific terminology, numerical reasoning requirements, and the need for high factual accuracy.

### FinanceBench

FinanceBench is a benchmark designed to evaluate LLM performance on open-book financial QA tasks. It provides curated question-answer pairs along with supporting evidence extracted from financial documents. A key strength of FinanceBench is its emphasis on **grounded reasoning**, where answers must be supported by evidence.

However, the publicly available version of FinanceBench contains only a limited number of annotated examples (~150 samples), making it unsuitable for training or large-scale experimentation. Instead, it is best used as a **high-quality evaluation dataset**.

### FiQA

FiQA (Financial Question Answering) is a larger dataset constructed from community-generated financial questions and answers sourced from platforms such as Reddit and StackExchange. Unlike FinanceBench, FiQA includes both a QA dataset and an associated corpus of financial text, making it suitable for retrieval-based methods.

While FiQA offers scale and diversity, it lacks the strict evidence grounding found in FinanceBench. This creates a contrast between:
- **FiQA**: larger, noisier, suitable for development  
- **FinanceBench**: smaller, cleaner, suitable for evaluation  

### Summary

Existing financial QA datasets exhibit a trade-off between scale and quality. This motivates a cross-dataset evaluation approach, where models are developed on larger datasets and evaluated on high-quality benchmarks.

---

## 3. Retrieval-Augmented Generation (RAG)

Retrieval-Augmented Generation (RAG) integrates external knowledge retrieval into the generation process of LLMs. Instead of relying solely on parametric knowledge stored in model weights, RAG retrieves relevant documents and conditions the generation on these documents.

### RAG Framework

The standard RAG pipeline consists of two main components:
1. A retriever that selects relevant documents given a query  
2. A generator (LLM) that produces answers conditioned on retrieved documents  

In the commonly used **RAG-Sequence** formulation, retrieval is performed once per query, and the retrieved documents are used throughout the entire generation process.

### Advantages

- Reduces hallucination by grounding outputs in external evidence  
- Enables access to up-to-date or domain-specific knowledge  
- Improves interpretability by exposing supporting documents  

### Limitations

Despite its advantages, RAG has several limitations:
- Performance depends heavily on retrieval quality  
- Retrieved documents may be irrelevant or incomplete  
- Models may still hallucinate even when evidence is provided  
- Sensitivity to domain shift between training and evaluation data  

### Relevance to This Work

This thesis focuses on evaluating how RAG performs in financial QA settings, particularly under domain shift and limited-data constraints.

---

## 4. Dense Retrieval Methods

Retrieval is a critical component of RAG systems. Traditional retrieval methods such as TF-IDF and BM25 rely on lexical matching and often fail to capture semantic similarity.

### Dense Passage Retrieval (DPR)

Dense Passage Retrieval (DPR) addresses this limitation by learning dense vector representations for queries and documents using neural encoders.

The architecture consists of:
- A **query encoder** that maps questions into vector representations  
- A **document encoder** that maps documents into the same vector space  

Similarity between a query and a document is computed using vector similarity (e.g., dot product), enabling semantic retrieval beyond keyword matching.

### Practical Implementations

In practice, DPR-like systems are often implemented using pre-trained embedding models such as Sentence-BERT. These embeddings are indexed using efficient similarity search libraries such as FAISS, enabling scalable retrieval.

### Limitations

- Retrieval errors propagate directly to downstream generation  
- Embeddings may not capture domain-specific nuances  
- Performance depends on chunking strategy and indexing design  

### Relevance to This Work

This thesis investigates how retrieval quality affects downstream QA performance and how different retrieval configurations influence RAG effectiveness.

---

## 5. Domain Shift in Question Answering

A key challenge in real-world QA systems is **domain shift**, where the distribution of evaluation data differs from the training or development data.

### Problem Definition

Domain shift occurs when:
- Question styles differ  
- Answer formats vary  
- Underlying document distributions change  

In financial QA, this is particularly relevant due to differences between curated datasets (e.g., FinanceBench) and community-generated data (e.g., FiQA).

### Impact on Models

Domain shift can lead to:
- Decreased accuracy  
- Increased hallucination  
- Reduced reliability of retrieved evidence  

Most existing studies evaluate models within a single dataset, leaving cross-dataset generalization underexplored.

### Relevance to This Work

This thesis explicitly evaluates domain shift by:
- Developing models on FiQA  
- Evaluating performance on FinanceBench  

This setup enables analysis of how well RAG systems generalize across different financial QA distributions.

---

## 6. Research Gap

Despite advances in retrieval-augmented generation and the availability of financial QA benchmarks, there is limited understanding of:

- How RAG systems perform under domain shift in financial QA  
- Whether retrieved evidence reliably supports generated answers  
- How retrieval quality influences hallucination and answer correctness  

---

## 7. Contribution of This Thesis

This thesis addresses the identified gap by:

1. Comparing LLM performance with and without retrieval augmentation  
2. Evaluating cross-dataset generalization (FiQA → FinanceBench)  
3. Analyzing hallucination and evidence grounding in RAG systems  
4. Investigating the impact of retrieval design choices on QA performance  

---

## 8. Conclusion

The literature highlights the potential of RAG systems for improving factual accuracy in domain-specific QA tasks, while also revealing key challenges related to retrieval quality and domain generalization. By combining insights from financial QA benchmarks, retrieval methods, and domain shift analysis, this thesis aims to provide a systematic evaluation of RAG systems in financial contexts.