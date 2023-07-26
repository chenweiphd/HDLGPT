![image](https://github.com/chenweiphd/HDLGPT/assets/100336131/d3aed6aa-9698-47ca-a225-fd8432db95d9)



# HDLGPT

## 1 Introduction

Hardware Description Language Generative Pre-trained Transformer (HDLGPT) is a type of transformer-based generative language model that has been pre-trained on large amounts of hardware description language (HDL) code data. 

HDLGPT is a state-of-the-art natural language processing model that has been specifically designed to generate high-quality HDL code for digital hardware circuits. The model is based on a sequence-to-sequence architecture, that takes a sequence of input tokens as input and generates a sequence of output tokens as output. The generated HDL code is not just of high quality, but is also optimized, which makes it ideal for use in the design of modern digital circuits.

With the increasing demand for digital circuits, HDLGPT has emerged as a powerful tool for hardware designers to generate optimized HDL code. The model's ability to generate code from natural language input has made it easier for designers to create complex digital circuits with ease. Moreover, the generated code is of high quality, which reduces the time and effort required for debugging and optimization.

HDLGPT is undeniably a revolutionary technology that has been gaining popularity in the hardware engineering industry. It is a powerful tool that can greatly enhance productivity by reducing the time and effort required to develop complex digital circuits. With HDLGPT, hardware engineers can now focus more on the creative aspects of the design process, rather than spending countless hours on mundane tasks such as coding and testing.

The beauty of HDLGPT lies in its ability to generate optimized HDL code that can help engineers create more efficient and reliable circuits. This is achieved by leveraging the power of pre-trained transformer models, which are capable of learning and predicting patterns in data. By using HDLGPT, engineers can significantly reduce the risk of errors and bugs in their circuits, thus improving their overall quality and performance.

Moreover, HDLGPT can also help engineers explore new design possibilities and quickly iterate through different design options. This is particularly useful when dealing with complex circuits that require a lot of experimentation and fine-tuning. By automating the design process, HDLGPT can help engineers save time and resources, while also enabling them to create more innovative and sophisticated designs.



## 2 Model Training

### 2.1 Supervised Pre-train

The supervised pre-training of HDLGPT involves training the model on a large dataset of program code and some hardware description language (HDL) code, where each input sequence is paired with a corresponding output sequence. This allows the model to learn the syntax and semantics of program code and HDL code and to identify patterns and relationships between different code fragments.

The pre-training stage involves optimizing the model's parameters and hyperparameters, such as the learning rate and batch size, to maximize its performance on the training data.

### 2.2 Fine-tune

Once the model is pre-trained, it can be fine-tuned on a smaller dataset of HDL code that is specific to a particular task or domain.

This process involves adjusting the pre-trained model to fit the nuances and idiosyncrasies of the specific HDL code, allowing it to generate more accurate and relevant code. Fine-tuning can be a time-consuming process, as it requires a significant amount of data to be collected and prepared for training. However, the benefits of fine-tuning are clear: it leads to models that are better suited to specific tasks and domains, and that can generate code that is more accurate and efficient.

### 2.3 Reinforce Learning with Human Feedback

Reinforcement learning with human feedback can be used to further optimize the performance of HDLGPT. This involves rewarding the model for generating high-quality HDL code and penalizing it for generating errors or invalid code. The model can be designed to receive feedback from human experts, who can evaluate the generated code and provide feedback on its quality.

This feedback can be used to adjust the model's parameters and further improve its performance. By combining the power of pre-training with reinforcement learning and human feedback, HDLGPT can become even more accurate and effective in generating optimized HDL code for digital hardware circuits.

## 3 Prompt Engineering

Prompt engineering refers to the process of designing and selecting appropriate prompts for a language model to generate specific outputs. In the case of HDLGPT, prompt engineering is a crucial step in ensuring that the model generates high-quality HDL code for digital hardware circuits.

The process of prompt engineering involves selecting a set of input tokens that best convey the desired output. The selected tokens are then used to generate the output sequence using the HDLGPT model. The challenge in prompt engineering is to find the right balance between providing enough information for the model to generate accurate output, without overwhelming it with unnecessary details.

To overcome this challenge, several techniques can be used, such as providing context-specific prompts, generating multiple prompts and selecting the best one, and using human feedback to refine the prompt selection process. By using these techniques, engineers can ensure that HDLGPT generates optimized HDL code that meets their design requirements.

## 4 Vector Database Design

In order for HDLGPT to generate high-quality HDL code, it needs to have access to a large and diverse set of hardware description language (HDL) vectors. These vectors can be used to train the model, fine-tune it for specific tasks, and evaluate its performance.

To facilitate the use of HDL vectors, a vector database can be created, which contains a collection of HDL code vectors that are organized by their properties and characteristics. This database can be used to quickly search for and retrieve relevant vectors for a particular task or domain.

The database should  be designed to be easily searchable and accessible, so that engineers can quickly find the vectors they need.

To ensure the quality and relevance of the vectors in the database, a rigorous vetting and curation process should be implemented. This process should involve experts in the field of HDL code design, who can evaluate the vectors for their accuracy, relevance, and usefulness.



## Refernce
Importance|Item |  Link  | Comment|
----- | -------- | -----|------|
| * |陈巍：GPT大模型攻克先进32位流水线RISC-V芯片设计难题 | https://zhuanlan.zhihu.com/p/638569518 | Overview of GPT design chip|
| * |陈巍：GPT-4核心技术分析报告（2）|https://zhuanlan.zhihu.com/p/620087339| 重点看上下文学习、Prompt、思维链 |
| *  |ChipChat论文 | https://arxiv.org/pdf/2305.1324 | 重点看prompt模式  |
|   |CyperRio代码 | https://github.com/hello-eternity/Cyberrio |   |
| * |ChipChat代码 | https://zenodo.org/record/7953725 | 参考Prompt   |
|    |ChipGPT论文 | [ChipGPT: How far are we from natural language hardware design](https://arxiv.org/abs/2305.14019) |   |



## Main Authors

Project guide : CHEN Wei, ZHOU Yinhao, LI Ying, LIU Jian

Code development:

| Name       | Organization                                               | Responsible for    |
| ---------- | ---------------------------------------------------------- | ------------------ |
| Sun Shichu | Institute of Microelectronics, Chinese Academy of Sciences | Prompt Engineering |