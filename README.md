# LLM Fine-Tuning Evaluation: Task vs. Domain Similarity

## Overview
This project investigates whether task similarity or domain similarity is a better predictor of LLM performance after fine-tuning. Using a controlled experiment with three distinct pre-trained models—Gemma-2B-it, Phi-2, and Mistral-7B we fine-tuned each on a text-to-SQL generation task and evaluated their performance using LLM-based scoring.


## Motivation
Training LLMs from scratch is a high energy and compute intensive venture. Fine-tuning is a more efficient alternative, but selecting which base model to fine-tune is not straightforward. This project aims to develop a principled way to choose models for fine-tuning based on their task and domain alignment with the target task, thereby reducing experimentation overhead and energy cost.

## Set Up

### Models Used
| Model Name   | Parameter Count | Instruction Tuned? | Notes                                                  |
|--------------|------------------|---------------------|---------------------------------------------------------|
| Gemma-2b-it  | 2 Billion         | Yes              | Very effective at question answering                   |
| Phi-2        | 2.7 Billion       | No               | Proficient at reasoning, coding, and math              |
| Mistral-7b   | 7.3 Billion       | No               | Strong with structured reasoning and code generation   |

### Dataset
Dataset: clinton/text-to-sql-v1

Subset Used:
* 20,000 samples for training
* 4,000 for testing
* 1,000 for validation

### Fine-Tuning Variables
* LoRA Parameters: r=8, alpha=16, dropout=0.1
* Quantization: 4-bit (NF4) via BitsAndBytes
* Precision: Mixed precision with AMP (float16)
* Max sequence length: 1024

## Evaluation

### Task and Domain Similarity Scoring
Each model was manually scored on the following attributes (0–100):
* Task Similarity: Feature Overlap, Problem Type, Goal Alignment
* Domain Similarity: Conceptual Overlap, Domain-Specific Features

| Model Name   | Task Similarity Score | Domain Similarity Score | Performance Score |
|--------------|------------------------|--------------------------|-------------------|
| Gemma-2b-it  | 80.0                   | 62.5                     | 84.0              |
| Phi-2        | 71.7                   | 77.5                     | 52.0              |
| Mistral-7b   | 66.7                   | 80.0                     | 80.5              |

After fine-tuning each model on the text-to-SQL task, I used Ollama to locally run Llama3.2, which served as the evaluator for model-generated SQL outputs. For each test sample, the instruction, schema, and the fine-tuned model’s SQL response were passed as a prompt to Llama3.2, along with the ground-truth SQL. Llama3.2 was prompted to assess the correctness of the generated SQL and assign a numerical score from 0 to 100 based on semantic correctness and alignment with the instruction. These scores were averaged across all test examples to produce a final performance score for each model. 

### Key Findings
* Task similarity is a better predictor of fine-tuning performance than domain similarity.
* Gemma-2B-it outperformed larger models due to stronger task alignment.
* Fine-tuning smaller instruction-aligned models can outperform larger base models in task-specific settings.

