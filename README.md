**Conversation AI: Transfer learning text using machine reading comprehension and Azure Machine Learning**
<br />
<br />
**Introduction**

Modern machine learning models, especially deep neural networks, often significantly benefit from transfer learning. In computer vision, deep convolutional neural networks trained on a large image classification dataset such as ImageNet have proved to be useful for initializing models on other vision tasks, such as object detection (Zeiler and Fergus, 2014).

In natural language processing (NLP), domain adaptation has traditionally been an important topic for syntactic parsing (McClosky et al., 2010) and named entity recognition (Chiticariu et al., 2010), among others. With the popularity of distributed representation, pre-trained word embedding models such as word2vec (Mikolov et al., 2013) and glove (Pennington et al., 2014) are also widely used for natural language tasks.

Question answering (QA) is a long-standing challenge in NLP, and the community has introduced several paradigms and datasets for the task over the past few years. These paradigms differ from each other in the type of questions and answers and the size of the training data, from a few hundreds to millions of examples.

In this blog, we are particularly interested in the context-aware QA paradigm, where the answer to each question can be obtained by referring to its accompanying context (paragraph or a list of sentences). For human beings, reading comprehension is a basic task, performed daily. As early as in elementary school, we can read an article, and answer questions about its key ideas and details. But for AI, full reading comprehension is still an elusive goal. Therefore, building machines that can perform machine reading comprehension (MRC) is of great interest.

In this blog, we attempt to capture a comprehensive study of existing MRC transfer learning literature in the research community. We came across seven popular MRC algorithms ( **Figure 1** ).  In our work, we evaluate and compare six of these papers – **BIDAF** , **DOCQA** , **ReasoNet** , **R-NET** , **SynNet** and **OpenNMT**. We initialize our models, pretrained on different source QA datasets, and show how standard transfer learning can achieve results on a large target corpus.

<br />
<br />

**Why is Machine Reading Comprehension (MRC) important for enterprises?**

Enterprise chatbots have been on the rise for some time now. Chatbots can simplify applications for users. For example, rather than navigating through an interface or website to find information, users can just say or type what they want. Users can also compress multistep tasks into a single command, such as, &quot;Get my list of open opportunities this quarter, and send it to John Smith.&quot; Chatbots allow for non-expert users to interact with many complex applications in an intuitive fashion from a single interface. This gives rise to powerful automation opportunities, where chatbots trigger actions and orchestrate processes across a range of applications through the course of dialogue in natural language. The business impacts can include reducing costs by increasing self-service, improving end-user experience and satisfaction, delivering relevant information faster, and increasing compliance with internal procedures.

To advance the Enterprise Chatbot scenarios, research and industry has turned toward Conversational AI approaches, especially in complex use cases such as banking, insurance and telecommunications. Beyond chatbot capabilities, Conversational AI strives to understand complex sentences of human speech in the same way humans do. Real human conversation is never straightforward - it is full of imperfections consisting of multi-string words, abbreviations, fragments, mispronunciations and a host of other issues. Intelligent conversational interfaces are the simplest way for businesses to interact with devices, services, customers, suppliers and employees everywhere. Intelligent assistants built on these approaches can be taught and continue to learn every day.

**Machine Reading Comprehension (MRC)** is an integral component for solving the Conversational AI problem we face today. Today MRC approaches can answer objective questions such as _&quot;what causes rain&quot;_ with high accuracy. Such approaches can be used in real-world applications like customer service.

<br />
<br />

**Machine Reading Comprehension (MRC)**

MRC is about answering a query about a given context paragraph. MRC requires modeling complex interactions between the context and the query. Recently, attention mechanisms have been successfully extended to MRC. Typically, these methods use attention to focus on a small portion of the context and summarize it with a fixed-size vector, couple attentions temporally, and/or often form a unidirectional attention. It has been shown that these MRC models perform well for transfer learning and finetuning of text for a new domain.

<br />
<br />

**MRC Transfer Learning**

Recently, several researchers have explored various approaches to attack this MRC transfer learning problem. Their work has been a key step towards developing some scalable solutions to extend MRC to a wider range of domains.
Currently, most state-of-the-art machine reading systems are built on supervised training data–trained end-to-end on data examples, containing not only the articles but also manually labeled questions about articles and corresponding answers. With these examples, the deep learning-based MRC model learns to understand the questions and infer the answers from the article, which involves multiple steps of reasoning and inference. For MRC Transfer Learning, we have 6 models as shown in **Figure 1**.


