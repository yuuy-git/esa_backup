---
title: "CBAM"
category: Users/kz/papers
tags: 2018, ECCV
created_at: 2018-09-10 21:16:28 +0900
updated_at: 2018-09-10 21:17:02 +0900
published: false
number: 62
---

# Title
* [CBAM](https://arxiv.org/abs/1807.06521)  
  
#Abstract
畳み込みニューラルネットワークの性能を向上させるConvolutional Block Attention Module (CBAM)を提案  
どんなCNNにも組み込めるし、計算量も増えないしすげえよというアピール。  
中間層からchannnel情報とspatial情報を用いてattention mapsを推測し、元のinput feature mapと掛け合わせて featureをrefineする。
<img width="371" alt="cbam1.PNG (115.7 kB)" src="https://img.esa.io/uploads/production/attachments/9824/2018/09/10/41035/9768c1b6-6ff3-4bc9-86f7-2bb0b8f4fba0.PNG">
<img width="360" alt="cbam2.PNG (152.0 kB)" src="https://img.esa.io/uploads/production/attachments/9824/2018/09/10/41035/a2b71aa6-1d5a-47d3-bfa8-e1b1daafca6b.PNG">
<img width="354.5" alt="cbam3.PNG (84.0 kB)" src="https://img.esa.io/uploads/production/attachments/9824/2018/09/10/41035/678ce648-f357-405d-8898-2f33f67e6b1d.PNG">


#Category
CNN

#Context
#### about network
network に関しては depth, width and cardinality(濃度)の３点が現在行われている研究
* depth 
    * 深いほうが良いよね(ResNetなど)
    * 深くするには同じ構造のものを繰り返すほうが良いよね (VGG, ResNetなど)
* width  
    * Wideなほうが良いよね(GoogLeNet) 
* cardinality 
    * 濃いほうが良いよね(どういう基準で判別できるの？)(Xception, ResNeXt)
    * パラメーターの数が少なくて済む
    * deep,widthより表現力の向上に寄与

#### architecture design (attention)
attention is focusing on important features and suppressing unnecessary ones
* attentionはwhere to focusを教えてくれる
* improve the representation of interest

#Contribution

#Comment
すげえ
実験してみたい。

