---
title: "model"
category: Users/tk/competitive/kaggle/salt
tags: 
created_at: 2018-09-17 11:26:41 +0900
updated_at: 2018-10-17 15:00:17 +0900
published: false
number: 66
---

# Model
## Model
DeepLab v3を採用する。
https://github.com/tensorflow/models/tree/master/research/deeplab

> My best is DeepLab v3 0.845 LB. With very basic augmentation. The score will improve quite a bit from here....

ということなので、基本的なaugmentationとDeepLab v3の実装を行うことを目標にする。

- モデルのコードが落ちてるか
→Githubに落ちてた
→他のモデルのPre-trainedの重みを利用しないのであれば、別にPytorchにこだわる必要がないので、TensorFlowで良い。

- DeepLabの使い方がわからん
→[How to use DeepLab in TensorFlow for object segmentation using Deep Learning](https://medium.freecodecamp.org/how-to-use-deeplab-in-tensorflow-for-object-segmentation-using-deep-learning-a5777290ab6b)

## Loss
LovaszSoftmaxという関数が良いらしいので使ってみる。
https://github.com/bermanmaxim/LovaszSoftmax

## Data
> The images are 101 x 101 pixels and each pixel is classified as either salt or sediment.

```bash:count
$ ls train/images -U1 | wc -l
4000
$ ls train/masks -U1 | wc -l
4000
$ ls test/images -U1 | wc -l
18000
```

## Augmantation
Flippingを行うだけで結構良いスコアが出たという書き込みがあったのでまずはそれでやってみる。

## Implementation
### ImageSets
まずは、ImageSetsを作る必要がある。
必要なのは以下の3つのテキストファイル。

- train.txt: list of image names for the training set
- val.txt: list of image names for the validation set
- trainval.txt: list of image names for training + validation set

なので、Augmentationを行った後に、ファイル一覧をリストとして取得し、テキストファイルに書き出す。

#### Augmentation
- 学習時に行う方が良いのか、学習前に行った方が良いのかわからない
- 学習時にランダムに処理をする場合、マスクも対応させて同じ処理をさせる方法がよくわからない
→毎回ループ処理時にランダム値を生成して、その値をシード値として指定することで同じ処理を画像とマスク両方にするのか？
→とりあえず今回は事前にすべての画像をフリップさせて水増ししておく

# Discussion
## [Pre-Trained Model](https://www.kaggle.com/c/tgs-salt-identification-challenge/discussion/61968)
Heng CherKengというけろっぴのアイコンの人（現在ランク11）が使っているpretrained-model
>pytorch models on this page and links from this page:
https://github.com/Cadene/pretrained-models.pytorch
https://github.com/ansleliu/LightNet

## [binary empty vs non-empty classifier](https://www.kaggle.com/c/tgs-salt-identification-challenge/discussion/65933)

How to test binary classifier on public LB?

1. submit a all zero mask csv to server. Reported LB is 0.38. i.e. There are 38% test images with zero mask.

1. use your classifier to predict which image is empty. Make a zero mask csv. For those predicted empty images, change the csv such that their rle ='something but not null'

1. submit your new csv. If your prediction is 100% correct, the new LB score will still be 0.38. For every wrong prediction you make, the LB will decrease from 0.38. From there, you can work out how accurate is your binary classifier

## [comparing different framework (unet, deeplab, linknet, psp, refinenet, etc ...)](https://www.kaggle.com/c/tgs-salt-identification-challenge/discussion/64645)
DeepLab v3 looks like the best model for this competition.
But we need do something to train correctly.

> I have been trying several models.
My best is DeepLab v3 0.845 LB. With very basic augmentation. The score will improve quite a bit from here....
...
>  I am creating an encoder/decoder network piece by piece that incorporates features from other successful networks that have been proven on segmentation tasks and not necessarily image classification tasks. For example, I like resnets because they enable you to train rather large structures due to their skip connections. **The Unet concept is key because you feed features at different levels from your encoder over to your decoder**. I like MSc, ASPP and CRF concepts from Deeplab. **The trick is how to put this all together and still be able to train it properly.**

## あとで読むリスト
- [Best single model without TTA](https://www.kaggle.com/c/tgs-salt-identification-challenge/discussion/64776)
- [data observation](https://www.kaggle.com/c/tgs-salt-identification-challenge/discussion/65556)
- [Augmentation that works](https://www.kaggle.com/c/tgs-salt-identification-challenge/discussion/63974)
- [how to stack 2nd layer of CNN ... with limited train data](https://www.kaggle.com/c/tgs-salt-identification-challenge/discussion/65763)
- [Improving from 0.78 to 0.84+](https://www.kaggle.com/c/tgs-salt-identification-challenge/discussion/65226) 
- [common tricks in kaggle image segmentation problems](https://www.kaggle.com/c/tgs-salt-identification-challenge/discussion/63984)


# Kernels
## [Intro to seismic, salt, and how to geophysics](https://www.kaggle.com/jesperdramsch/intro-to-seismic-salt-and-how-to-geophysics)

## あとで読むリスト
- [U-net, dropout, augmentation, stratification](https://www.kaggle.com/phoenigs/u-net-dropout-augmentation-stratification)
- [Basic data visualization using PyTorch Dataset](https://www.kaggle.com/skainkaryam/basic-data-visualization-using-pytorch-dataset)
- [NO MASK prediction](https://www.kaggle.com/osciiart/no-mask-prediction)
- [UNet+ResNet34 in keras](https://www.kaggle.com/meaninglesslives/unet-resnet34-in-keras)


# Note
## 疑問点

