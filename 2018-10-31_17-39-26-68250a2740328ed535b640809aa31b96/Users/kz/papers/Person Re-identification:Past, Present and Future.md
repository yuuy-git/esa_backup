---
title: "Person Re-identification:Past, Present and Future"
category: Users/kz/papers
tags: survey, Re-ID, paper
created_at: 2018-08-08 21:50:31 +0900
updated_at: 2018-08-20 21:53:49 +0900
published: false
number: 12
---

# [Person Re-Identification: Past, Present and Future(2015)](https://arxiv.org/abs/1610.02984)

2015年時点でのRe-ID系のサーベイ論文
とりあえずこれを読むことにした。

# Abstract
### early days
hand-crafted algorithms and small-scale datasets  
### recent years
deep learning algorithms and learge-scale datasets
### ID-methods
1.image-based
2.video-based
どちらにも共通することは実用に向けたような議論がされている。
(end-to-end re-ID, fast re-ID in very large galleriesなど)

### what is discussed in this paper....
* introduces the history of person re-ID and its relationship with image classification and instance retrieval(instance retrieval 人物検索っぽい)
* surveys a broad selection of the hand-crafted systems and the large-scale methods in both image- and video-based re-ID
* describes critical future directions in end-to-end re-ID and fast retrieval in large galleries
* briefs some important yet under-developed isshues

# Introduction

person Re-IDとある人がほかのカメラでも移っているのかどうかを判定する
１）公共の安全のために必要
２）テーマパークや大学や道路などたくさんのカメラネットワークの場所において必要

person Re-IDというのは技術的には３つの部分に分かれる
①person detection(人物検知)
②person tracking(人物追跡)
③person retrieval(人物検索)

①②はcomputer visionにおいて独立したタスクとして研究されているので
person Re-IDというと③にフォーカスしていることが多い
本サーベイでも③にフォーカスしてやる。

Re-IDのもっとも難しいのは照明や、姿勢、視点などが違う２つ画像から、同一人物が写っていると判断させること。


### A brief History of Person Re-ID
#### Multi-camera tracking
始まりはmulti-camera tracking(1997 Human and Russell)  
*  Huang and Russel,1997  
 あるカメラである物体が観測された画像をもとに、他のカメラでその物体が出現しているかどうかを予測することをベイズを用いて定式化した。  

#### Multi-camera tracking with explicit "re-identification"
* (Wojciech Zajdel, Zoran Zivkovic and BenJ.A.Krose, 2005)  
"Person re-identification appears in multi-cam tracking"ではじめて"person re-identification"という用語が用いられる
* この時期の論文はp.2黄色部分訳す。

#### The independence of re-ID(image-based) 
* (Gheissari,2006)
背景でない部分をセグメントした後のvisual cues only (画像だけで)解いた(うえで画像ではない何を使っていたかを整理して表にする？)  
→純粋なcomputer vision taskとしてPerson Re-IDが初めて行われた
(visual matchingはcolor and salient edgel histogramをベースに？)

#### the video-based re-ID  
* 2010年にmulti-frame
* (12,2010)
* (13,2010)
* 
* deep learning for re-ID
* End-to-end image-based re-ID



