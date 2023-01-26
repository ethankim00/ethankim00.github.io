---
title: "Decoder Inference Optimization"
last_modified_at: 2023-01-25T21:02:14-05:00
categories: paper_review
tags:
    - Paper
---
## Introduction

Transformer-based decoder-only models have important applications in natural language processing, particularly for fluent text generation. Practical use cases for such models depend on efficient inference for the purposes of 1) reducing latency for user experience and 2) mitigating the deployment cost of large language models. In this paper, we survey the recent advances in inference optimization for decoder-only models with a focus on GPT2. We categorize the inference optimization techniques into several categories namely run time optimization, architectural model changes, and distillation. We discuss the advantages and disadvantages of each technique and propose a roadmap for the implementation of techniques. Finally, we discuss a real-life bag of tricks for deploying such models.

## Optimization Techniques

The two most basic techniques to improve autoregressive inference without changing the model outputs are K-V caching and graph optimization. Key-value caching involves storing the previous Key and Value pairs during transformer inference and reusing them to generate the next token. Graph optimization, on the other hand, involves analyzing and modifying the computation graph of the model to make it more efficient. This can include techniques such as fusion and kernel replacement. Operator fusion involves combining multiple operations into a single operation that can be performed in a single computation cycle. Other graph operations can be replaced with optimized kernels for faster execution. One newer optimization for the attention operations in transformer models is Flash attention (Dao 2022). Flash attention fuses the softmax and matrix multiplication operation in attention to avoid being bottlenecked by GPU memory.

Further techniques require modifications to the actual mode such as quantization, pruning, and knowledge distillation. Quantization reduces inference computation by lowering the precision of model parameters and activations, typically from 32-bit floating point to 16-bit floating point or even Int8 or Int4. This can significantly reduce the amount of memory required to store the model and the amount of time required to perform computations, but can also introduce some loss of accuracy.

Pruning involves removing unnecessary parameters or operations from the model, which can reduce the number of computations and the size of the model, but again can also impact accuracy. There are several methods for pruning a model, including weight pruning, unit pruning, and structured pruning.

Knowledge distillation involves training a smaller model to mimic the behavior of a larger, pre-trained model (Hinton 2015). The smaller model, called the "student," is trained to produce the same output as the larger model, or "teacher," on a given dataset. By using the output of the teacher model as a target, the student model can learn to perform the same task more efficiently.

## Discussion

These techniques have various pros and cons and can be used in combination with one another \(Movva 2022). Overall in comparison with naive, dense, PyTorch eager model inference a good suite of optimizations can reduce inference cost an order of magnitude or more. K-V caching and graph optimizations as well as flash attention are standard techniques that should not change the model outputs at all and thus can be applied at no cost. Optimized runtimes for transformer models include TensrorRT, OpenVino, and Triton inference server. Other optimization techniques change the model output and thus involve additional work to implement and verify the optimized model outputs. In general FP-16 16-bit inference is stable. Lower precisions such as Int8 and Int4 may require more specialized techniques as well as Quantization Aware Training (Dettmers et al, 2022). In addition, models can be run with TF32 enabled on ampere GPUS to get a significant speedup. Pruning reduces the number of parameters in the model but can require additional computation to select the parameters to prune and may require specialized hardware for inference. Knowledge distillation has a significant advantage in that the smaller student model can be more efficient. However, it is involved in terms of the training setup needed to distill from the teacher model.

![Untitled](/assets/images/DecoderInferenceOptimization/Untitled.png)

## Roadmap

The actual optimizations relevant to a production system depend on a number of factors including the latency and throughput demands, the number of models, and the availability of engineering resources. Generally, the key consideration is to mine the low-hanging fruit first to get the most per engineering hour invested and work on additional optimizations as needed. Given the rapid development of open-source services, it also makes sense to maintain flexibility in order to adapt to new tools as they become available. Concretely useful packages include NVIDIA's faster transformer and Huggingface Optimum. Optimization targets should be selected according to business requirements. For example, setting a minimum latency to return results to a user vs throughput optimizations to save costs on batch processing jobs. Minimally, models should be run with key-value caching on an optimized runtime. The Triton inference server provides such a service along with dynamic batching. Next, if more speedups are needed FP16 should be used. . Int8 can over additional speedup but the fastest static quantization approaches require additional computation. Depending on the number of models and their serving cost, it may be worth it to set up a pipeline for lower levels of quantization. If the serving costs are high enough for a single model it might be worth outsourcing model inference optimization to specialized providers such as [Stochastic.ai](/assets/images/DecoderInferenceOptimizationhttp://stochastic.ai/). Such companies specialize in inference optimization and thus can likely provide the best efficiency.

