参数高效微调（PEFT）。其中一种流行的PEFT方法是低秩适应（LoRA），LoRA 是低秩适应 (Low-Rank Adaptation) 的缩写
LoRA 可以将可训练参数数量减少 10,000 倍，GPU 内存需求减少 3 倍。尽管可训练参数更少、训练吞吐量更高且无需额外推理，LoRA 在 RoBERTa、DeBERTa、GPT-2 和 GPT-3 上的模型质量表现与微调相当或更好延迟。

具体思路是，与微调预训练的大型语言模型的权重矩阵（W）中的所有权重相比，微调两个较小的矩阵（A和B），这两个矩阵近似于对原始矩阵的更新。

W0 + ΔW = W0 + BA，其中W0（dk）、A（dr）和B（r*k），r << d、k

这些矩阵构成LoRA适配器。这里的“r”是一个超参数（该论文建议使用1、2、4、8或64，其中4或8在大多数情况下效果最好）。
在训练期间，W0被冻结，不接收梯度更新，而A和B包含可训练参数。W0和ΔW = BA与相同的输入进行乘法运算，它们的输出向量在坐标上进行求和。A使用随机高斯初始化，B使用零初始化，因此在训练开始时ΔW = BA为零。