![alt text](https://github.com/antriv/Transfer_Learning_Text/blob/master/screenshots/main1.PNG)

<br />
<br />

**MRC Fine Tuning**

Despite great progress of transfer learning using MRC, a key problem that has been overlooked until recently – how to build an MRC system for a very niche domain?
Currently, most state-of-the-art machine reading systems are built on supervised training data, trained end-to-end on data examples, containing not only the articles but also manually labeled questions about articles and corresponding answers. With these examples, the deep learning-based MRC models learn to understand the questions and infer the answers from the article, which involves multiple steps of reasoning and inference.

The MRC transfer learning works very well for generic articles. However, for many niche domains or verticals, this supervised training data does not exist. For example, if we need to build a new machine reading system to help doctors find valuable information about a new disease, there could be many documents available, but there is a lack of manually labeled questions about the articles, and the corresponding answers. This challenge is magnified by both the need to build a separate MRC system for each different disease, and that the volume of literature is increasing rapidly. Therefore, it is of crucial importance to figure out how to transfer an MRC system to a niche domain where no manually labeled questions and answers are available, but there is a body of documents.

The idea of generating synthetic data to augment insufﬁcient training data has been explored before. For the target task of translation, [Sennrich et.al., 2016](https://arxiv.org/abs/1511.06709)  proposed to generate synthetic translations given real sentences to reﬁne an existing machine translation system. However, unlike machine translation, for tasks like MRC, we need to synthesize both questions and answers for an article. Moreover, while the question is a syntactically ﬂuent natural language sentence, the answer is mostly a salient semantic concept in the paragraph, such as a named entity, an action, or a number. Since the answer has a different linguistic structure than the question, it may be more appropriate to view answers and questions as two diverse types of data. A novel model called **SynNet** has been proposed by [Golub et. al. 2017](https://arxiv.org/pdf/1706.09789.pdf), to address this critical need. OpenNMT also has a finetuning method implemented by [Xinya Du et. al., 2017](https://arxiv.org/pdf/1705.00106.pdf).

<br />
<br />

**Microsoft AI Platform for training the MRC models**

To build all the MRC models, we use the following tools and Platform –

  **1.** We use [DSVM](https://azure.microsoft.com/en-us/services/virtual-machines/data-science-virtual-machines/) as the compute environment with a NVIDIA Tesla K80 GPU, CUDA and cuDNN libraries.

  **2.** All the experiments were run on [Microsoft Azure Machine Learning (AML)](https://docs.microsoft.com/en-us/azure/machine-learning/preview/). Azure Machine Learning is a cross-platform application, which makes the modelling and model deployment process much faster versus what was possible before. We create the MRC models using open-source packages supported in AML. We use TensorFlow and Keras with Tensorflow backend to build the models. We pip installed all the dependencies in the AML environment.

## Prerequisites:
```
pip install <dependencies>
```
## Experimentation steps:
To install and create the experimentation framework using AML, please follow the simple tutorial here (https://docs.microsoft.com/en-us/azure/machine-learning/preview/quickstart-installation). Once the code is setup in AML- 
1) We run the training code for training the model
2) This produces a trained model
3) We then run the scoring code to test the accuracy of the trained model

<br />
<br />

**Operationalize trained MRC models on Azure for evaluation**

Operationalization is the process of publishing models and code as web services and the consumption of these services to produce business results. The AI models can be deployed to local or Azure Container Service (ACS) cluster. You can scale the service to more containers.

We use Azure Machine Learning (AML) for deploying the MRC models in local system. AML enable data scientists to easily operationalize their models. To operationalize an AI model using AML, we can leverage the command-line interface (CLI), and specify the required configurations using the AML operationalization module. From the AML command line:

1. Make sure to run az login before the environment setup step.
2. Make sure to run az ml env set -n [environment name] -g [resource group] to set up deployment environment
3. AML Web deployment solution architecture diagram is in **Figure 2**.

![alt text](https://github.com/antriv/Transfer_Learning_Text/blob/master/screenshots/deploymentarchitecture.PNG)

4. Once the deployment environment is setup, run the commands below to operationalize the MRC model.
```
az ml service create realtime\
-f scoring_code.py\
-model-file model.pkl\
-s service schema.json\
-n azure_AI_model_api\
-r python\
-collect-model-data true\
-c aml_config\conda_dependencies.yml
```
<br />
<br />

**Evaluation Approach**

For our comparison study, we wanted to take different MRC models trained on different datasets and test them on a single large corpus. In this blog, we use six MRC model approaches - **BIDAF** , **DOCQA** , **ReasoNet** , **R-NET** , **SynNet** and **OpenNMT** - to create a QA-Bot for a big corpus using the trained MRC models and compare the results.

For creating our test corpus, we choose the book [Future Computed](https://blogs.microsoft.com/blog/2018/01/17/future-computed-artificial-intelligence-role-society/) by Harry Shum and Brad Smith. We converted the online book PDF to a word format and removed all images and diagrams from the book. Our test corpus now consists of text only.

**BIDAF** , **DOCQA** , **R-NET** , **SynNet** and **OpenNMT** have open GitHub resources available to reproduce the paper results. We used these open GitHub links to train the models and extended these codes wherever necessary for our comparison study. For the **ReasoNet** paper, we reached out to the authors and got access to their private code for our evaluation work.

Please refer to the detailed blogs below for evaluation of each MRC Model on our test corpus.

[**Part 1 - Evaluating the Bi-Directional Attention Flow (BIDAF) Model**](https://github.com/antriv/Transfer_Learning_Text/blob/master/Transfer_Learning/bi-att-flow/README.md)

[**Part 2 - Evaluating the Document-QA Model**](https://github.com/antriv/Transfer_Learning_Text/blob/master/Transfer_Learning/document-qa/README.md)

[**Part 3 - Evaluating the ReasoNet Model**](https://github.com/antriv/Transfer_Learning_Text/blob/master/Transfer_Learning/ReasoNet/README.md)

[**Part 4 - Evaluating the R-NET Model**](https://github.com/antriv/Transfer_Learning_Text/blob/master/Transfer_Learning/R-net/README.md)

[**Part 5 - Evaluating the SynNet Model**](https://github.com/antriv/Transfer_Learning_Text/tree/master/Finetuning/MSR_QuestionGeneration)

[**Part 6 - Evaluating the OpenNMT Model**](https://github.com/antriv/Transfer_Learning_Text/blob/master/Finetuning/OpenNMT_QuestionGeneration/README.md)

Our comparison work is summarized below.

<br />
<br />

**Learnings from our evaluation work**

In this blog, we investigated the performance of four different MRC methodologies on **SQUAD** and **TriviaQA** datasets from the literature. We compared the performance of transfer learning approach for creating a QA system for the **Future Decoded** book using these pretrained MRC models. Please note, that the comparison evaluation listed here is restricted to our evaluation scenario only. IT might differ for other documents or scenarios.

Our evaluation scenario shows that the performance of the **OpenNMT** fine-tuning approach outperforms that of plain transfer learning MRC mechanisms for domain-specific datasets. However, for generic large articles, the **Document-QA** model outperforms **BIDAF** , **ReasoNet** and **R-NET** models.

We compare the performance in more details below.
<br />
1. **Pros/Cons of using the BiDAF model for Transfer Learning**

    a. **Pros**

    BiDAF model is easy to train and test (thanks to AllenAI for making all the codes available through open [GitHub](https://github.com/allenai/bi-att-flow) link).

    b. **Cons**

    As we can see for Figure 3 and Figure 4 above, the BiDAF model has very restricted usage. It works well only on a small paragraph. Given a larger paragraph or many small paragraphs, this model usually takes a long time and comes back with a probable span as an answer which might not make any sense at all.

    c. **Our Resource Contribution**  **in GitHub:** [**https://github.com/antriv/Transfer\_Learning\_Text/tree/master/Transfer\_Learning/bi-att-flow**](https://github.com/antriv/Transfer_Learning_Text/tree/master/Transfer_Learning/bi-att-flow)

<br />

2. **Pros/Cons of using the Document-QA model for Transfer Learning**

    a. **Pros**

    Document-QA model is very easy to train and test (thanks to AllenAI for making all the codes available through open [GitHub](https://github.com/allenai/document-qa) link). As we can see from **Figure 5** and **Figure 7** , the Document-QA model does a better job compared to the BiDAF model we explored earlier. Given multiple larger documents, this model usually takes a very little time to produce multiple probable spans as answers.

    b. **Cons**

    However, as Document-QA does not give a single answer, it might be the case the most probable answer is assigned a lower priority by the algorithm.which might not make any sense at all. We hypothesize that if the model only sees paragraphs that contain answers, it might become too confident in heuristics or patterns that are only effective when it is known a priori that an answer exists. For example, in **Table 1** (adapted from the [paper](https://arxiv.org/pdf/1710.10723.pdf)) below, we observe that the model will assign high confidence values to spans that strongly match the category of the answer, even if the question words do not match the context. This might work passably well if an answer is present but can lead to highly over-confident extractions in other cases.

    c. **Our Resource Contribution in GitHub:** [https://github.com/antriv/Transfer\_Learning\_Text/tree/master/Transfer\_Learning/document-qa](https://github.com/antriv/Transfer_Learning_Text/tree/master/Transfer_Learning/document-qa)


![alt text](https://github.com/antriv/Transfer_Learning_Text/blob/master/screenshots/maintable1.PNG)

<br />

3. **Pros/Cons of using the ReasoNet model for Transfer Learning**

    a. **Pros**

    ReasoNets make use of multiple turns to effectively exploit and then reason over the relation among queries, documents, and answers. With the use of reinforcement learning, ReasoNets can dynamically determine whether to continue the comprehension process after digesting intermediate results, or to terminate reading.

    b. **Cons**

    It&#39;s hard to recreate results from this paper. No open code is available for this. The ReasoNet model has very restricted usage. It works well only on a small paragraph. Given a larger paragraph, this model usually takes a long time and comes back with a probable span as an answer which might not make any sense at all.

    c. **Our Resource Contribution**

    **GitHub:** We added some Demo code for this work. But no public GitHub code available for this.

<br />

4. **Pros/Cons of using the R-NET model for Transfer Learning**

    a. **Pros**

    Apart from training this on SQUAD, we can train this model also on MS-MARCO. In MS-MARCO, every question has several corresponding passages, so we simply concatenate all passages of one question in the order given in the dataset. Secondly, the answers in MS-MARCO are not necessarily subspans of the passages. In this regard, we choose the span with the highest evaluation score with the reference answer as the gold span in the training and predict the highest scoring span as answer during prediction. R-NET model on MS-MARCO dataset out-performs other competitive baselines like ReasoNet.

    b. **Cons**

    For data-driven approach, labeled data might become the bottleneck for better performance. While texts are abundant, it is not easy to find question-passage pairs that match the style of SQuAD. To generate more data, R-NET model authors trained a sequence-to-sequence question generation model using SQuAD dataset and produced a large amount of pseudo question-passage pairs from English Wikipedia. But analysis shows that the quality of generated questions needs improvement. R-NET works well only on a small paragraph. Given a larger paragraph or many small paragraphs, this model usually takes a long time and comes back with a probable span as an answer which might not make any sense at all.

<br />

5. **Pros/Cons of using the SynNet model for Finetuning**

    a. **Pros:**

    Using the SynNet model on NewsQA dataset was very easy. It also generated great QA pairs on NewsQA dataset.

    b. **Cons**

    The SynNet model has very restricted usage. It is hard to run the open existing code on a custom paragraph/text. IT needs a lot of manual data processing, and the steps are not clear from this open GitHub link. Thus, we could not test it on our test book corpus.

<br />

6. **Pros/Cons of using the OpenNMT model for Finetuning**

    a. **Pros:**

    Using the OpenNMT model, we were able to get most accurate results till date on a niche domain without any additional training data, approaching to the performance of a fully supervised MRC system. OpenNMT works in two stages:

      **i.** Answer Synthesis - Given a text paragraph, generate an answer.

      **ii.** Question Synthesis - Given a text paragraph and an answer, generate a question.

    Once we get the generated QA pairs from a new domain, we can also train a Seq2Seq model on this QA pairs to generate more human-like Conversation AI approach from MRC.

    b. **Cons**

    The OpenNMT model training code is not available open source. It works well only on a small paragraph. Given a larger paragraph or many small paragraphs, this model usually takes a long time and comes back with a probable span as an answer which might not make any sense at all.

<br />
<br />

**Conclusion**

In this blog post, we use Azure Machine Learning to train and compare different MRC models for transfer learning. We evaluate four MRC algorithms and compare their performance by creating a QA bot for a corpus using each of the models. In the blog, we demonstrate the importance of selecting relevant data using transfer learning. We show that considering task and domain-specific characteristics and learning an appropriate data selection measure outperforms off-the-shelf metrics. MRC approaches had given AI more comprehension power, but MRC algorithms still don&#39;t really understand what it reads—it doesn&#39;t know what _&quot;British rock group Coldplay&quot;_ really is, besides it being the answer to a Super Bowl question. There are many NLP applications that need models that can transfer knowledge to new tasks and adapt to new domains with such human level understanding, and we feel this is just the beginning of our text transfer learning journey.

Please feel free to email me at [antriv@microsoft.com](mailto:antriv@microsoft.com) if you have questions.
