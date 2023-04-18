# SACB: Tackling Label Noise in a Self-Adaptive Class-Balanced Manner
**Abstract:** Label noise is inevitable in the real-world dataset, which makes deep neural networks overfit seriously and hurts the model performance dramatically.
The excellent performance of current methods for learning with label noise is based on prior solid knowledge, such as noise rate, artificially preset threshold, and a well-labeled subset. However, this prior solid knowledge is difficult to estimate and obtain in real-world scenarios. Specifically, we design an innovative elf-adaptive Class-balanced sample Selection (SCS) method. Dynamically updated global thresholds and class-based local thresholds of SCS are estimated to divide the clean and noisy subsets based on given labels’ prediction probability and statistical values. Besides, we propose a Self-adaptive Class-balanced sample Re-weighting (SCR) method. A dynamic truncated normal distribution is estimated to assign different weights to different corrected samples based on their confidence to alleviate the deviation of correcting label class distribution. Finally, we employ Consistency Regularization (CR) between the sample’s strong data augmentation predictions and the sample’s weak data augmentation correction labels to mitigate the influence of a few inevitable noisy samples in the clean subset. Extensive experiment results on synthetic and real-world datasets demonstrate our proposed method's effectiveness and superiority, especially when the labeled data are extremely noisy (e.g., Symmetric-80%).

# Pipeline

![framework](Figure.png)

# Installation
```
pip install -r requirements.txt
```

# Datasets
We conduct noise robustness experiments on two synthetically corrupted datasets (i.e., CIFAR100N and CIFAR80N) and three real-world datasets (i.e., Web-Aircraft, Web-Car and Web-Bird.
Specifically, we create the closed-set noisy dataset CIFAR100N and the open-set noisy dataset CIFAR80N based on CIFAR100.
To make the open-set noisy dataset CIFAR80N, we regard the last 20 categories in CIFAR100 as out-of-distribution. 
We adopt two classic noise structures: symmetric and asymmetric, with a noise ratio $n \in (0,1)$.

You can download the CIFAR10 and CIFAR100 on [this](https://www.cs.toronto.edu/~kriz/cifar.html).

You can download the Clothing1M from [here](https://github.com/NUST-Machine-Intelligence-Laboratory/weblyFG-dataset).

# Training

An example shell script to run SACB on CIFAR-100N :

```python
CUDA_VISIBLE_DEVICES=0 python main.py --warmup-epoch 20 --epoch 100 --batch-size 128 --lr 0.01 --warmup-lr 0.05  --noise-type symmetric --closeset-ratio 0.2 --lr-decay cosine:20,5e-4,100  --opt sgd --dataset cifar100nc  --momentum_scs 0.9 --momentum_scr 0.9 --alpha 1.0 --aph 0.99 
```
An example shell script to run SACB on CIFAR-80N :

```python
CUDA_VISIBLE_DEVICES=0 python main.py --warmup-epoch 20 --epoch 150 --batch-size 128 --lr 0.05 --warmup-lr 0.05  --noise-type symmetric --closeset-ratio 0.2 --lr-decay cosine:20,5e-4,140  --opt sgd --dataset cifar80no  --momentum_scs 0.999 --momentum_scr 0.85 --alpha 0.5 --aph 0.99  
```
Here is an example shell script to run SACB on Web-aircraft :

```python
CUDA_VISIBLE_DEVICES=0 python main_webfg.py --warmup-epoch 5 --epoch 50 --batch-size 32 --lr 0.005 --warmup-lr 0.005  --lr-decay cosine:5,5e-4,50 --weight-decay 5e-4 --seed 123 --opt sgd --dataset web-bird --SSL True --gpu 0 --momentum-mask 0.999 --momentum-qaq 0.99 --pretrain True --alpha 1 --aph 0.95 --save-weights True
```

# Results on Cifar100N and Cifar80N:

![framework](Table1.png)


# Results on Web-aircraft, Web-bird, and Web-car:

![framework](Table2.png)


# Effects of different compoments in test accuracy (%) onCIFAR100N (noise rate and noise type are 0.5 and symmetric,respectively)

![framework](Table3.png)
