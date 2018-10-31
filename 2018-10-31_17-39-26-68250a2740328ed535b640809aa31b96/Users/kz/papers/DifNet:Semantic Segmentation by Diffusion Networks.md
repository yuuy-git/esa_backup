---
title: "DifNet:Semantic Segmentation by Diffusion Networks"
category: Users/kz/papers
tags: 2018, NIPS
created_at: 2018-09-26 09:43:27 +0900
updated_at: 2018-09-26 09:45:53 +0900
published: true
number: 69
---

# Title
* [DifNet:Semantic Segmentation by Diffusion Networks](https://arxiv.org/abs/1805.08015)  
  
#Abstract
Deep Neural NetがSegのState of the artにはなったが、下記はまだ苦手。
* poor boundary localization  
* spatial fragmented predictions  
ではなぜ難しいか？  
→requirement of making dense predictions from a long path model all at once,
since details are hard to keep when data goes through deeper layers.   
層が深くなるほど詳細な情報を維持しておくのは難しいので、層の深いモデルで密な予測を要求することがそもそも構造的に難しい。  

そこでどうしたか？
発想：この難しさに対処するためSemantic Segmentationを2つの問題に分解
*  seed detection   
    →which is required to predict initial predictions without need of wholeness
and preciseness
(wholeness 完全な)
* similarity estimation   
    →which measures the possibility of any two nodes belong to the same class without need of knowing which class they are.   
具体的には
 We use one branch for one sub-task each, and apply a cascade of random
walks base on hierarchical semantics to approximate a complex diffusion process which propagates seed information to the whole image according to the estimated similarities.(よくわからん)

#Category
Segmentation

#Context
FCNからSegはDeepにその後も[2]-[16]で発展も下記の問題は残る
* poor boundary localization  
* spatial fragmented predictions  
理由①層が深くなると位置情報は失われる(既存はskiplayerで対応)  
理由②層が深いモデルで一度に予測すると理由①の問題はより顕著になる  
理由③ the lack of ability to capturing long-range dependencies causes
model hard to generate accuracy and uniform predictions [17].  
（広範囲の依存関係を捉えられないので全体として一意的な予測ができない）

#Contribution
DiffNetの提案
上記課題に対応するためにSemantic Segmentationを2つのtaskに分解
each sub-task, we train one branch network respectively and simultaneously, and thus our model has two branches: seed branch and similarity branch.   

*  seed detection   
we hope it can give a initial predictions without need of wholeness
and preciseness, this requirement is highly appropriate to the property of DNNs’ high level features which are good at reflecting high level semantic but hard to keep details.  
 seed branch firstly predicts score map which gives score values of all classes for each node, where the larger the certain score value is, the more likely the node belongs to the corresponding class, then seed branch learns a importance map which will be used to re-weight each channel of score map.
*  similarity estimation  
we intend to estimate the possibility of any two nodes that they belong to the same class, in this case, relative low level features already could be competent.  
similarity branch will extract different levels of features, from which transition matrices are computed. Transition matrices measure the possibility of random walk between any two nodes, with our implementation, they also could reflect similarities on different semantic levels.
  
その後diffuse seeds information to the whole image according to the estimated similarities.(seed detectionでdetectした情報をestimated estimationを用いて画像全体に適応するということ。)詳しくは下記  
 Finally, we apply a cascade of random walks based on these transition matrices to approximate a complex diffusion process, in order to propagate seed information to the whole image according to the hierarchical similarities. In this way, the inversion operation of dense matrix in diffusion process could be avoid. Our diffusion process by cascaded random walks shares the similar idea as residual learning framework [18] who eases the approximation of a complex objective by learning residuals. Moreover, our random walk actually computes the final response at a position as a weighted sum of all the seeds values which is a nonlocal operation that can capture long-range dependencies regardless of positional distance. And from the Fig. 1(a), we can see the cascaded random walks also increase the flexibility and diversity of information propagation paths.

#Comment
実装したい

<img width="522.5" alt="diffnet.PNG (335.6 kB)" src="https://img.esa.io/uploads/production/attachments/9824/2018/09/26/41035/c6aedcb5-22d0-4bbb-af31-4d98bb31c237.PNG">