## Advanced Techniques

### Useful Deployment Techniques

Quick hit optimizations for useful NLP generation systems: These techniques can give you quick and easy improvements for your model deployments.

- truncate obviously hallucinated LM output: If a language model looping and hallucinating repeated text we can truncate the language model output
- cache generations for the most popular inputs
- Optimize your temperature top p  repetition penalty and number of beams parameters
- If serving a very large number of requests set a target GPU utilization below 90% to allow time for autoscaling to kick in

### Large Language Model Inference

Larger language models beyond GPT2 require multiple GPUS. Scaling beyond a single GPU presents an additional set of challenges inherent to splitting the model across multiple GPUs. Frameworks that support multiple GPUs include Deepspeed and Megatron as well as HuggingFace Accelerate.

### Multitask Model Serving

Parameter-efficient finetuning methods only update a fraction of a language model's parameters during finetuning. In addition to memory efficiency, methods such as prompt tuning have the key property that they allow for *in-batch parallel computing* for different finetuned models (Ding et al 2022). Effectively, they can allow the same service to serve efficiently serve dozens of models at one time (Tang 2022).

# References

```jsx

@misc{zhao_compressing_2022,
	title = {Compressing {Sentence} {Representation} for {Semantic} {Retrieval} via {Homomorphic} {Projective} {Distillation}},
	url = {http://arxiv.org/abs/2203.07687},
	abstract = {How to learn highly compact yet effective sentence representation? Pre-trained language models have been effective in many NLP tasks. However, these models are often huge and produce large sentence embeddings. Moreover, there is a big performance gap between large and small models. In this paper, we propose Homomorphic Projective Distillation (HPD) to learn compressed sentence embeddings. Our method augments a small Transformer encoder model with learnable projection layers to produce compact representations while mimicking a large pre-trained language model to retain the sentence representation quality. We evaluate our method with different model sizes on both semantic textual similarity (STS) and semantic retrieval (SR) tasks. Experiments show that our method achieves 2.7-4.5 points performance gain on STS tasks compared with previous best representations of the same size. In SR tasks, our method improves retrieval speed (8.2\${\textbackslash}times\$) and memory usage (8.0\${\textbackslash}times\$) compared with state-of-the-art large models.},
	urldate = {2022-09-26},
	publisher = {arXiv},
	author = {Zhao, Xuandong and Yu, Zhiguo and Wu, Ming and Li, Lei},
	month = mar,
	year = {2022},
	note = {arXiv:2203.07687 [cs]},
	keywords = {Computer Science - Computation and Language, Computer Science - Information Retrieval},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/KMT9GTX2/Zhao et al. - 2022 - Compressing Sentence Representation for Semantic R.pdf:application/pdf;arXiv.org Snapshot:/Users/ethankim/Zotero/storage/QSF5WWEU/2203.html:text/html},
}

@misc{movva_combining_2022,
	title = {Combining {Compressions} for {Multiplicative} {Size} {Scaling} on {Natural} {Language} {Tasks}},
	url = {http://arxiv.org/abs/2208.09684},
	abstract = {Quantization, knowledge distillation, and magnitude pruning are among the most popular methods for neural network compression in NLP. Independently, these methods reduce model size and can accelerate inference, but their relative benefit and combinatorial interactions have not been rigorously studied. For each of the eight possible subsets of these techniques, we compare accuracy vs. model size tradeoffs across six BERT architecture sizes and eight GLUE tasks. We find that quantization and distillation consistently provide greater benefit than pruning. Surprisingly, except for the pair of pruning and quantization, using multiple methods together rarely yields diminishing returns. Instead, we observe complementary and super-multiplicative reductions to model size. Our work quantitatively demonstrates that combining compression methods can synergistically reduce model size, and that practitioners should prioritize (1) quantization, (2) knowledge distillation, and (3) pruning to maximize accuracy vs. model size tradeoffs.},
	urldate = {2022-10-07},
	publisher = {arXiv},
	author = {Movva, Rajiv and Lei, Jinhao and Longpre, Shayne and Gupta, Ajay and DuBois, Chris},
	month = aug,
	year = {2022},
	note = {arXiv:2208.09684 [cs]},
	keywords = {Computer Science - Computation and Language, I.2.7},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/WC4J492J/Movva et al. - 2022 - Combining Compressions for Multiplicative Size Sca.pdf:application/pdf;arXiv.org Snapshot:/Users/ethankim/Zotero/storage/JNUCAJAQ/2208.html:text/html},
}

@misc{sun_contrastive_2020,
	title = {Contrastive {Distillation} on {Intermediate} {Representations} for {Language} {Model} {Compression}},
	url = {http://arxiv.org/abs/2009.14167},
	doi = {10.48550/arXiv.2009.14167},
	abstract = {Existing language model compression methods mostly use a simple L2 loss to distill knowledge in the intermediate representations of a large BERT model to a smaller one. Although widely used, this objective by design assumes that all the dimensions of hidden representations are independent, failing to capture important structural knowledge in the intermediate layers of the teacher network. To achieve better distillation efficacy, we propose Contrastive Distillation on Intermediate Representations (CoDIR), a principled knowledge distillation framework where the student is trained to distill knowledge through intermediate layers of the teacher via a contrastive objective. By learning to distinguish positive sample from a large set of negative samples, CoDIR facilitates the student's exploitation of rich information in teacher's hidden layers. CoDIR can be readily applied to compress large-scale language models in both pre-training and finetuning stages, and achieves superb performance on the GLUE benchmark, outperforming state-of-the-art compression methods.},
	urldate = {2022-10-14},
	publisher = {arXiv},
	author = {Sun, Siqi and Gan, Zhe and Cheng, Yu and Fang, Yuwei and Wang, Shuohang and Liu, Jingjing},
	month = sep,
	year = {2020},
	note = {arXiv:2009.14167 [cs]},
	keywords = {Computer Science - Computation and Language, Computer Science - Machine Learning},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/DVWKW6S8/Sun et al. - 2020 - Contrastive Distillation on Intermediate Represent.pdf:application/pdf;arXiv.org Snapshot:/Users/ethankim/Zotero/storage/376JPMYM/2009.html:text/html},
}

@misc{sung_lst_2022,
	title = {{LST}: {Ladder} {Side}-{Tuning} for {Parameter} and {Memory} {Efficient} {Transfer} {Learning}},
	shorttitle = {{LST}},
	url = {http://arxiv.org/abs/2206.06522},
	doi = {10.48550/arXiv.2206.06522},
	abstract = {Fine-tuning large pre-trained models on downstream tasks has been adopted in a variety of domains recently. However, it is costly to update the entire parameter set of large pre-trained models. Although recently proposed parameter-efficient transfer learning (PETL) techniques allow updating a small subset of parameters (e.g. only using 2\% of parameters) inside a pre-trained backbone network for a new task, they only reduce the training memory requirement by up to 30\%. This is because the gradient computation for the trainable parameters still requires backpropagation through the large pre-trained backbone model. To address this, we propose Ladder Side-Tuning (LST), a new PETL technique that can reduce training memory requirements by more substantial amounts. Unlike existing parameter-efficient methods that insert additional parameters inside backbone networks, we train a ladder side network, a small and separate network that takes intermediate activations as input via shortcut connections (called ladders) from backbone networks and makes predictions. LST has significantly lower memory requirements than previous methods, because it does not require backpropagation through the backbone network, but instead only through the side network and ladder connections. We evaluate our method with various models (T5 and CLIP-T5) on both NLP (GLUE) and vision-and-language (VQA, GQA, NLVR2 , MSCOCO) tasks. LST saves 69\% of the memory costs to fine-tune the whole network, while other methods only save 26\% of that in similar parameter usages (hence, 2.7x more memory savings). Moreover, LST achieves higher accuracy than Adapter and LoRA in a low-memory regime. To further show the advantage of this better memory efficiency, we also apply LST to larger T5 models, attaining better GLUE performance than full fine-tuning and other PETL methods. The accuracy-efficiency trade-off also holds on VL tasks.},
	urldate = {2022-11-11},
	publisher = {arXiv},
	author = {Sung, Yi-Lin and Cho, Jaemin and Bansal, Mohit},
	month = oct,
	year = {2022},
	note = {arXiv:2206.06522 [cs]},
	keywords = {Computer Science - Computation and Language, Computer Science - Computer Vision and Pattern Recognition, Computer Science - Artificial Intelligence},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/MYU6DSQM/Sung et al. - 2022 - LST Ladder Side-Tuning for Parameter and Memory E.pdf:application/pdf;arXiv.org Snapshot:/Users/ethankim/Zotero/storage/FKNSFD5N/2206.html:text/html},
}

@misc{wang_adamix_2022,
	title = {{AdaMix}: {Mixture}-of-{Adaptations} for {Parameter}-efficient {Model} {Tuning}},
	shorttitle = {{AdaMix}},
	url = {http://arxiv.org/abs/2205.12410},
	doi = {10.48550/arXiv.2205.12410},
	abstract = {Standard fine-tuning of large pre-trained language models (PLMs) for downstream tasks requires updating hundreds of millions to billions of parameters, and storing a large copy of the PLM weights for every task resulting in increased cost for storing, sharing and serving the models. To address this, parameter-efficient fine-tuning (PEFT) techniques were introduced where small trainable components are injected in the PLM and updated during fine-tuning. We propose AdaMix as a general PEFT method that tunes a mixture of adaptation modules -- given the underlying PEFT method of choice -- introduced in each Transformer layer while keeping most of the PLM weights frozen. For instance, AdaMix can leverage a mixture of adapters like Houlsby or a mixture of low rank decomposition matrices like LoRA to improve downstream task performance over the corresponding PEFT methods for fully supervised and few-shot NLU and NLG tasks. Further, we design AdaMix such that it matches the same computational cost and the number of tunable parameters as the underlying PEFT method. By only tuning 0.1-0.2\% of PLM parameters, we show that AdaMix outperforms SOTA parameter-efficient fine-tuning and full model fine-tuning for both NLU and NLG tasks.},
	urldate = {2022-11-11},
	publisher = {arXiv},
	author = {Wang, Yaqing and Agarwal, Sahaj and Mukherjee, Subhabrata and Liu, Xiaodong and Gao, Jing and Awadallah, Ahmed Hassan and Gao, Jianfeng},
	month = nov,
	year = {2022},
	note = {arXiv:2205.12410 [cs]},
	keywords = {Computer Science - Computation and Language, Computer Science - Machine Learning, Computer Science - Artificial Intelligence},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/G9CNBYE6/Wang et al. - 2022 - AdaMix Mixture-of-Adaptations for Parameter-effic.pdf:application/pdf;arXiv.org Snapshot:/Users/ethankim/Zotero/storage/EPM5YCW6/2205.html:text/html},
}

@inproceedings{anonymous_adaptive_2022,
	title = {Adaptive {Budget} {Allocation} for {Parameter}-{Efficient} {Fine}-{Tuning}},
	url = {https://openreview.net/forum?id=lq62uWRJjiY},
	abstract = {Fine-tuning large pre-trained language models on downstream tasks has become an important paradigm in NLP. However, common practice fine-tunes all of the parameters in a pre-trained model, which becomes prohibitive when a large number of downstream tasks are present. Therefore, many fine-tuning methods are proposed to learn incremental updates of pre-trained weights in a parameter efficient way, e.g., low-rank increments. These methods often evenly distribute the budget of incremental updates across all pre-trained weight matrices, and overlook the varying importance of different weight parameters. As a consequence, the fine-tuning performance is suboptimal. To bridge this gap, we propose MARVEL, which adaptively allocates the parameter budget among weight matrices according to their importance score. In particular, MARVEL parameterizes the incremental updates in the form of singular value decomposition. Such a novel approach allows us to effectively prune the singular values of unimportant updates, which is essentially to reduce their parameter budget but circumvent intensive exact SVD computations. We conduct extensive experiments with several pre-trained models on natural language processing, question answering, and natural language generation to validate the effectiveness of MARVEL. Results demonstrate that MARVEL manifests notable improvement over baselines, especially in the low budget settings. Our code will be publicly available.},
	language = {en},
	urldate = {2022-11-11},
	author = {Anonymous},
	month = oct,
	year = {2022},
	file = {Full Text PDF:/Users/ethankim/Zotero/storage/TA8DBT62/Anonymous - 2022 - Adaptive Budget Allocation for Parameter-Efficient.pdf:application/pdf;Snapshot:/Users/ethankim/Zotero/storage/F55I3XNW/forum.html:text/html},
}

@misc{hinton_distilling_2015,
	title = {Distilling the {Knowledge} in a {Neural} {Network}},
	url = {http://arxiv.org/abs/1503.02531},
	doi = {10.48550/arXiv.1503.02531},
	abstract = {A very simple way to improve the performance of almost any machine learning algorithm is to train many different models on the same data and then to average their predictions. Unfortunately, making predictions using a whole ensemble of models is cumbersome and may be too computationally expensive to allow deployment to a large number of users, especially if the individual models are large neural nets. Caruana and his collaborators have shown that it is possible to compress the knowledge in an ensemble into a single model which is much easier to deploy and we develop this approach further using a different compression technique. We achieve some surprising results on MNIST and we show that we can significantly improve the acoustic model of a heavily used commercial system by distilling the knowledge in an ensemble of models into a single model. We also introduce a new type of ensemble composed of one or more full models and many specialist models which learn to distinguish fine-grained classes that the full models confuse. Unlike a mixture of experts, these specialist models can be trained rapidly and in parallel.},
	urldate = {2023-01-09},
	publisher = {arXiv},
	author = {Hinton, Geoffrey and Vinyals, Oriol and Dean, Jeff},
	month = mar,
	year = {2015},
	note = {arXiv:1503.02531 [cs, stat]},
	keywords = {Computer Science - Machine Learning, Computer Science - Neural and Evolutionary Computing, Statistics - Machine Learning},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/U4V8YV4Z/Hinton et al. - 2015 - Distilling the Knowledge in a Neural Network.pdf:application/pdf},
}

@misc{ding_delta_2022,
	title = {Delta {Tuning}: {A} {Comprehensive} {Study} of {Parameter} {Efficient} {Methods} for {Pre}-trained {Language} {Models}},
	shorttitle = {Delta {Tuning}},
	url = {http://arxiv.org/abs/2203.06904},
	abstract = {Despite the success, the process of fine-tuning large-scale PLMs brings prohibitive adaptation costs. In fact, fine-tuning all the parameters of a colossal model and retaining separate instances for different tasks are practically infeasible. This necessitates a new branch of research focusing on the parameter-efficient adaptation of PLMs, dubbed as delta tuning in this paper. In contrast with the standard fine-tuning, delta tuning only fine-tunes a small portion of the model parameters while keeping the rest untouched, largely reducing both the computation and storage costs. Recent studies have demonstrated that a series of delta tuning methods with distinct tuned parameter selection could achieve performance on a par with full-parameter fine-tuning, suggesting a new promising way of stimulating large-scale PLMs. In this paper, we first formally describe the problem of delta tuning and then comprehensively review recent delta tuning approaches. We also propose a unified categorization criterion that divide existing delta tuning methods into three groups: addition-based, specification-based, and reparameterization-based methods. Though initially proposed as an efficient method to steer large models, we believe that some of the fascinating evidence discovered along with delta tuning could help further reveal the mechanisms of PLMs and even deep neural networks. To this end, we discuss the theoretical principles underlying the effectiveness of delta tuning and propose frameworks to interpret delta tuning from the perspective of optimization and optimal control, respectively. Furthermore, we provide a holistic empirical study of representative methods, where results on over 100 NLP tasks demonstrate a comprehensive performance comparison of different approaches. The experimental results also cover the analysis of combinatorial, scaling and transferable properties of delta tuning.},
	urldate = {2023-01-09},
	publisher = {arXiv},
	author = {Ding, Ning and Qin, Yujia and Yang, Guang and Wei, Fuchao and Yang, Zonghan and Su, Yusheng and Hu, Shengding and Chen, Yulin and Chan, Chi-Min and Chen, Weize and Yi, Jing and Zhao, Weilin and Wang, Xiaozhi and Liu, Zhiyuan and Zheng, Hai-Tao and Chen, Jianfei and Liu, Yang and Tang, Jie and Li, Juanzi and Sun, Maosong},
	month = mar,
	year = {2022},
	note = {arXiv:2203.06904 [cs]},
	keywords = {Computer Science - Artificial Intelligence, Computer Science - Computation and Language, Computer Science - Machine Learning},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/9DM3N9RQ/Ding et al. - 2022 - Delta Tuning A Comprehensive Study of Parameter E.pdf:application/pdf;arXiv.org Snapshot:/Users/ethankim/Zotero/storage/PHPMETQD/2203.html:text/html},
}

@misc{dettmers_llmint8_2022,
	title = {{LLM}.int8(): 8-bit {Matrix} {Multiplication} for {Transformers} at {Scale}},
	shorttitle = {{LLM}.int8()},
	url = {http://arxiv.org/abs/2208.07339},
	abstract = {Large language models have been widely adopted but require significant GPU memory for inference. We develop a procedure for Int8 matrix multiplication for feed-forward and attention projection layers in transformers, which cut the memory needed for inference by half while retaining full precision performance. With our method, a 175B parameter 16/32-bit checkpoint can be loaded, converted to Int8, and used immediately without performance degradation. This is made possible by understanding and working around properties of highly systematic emergent features in transformer language models that dominate attention and transformer predictive performance. To cope with these features, we develop a two-part quantization procedure, LLM.int8(). We first use vector-wise quantization with separate normalization constants for each inner product in the matrix multiplication, to quantize most of the features. However, for the emergent outliers, we also include a new mixed-precision decomposition scheme, which isolates the outlier feature dimensions into a 16-bit matrix multiplication while still more than 99.9\% of values are multiplied in 8-bit. Using LLM.int8(), we show empirically it is possible to perform inference in LLMs with up to 175B parameters without any performance degradation. This result makes such models much more accessible, for example making it possible to use OPT-175B/BLOOM on a single server with consumer GPUs. We open-source our software.},
	urldate = {2023-01-09},
	publisher = {arXiv},
	author = {Dettmers, Tim and Lewis, Mike and Belkada, Younes and Zettlemoyer, Luke},
	month = nov,
	year = {2022},
	note = {arXiv:2208.07339 [cs]},
	keywords = {Computer Science - Artificial Intelligence, Computer Science - Machine Learning},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/LEJB3RH9/Dettmers et al. - 2022 - LLM.int8() 8-bit Matrix Multiplication for Transf.pdf:application/pdf;arXiv.org Snapshot:/Users/ethankim/Zotero/storage/V9FZTM5L/2208.html:text/html},
}

@misc{yao_zeroquant_2022,
	title = {{ZeroQuant}: {Efficient} and {Affordable} {Post}-{Training} {Quantization} for {Large}-{Scale} {Transformers}},
	shorttitle = {{ZeroQuant}},
	url = {http://arxiv.org/abs/2206.01861},
	abstract = {How to efficiently serve ever-larger trained natural language models in practice has become exceptionally challenging even for powerful cloud servers due to their prohibitive memory/computation requirements. In this work, we present an efficient and affordable post-training quantization approach to compress large Transformer-based models, termed as ZeroQuant. ZeroQuant is an end-to-end quantization and inference pipeline with three main components: (1) a fine-grained hardware-friendly quantization scheme for both weight and activations; (2) a novel affordable layer-by-layer knowledge distillation algorithm (LKD) even without the access to the original training data; (3) a highly-optimized quantization system backend support to remove the quantization/dequantization overhead. As such, we are able to show that: (1) ZeroQuant can reduce the precision for weights and activations to INT8 in a cost-free way for both BERT and GPT3-style models with minimal accuracy impact, which leads to up to 5.19x/4.16x speedup on those models compared to FP16 inference; (2) ZeroQuant plus LKD affordably quantize the weights in the fully-connected module to INT4 along with INT8 weights in the attention module and INT8 activations, resulting in 3x memory footprint reduction compared to the FP16 model; (3) ZeroQuant can be directly applied to two of the largest open-sourced language models, including GPT-J6B and GPT-NeoX20, for which our INT8 model achieves similar accuracy as the FP16 model but achieves up to 5.2x better efficiency.},
	urldate = {2023-01-09},
	publisher = {arXiv},
	author = {Yao, Zhewei and Aminabadi, Reza Yazdani and Zhang, Minjia and Wu, Xiaoxia and Li, Conglong and He, Yuxiong},
	month = jun,
	year = {2022},
	note = {arXiv:2206.01861 [cs]},
	keywords = {Computer Science - Computation and Language, Computer Science - Machine Learning},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/9MLRV23P/Yao et al. - 2022 - ZeroQuant Efficient and Affordable Post-Training .pdf:application/pdf;arXiv.org Snapshot:/Users/ethankim/Zotero/storage/PQUNJE5T/2206.html:text/html},
}

@misc{dao_flashattention_2022,
	title = {{FlashAttention}: {Fast} and {Memory}-{Efficient} {Exact} {Attention} with {IO}-{Awareness}},
	shorttitle = {{FlashAttention}},
	url = {http://arxiv.org/abs/2205.14135},
	abstract = {Transformers are slow and memory-hungry on long sequences, since the time and memory complexity of self-attention are quadratic in sequence length. Approximate attention methods have attempted to address this problem by trading off model quality to reduce the compute complexity, but often do not achieve wall-clock speedup. We argue that a missing principle is making attention algorithms IO-aware -- accounting for reads and writes between levels of GPU memory. We propose FlashAttention, an IO-aware exact attention algorithm that uses tiling to reduce the number of memory reads/writes between GPU high bandwidth memory (HBM) and GPU on-chip SRAM. We analyze the IO complexity of FlashAttention, showing that it requires fewer HBM accesses than standard attention, and is optimal for a range of SRAM sizes. We also extend FlashAttention to block-sparse attention, yielding an approximate attention algorithm that is faster than any existing approximate attention method. FlashAttention trains Transformers faster than existing baselines: 15\% end-to-end wall-clock speedup on BERT-large (seq. length 512) compared to the MLPerf 1.1 training speed record, 3\${\textbackslash}times\$ speedup on GPT-2 (seq. length 1K), and 2.4\${\textbackslash}times\$ speedup on long-range arena (seq. length 1K-4K). FlashAttention and block-sparse FlashAttention enable longer context in Transformers, yielding higher quality models (0.7 better perplexity on GPT-2 and 6.4 points of lift on long-document classification) and entirely new capabilities: the first Transformers to achieve better-than-chance performance on the Path-X challenge (seq. length 16K, 61.4\% accuracy) and Path-256 (seq. length 64K, 63.1\% accuracy).},
	urldate = {2023-01-09},
	publisher = {arXiv},
	author = {Dao, Tri and Fu, Daniel Y. and Ermon, Stefano and Rudra, Atri and Ré, Christopher},
	month = jun,
	year = {2022},
	note = {arXiv:2205.14135 [cs]},
	keywords = {Computer Science - Machine Learning},
	file = {arXiv Fulltext PDF:/Users/ethankim/Zotero/storage/ISEJFT8F/Dao et al. - 2022 - FlashAttention Fast and Memory-Efficient Exact At.pdf:application/pdf;arXiv.org Snapshot:/Users/ethankim/Zotero/storage/EYIDJDMA/2205.html:text/html},
}

@article{tang2022dptdr,
  title={DPTDR: Deep Prompt Tuning for Dense Passage Retrieval},
  author={Tang, Zhengyang and Wang, Benyou and Yao, Ting},
  journal={arXiv preprint arXiv:2208.11503},
  year={2022}
}
```