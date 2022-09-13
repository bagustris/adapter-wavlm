<!-- # Introduction
This repository contains a version of WavLM that can be trained using adapters.
Adapters are lightweight modules that can be inserted into some intermediate layers of a frozen pre-trained model. We proposed a novel adapter architecture for multiple speech processing tasks. The proposed adapter architecture produces effective features for downstream tasks to use a weighted sum of outputs of intermediate layers of the WavLM encoder. -->
# Abstract
Fine-tuning of self-supervised models is a powerful transfer learning method in a variety of fields, including speech processing, since it can utilize generic feature representations obtained from large unlabeled data. Fine-tuning, however, requires a new parameter set for each downstream task, which is parameter inefficient. Adapter architectures can improve the parameter efficiency by inserting lightweight learnable modules into a frozen pre-trained model. However, existing adapter architectures fail to adaptively leverage low- to high-level features stored in different layers, which is necessary for tasks other than automatic speech recognition. Thus, we propose a new adapter architecture to acquire feature representations more flexibly for various speech tasks. In experiments, we applied this adapter to WavLM on four speech tasks. It performed on par or better than na ̈ıve fine-tuning, with less than 10% of learnable parameters. It also outperformed an existing adapter architecture.

# Adapter Architecture
<img width="337" alt="layeradapter" src="https://user-images.githubusercontent.com/48460458/189790212-b1863b1a-c985-4e1f-86a4-363cd9f31ffc.png">
<!-- ![layeradapter5](https://user-images.githubusercontent.com/48460458/189691649-a3fd4264-3de6-4e61-abe5-3c60a67b07ac.png) -->

The proposed adapter architecture incorporates two types of adapters, namely Layer adapters (L-adapters) and Encoder adapter (E-adapters), into a frozen backbone. The L-adapters bridge each intermediate layer and the top layer as shown in Figure 1a. They help the model to quickly adapt speech representations to various downstream tasks, and also to reduce dependency on the initialization of adapter parameters. The E-adapters are inserted to each encoder layer in a similar way as previous work [EFFICIENT ADAPTER TRANSFER OF SELF-SUPERVISED SPEECH MODELS FOR AUTOMATIC SPEECH RECOGNITION](https://arxiv.org/pdf/2202.03218.pdf) as shown in Figure 1b. In contrast to the previous work, our architecture does not have adapters after the multi-head self-attention (MHSA) modules, and alternatively has L-adapters.
We use wavlm-base-plus as the model backbone.

# Experiment and Results
<!-- ![result](https://user-images.githubusercontent.com/48460458/189800739-e711e953-9095-45d6-bdec-f509581965bb.png) -->
<img width="657" alt="result_cap" src="https://user-images.githubusercontent.com/48460458/189802186-482752a1-ad28-4482-bd6d-8ae11e582a8c.png">
We demonstrate the effectiveness of the proposed method on
four downstream tasks: automatic speaker verification (ASV), emotion recognition (ER), automatic speech recognition (ASR) and intent classification (IC). 
We conduct experiments to compare the performance of different five training methods in four tasks.

| Method              | ASV EER                     | ER WA                         | ASR WER                      | IC AAC                        | 
| ------------------- | --------------------------- | ----------------------------- | ---------------------------- | ----------------------------- | 
| Fine-tuning         | $4.42\pm 0.25$              | $79.03 \pm 0.62$              | $\boldsymbol{7.87} \pm 0.08$ | $99.65 \pm 0.076$             | 
| Conventional method | $3.95 \pm0.29$              | $79.19 \pm 0.44$              | $8.92 \pm 0.13$              | $99.61 \pm 0.042$             | 
| Proposed method     | $\boldsymbol{2.63}\pm 0.09$ | $\boldsymbol{80.00} \pm 0.31$ | $7.90 \pm 0.06$              | $99.67 \pm 0.041$             | 
| L-adapters-only     | $2.74\pm 0.09$              | $78.93 \pm 0.52$              | $9.50 \pm 0.08$              | $\boldsymbol{99.68}\pm 0.025$ | 
| E-adapters-only     | $4.82\pm 0.02$              | $76.91 \pm 0.48$              | $9.00 \pm 0.16$              | $99.66 \pm 0.040$             | 

<ol type=i>
    <li>Fine-tuning: Fine-tuning all layers of the WavLM encoder
    <li>Conventional method: Adapters are inserted into after a MHSA module and a feedfoward module in each layer of the WavLM encoder.
    <li>Proposed method: There are L-adapters and E-adapters attached on the WavLM encoder. L-adapters are attached to all layers and E-adapters are inserted into all but the final layer.
    <li> L-Adapters-only: L-adapters are attached to all layers of the WavLM encoder.
    <li> E-adapters-only: E-adapters are inserting into all layers of the WavLM encoder.
</ol>
