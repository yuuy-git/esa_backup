---
title: "Survey on deeplearning techniques for person Re-Identification Task"
category: Users/kz/papers
tags: 
created_at: 2018-09-02 23:50:37 +0900
updated_at: 2018-09-05 19:20:03 +0900
published: false
number: 53
---

##0 abstract
Person Re-ID is to recognize whether an individual has already been observed over a camera in a network or not.


##1.  Introduction
* Person Re Identificationとは  
とあるカメラで写っていた人物をほかのカメラで写ったかどうかを判定する
queryとしてとある人物がうつっている画像を与え、その人物が写っているビデオフレームを取り出すということを行う。
全てのビデオフレーム(template gallery)はとある人物に似ているかという基準でランク付けする。
* なぜchallengingか？（ここは考えてもらう）  
low image resolution  
unconstrained  
illumination changes
background changes  
occlusions
顔とかが特徴として使えない。らしい（ここ勘違いしていた。）  
* 特徴として使うもの 
Clothing appearance（服装）  
gait(足取り、歩きぶり)    
anthropometric measures(人体の測定、背格好)  
色や形状などの服装を説明因子として利用している  
類似度の指標(matching score)は人が定義したものもしくはデータから学習したものどちらかで定義され、説明因子のペアの類似度を計算する。
* A standard re-ID method  
$D$:Descriptor  
$m(・,・)$:similarity measure between a pair of images  
$T$ descriptor of a template images  
$P$ descriptor of  a probe image  
$G={ T_1,T_2,...,T_n }$ template gallery  
re-iには幾つかのパターンがあり、そのパターンによってtemplate gallearlyの定義も変わる  
    1. single shot which has only one template frame per individual
    2. multiple shots which contains more than one template frame per individual  
* standard framework Re-ID(Figuar2が良いまとめ)  
    1. image description is generated for probe and template images of gallery set.  
    1. the matching scores between each of them is computed
    1. the ranked list is generated by sorting the matching scores
* hand-craftedに特徴量を作る研究もまだ多いが、

CVPR2014からPerson Re-IDの論文数を調べる。

##2.Person Re-id BenchmarkDatasets  
table1を見る  
* VIPeR
* i-LIDS  
* ETHZ  
* CAVIAR  
* CUHK
* PRID
* WARD
* Re-identification Across indoor-outdoor Dataset
* Market-1501  
* MARS  
* DukeMTMC  
* MSMT

##3.Deep Neural Networks for PReID  
* classification models
* Siamese model: based oneither pair or triplet comparisons
(サンプル数少ないので後者が優勢？)

#### 3.1 Models based on Classification 
Classification modelsではinputに対して正解クラスを用意しなければならない。
Figure5は図にする
→Classification modelsはある人物に対するサンプル画像が少ないとoverfittingでうまくいかないケースが多い。
* Wu et al[26]  
汎用化するためにhand-crafted featuresとCNNの特徴量を融合する(fusion)手法を提案。
input(244*244*3),hand-crafted featuresは既存研究のベストな奴ELF
a buffer layerとa fully connected layerいうのがfusion layerとして存在。
buffer layerはfusionする、でCNNとhand-craftのfeaturesのギャップを埋める
(softmaxのoutputlayerは hand-craft featuresによって出力サイズ変わる。は？なぜ？)
ミニバッチ確率的勾配法でとける
hand-crafted featuresの影響が大きい。
（他のhand-crafted featuresはSIFT, color histogramsなど）
例えば[28]ではLAB色空間を用いて、32*32のパッチを16ストライドで14パッチ作る。次にPCAで次元を削減する。embedded埋め込むFisher Vectorを使って埋め込む。２つのFisher vector(SIFT color_hist)は結合される

* Xiao et al[29]  
CNNを使って複数のデータセットから特徴表現を学習し、それぞれのトレー人セットで効果的なニューロンを発見する。  
複数のデータセットから学習しsoftmax lossでトレインした複数のデータセットで同時に機能するbase line modelを最初に作る  
次にそれぞれのデータセットで全サンプルでforward passを実行し、それぞれのニューロンに対して目的関数に平均してどれだけ影響を与えているかを計算する。  
で、standard Dropoutをdeterministic Domain Guided Dropoutに置き換える。これはデータセットごとに使っていないニューロンを捨てるために行う。
その後数エポックもう一度学習する。
例えばの部分割とわかりよかった
* [30]  
不均衡や姿勢のバリエーションをなんとかやりくりするためにCNNを提案。
multi-class person Re-ID networkは全体として２つのsub-networkで構成される。
1つはオリジナルイメージからグローバルな特徴を学ぶconvolutional modelで
もう一つは歩行者の体を６部分に分割し、それぞれのlocalな特徴を学ぶpart-based networkである。
最終的に２つのsubnetworkはnetworkのoutputであるfusion layerの後に重みを共通しながらcombineされる？？よくわからず
でてきた特徴量を使って差を計算して類似度をはかる？
* Li et al [31]  
multi-scale context aware network(カメラによって写る大きさが違うことを言っている？)を提案。身体全体と身体部分を学習し、それぞれの層でmultiple scales of convolution(いろんなカーネルサイズの畳み込み)を保持しておくことで、local contextの知識を獲得できる。　
加えて、前もって定義された固定した部分を使うのではなく、歩行者の身体部分を可変で指定して学習している

####3.2 Models based on Siamese Network
Siamese Networkはサンプル数が限られているこの分野で広く利用されている。  
siamese networkは2つ以上の 同じsub-networkを持つ 。ここでいう同じというのは同じネットワーク構造で同じパラメータの重みをもつ。
Siamese Networkは通常２つのsub-network(pairwise)もしくは3つのsub-network(triplet)で使われる  
出力outputは類似度スコア（similarity score）
