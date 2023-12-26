# Employee Point of Contact Classifier

## Introduction

This project was conceived from a challenge encountered during my tenure as an intern at a previous organization. An essential component of the training curriculum involved memorizing the designated points of contact for various potential issues an employee might encounter. While such training is a widespread mandate across contemporary corporate landscapes, it has been observed that employees frequently struggle with identifying the appropriate contact for specific concerns, leading to unresolved issues. The primary objective of this project is to explore the feasibility of leveraging machine learning models to streamline and possibly resolve this identification process.

## Objective

Contemporary corporate structures necessitate a defined network of points of contact to address the multifaceted challenges employees may encounter within the workplace. The aim of this project is to harness the capabilities of a zero-shot classification model to ascertain its efficacy in assigning the correct points of contact based on a generalized dataset. The intent is to refine the model to comprehend corporate communications effectively, thereby facilitating the identification of the appropriate point of contact for a given issue.

## Data

The dataset employed in this study was custom developed, drawing inspiration from the structured training materials provided during my internship. This dataset comprises two distinct columns: one detailing various employee queries or issues, and the other specifying the corresponding point of contact. It includes a collection of thirty distinct problem scenarios, each paired with an ideal point of contact within the organizational framework. Although the volume of data is relatively modest and may not be conducive to fine-tuning a model, it is ideally suited for the intended purpose of evaluating the performance of a zero-shot learning model. For details on the dataset, including its specific contents and distribution, refer to the appendix section of this report.

![Data Distribution(https://imgur.com/a/scBu31b)

## Model Choice

In the initial experimentation phase, the 'bart-large-mnli' model, developed by Facebook, was employed for zero-shot classification tasks. Despite extensive prompt engineering efforts, the model failed to yield satisfactory accuracy levels during the testing phase. Subsequently, a thorough examination of the 'text classification' segment within the HuggingFace Model page was undertaken, focusing on models with high download rates. It was during this review that the work of Moritz Laurer came to attention. Among his contributions, the 'deberta-v3-large-zeroshot-v1' model demonstrated superior performance on the curated dataset. This particular model is recognized as an enhanced derivative of the BERT architecture, optimized for zero-shot learning applications.

## Specific Questions + Hypothesis

- **Primary Objective:** Can the zero-shot classifier models (bart, deberta), effectively identify the appropriate point of contact (HR representative, team manager, or team partner) in various corporate scenarios?
- **Comparative Analysis:** How does the performance of the BART classifier with zero-shot learning compare to a state-of-the-art, popular Large Language Model like GPT-4?
- **Hypothesis:** It is posited that zero-shot classifier models are adept at discerning the appropriate point of contact within a corporate setting, thereby serving as a preliminary resource for employees prior to seeking direct consultation.

## Approach + Prompt Engineering

To operationalize the zero-shot classification models, the HuggingFace 'pipeline' interface was utilized with 'zero-shot-classification' as the specified task, and 'deberta' as the selected model. The dataset was integrated into a pandas DataFrame, facilitating the iteration over problem descriptions. The initial prompt design aligned with the three targeted classifications: “HR Representative”, “Team Leader”, and “Managing Partner”. Although the model's performance was acceptably accurate, iterative refinement of the prompts led to enhanced results.

Enhancements to the prompts included the use of formatted strings to provide additional context, which progressively improved model accuracy. The first iteration involved prompts such as `f"Contact {contact}"`, with 'contact' being a placeholder for each point of contact. Subsequent iterations incorporated more detailed prompts, for instance, `f"Contact {contact} for this problem"`, which yielded better performance. Through continuous experimentation, the most effective prompt structure identified was `f"Problem to be taken up with {contact}"`, where 'contact' iterated over all possible points of contact. This methodology of incremental prompt refinement was pivotal in optimizing the performance of the zero-shot classification model.

## Results of the Zero-shot Classification Model

The model was mostly successful in making the distinction for the problems given to it. Here is the numerical breakdown of my results:

- Accuracy: 0.73
- Precision: 0.78
- Recall: 0.73
- F1 Score: 0.73

As earlier mentioned, the statistical significance is that the model is able to make the distinction and is able to identify the right person of contact. The confusion matrix for the DeBERTa model is given below.

![Confusion Matrix for DeBERTa Model](https://imgur.com/a/N69IbrI)

## Comparative Analysis (GPT-4)

GPT-4 is recognized as the most advanced large language model presently available. In the comparative analysis, the problem dataset was presented to GPT-4 with the task of predicting labels for each predefined class. This evaluation was conducted without providing additional contextual information to the model. The outcomes of this exercise are detailed in the section below.

- Accuracy: 1.0
- Precision: 1.0
- Recall: 1.0
- F1 Score: 1.0

The confusion matrix for GPT-4 predictions is given below.
![Confusion Matrix for GPT4 Model](https://imgur.com/7wfKrgx)

## Ethical Concerns + Potential Harm + Biases

While the GPT-4 model heavily outperforms the zero-shot classification model, it is not open-sourced and not locally run on personal systems. This presents a privacy risk as the prompts used for GPT-4 are stored in OpenAI’s prompt data and are used to train their next model.

Conversely, while the zero-shot model is locally run and open-sourced and the training process can be viewed freely
