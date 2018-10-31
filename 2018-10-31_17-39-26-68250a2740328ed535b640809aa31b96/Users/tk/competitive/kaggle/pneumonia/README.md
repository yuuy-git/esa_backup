---
title: "README"
category: Users/tk/competitive/kaggle/pneumonia
tags: 
created_at: 2018-09-12 09:14:40 +0900
updated_at: 2018-09-17 10:49:49 +0900
published: true
number: 65
---

# Competition
- [RSNA Pneumonia Detection Challenge](https://www.kaggle.com/c/rsna-pneumonia-detection-challenge)

##Schedule
October 17, 2018 - Entry deadline. You must accept the competition rules before this date in order to compete.

October 17, 2018 - Team Merger deadline. This is the last day participants may join or merge teams.

October 24, 2018 - Stage 1 ends & Model upload deadline*.

October 25, 2018 - Stage 2 begins. New test set uploaded.

October 31, 2018 - Stage 2 ends & Final submission deadline.

November 9, 2018 - Solutions & Other Winners Obligations due from winners.

November 25-30, 2018 - RSNA 2018 Conference in Chicago, IL

- * In order to be eligible for Stage 2, each team's Stage 1 submission must include the model uploaded, via Team -> Your Model, per the Competition Rules. This model should match that which was used to generate the 1 final submission selected for scoring. Be aware that if you do not select a final submission (via 'My Submissions'), the platform will auto-select your best-scoring model on the Stage 1 public leaderboard. The deadline for model upload is firmly the end of Stage 1.

This requirement is in place for the host team to verify the performance of the uploaded models matches the Stage 2 submission file. Compliance with the above will be verified by the host team. Submitters who fail to upload their model by the Stage 1 deadline, or are found not to be in compliance, may be disqualified from Stage 2 and removed from the final leaderboard.

All deadlines are at 11:59 PM UTC on the corresponding day unless otherwise noted. The competition organizers reserve the right to update the contest timeline if they deem it necessary.


## General
> In this competition, you’re challenged to build an algorithm to detect a visual signal for pneumonia in medical images. **Specifically, your algorithm needs to automatically locate lung opacities on chest radiographs.**

肺炎は特に子供で死亡者が多いのに正確な診断をするのが難しい。
CXR(chest radiograph)で不透明な部分を見て肺炎か判断するが、それには熟練の医師に判断を仰ぐ必要があり、肺に水が溜まっている、出血している、肺がんである、などの他の要因と区別するのが難しい。
可能であれば、同じ患者のパネルデータを比較すると診断に役立つ。

CXRは他の診断でも広く使われており、医療機関では常に大量のCXRを処理している。
この作業の効率化と診察数を増やすために多方面と協力してデータセットを揃えてカグルのコンペを開催した。

コンペの入賞者はは11月25〜30日にシカゴで開かれるセレモニーに参加してモデルや手法の説明をする必要がある。

## Data
###What files do I need?
This is a two-stage challenge. You will need the images for the current stage - provided as **stage_1_train_images.zip** and **stage_1_test_images.zip**. You will also need the training data - **stage_1_train_labels.csv** - and the sample submission **stage_1_sample_submission.csv**, which provides the IDs for the test set, as well as a sample of what your submission should look like. The file **stage_1_detailed_class_info.csv** contains detailed information about the positive and negative classes in the training set, and may be used to build more nuanced models.

###What should I expect the data format to be?
**The training data is provided as a set of `patientIds` and bounding boxes. Bounding boxes are defined as follows: `x-min y-min width height`**

**There is also a binary target column, `Target`, indicating pneumonia or non-pneumonia.**

**There may be multiple rows per `patientId`.**

###DICOM Images
All provided images are in DICOM format.

###What am I predicting?
In this challenge competitors are predicting whether pneumonia exists in a given image. They do so by predicting bounding boxes around areas of the lung. Samples without bounding boxes are negative and contain no definitive evidence of pneumonia. Samples with bounding boxes indicate evidence of pneumonia.

When making predictions, competitors should predict as many bounding boxes as they feel are necessary, in the format: `confidence x-min y-min width height`

**There should be only ONE predicted row per image. This row may include multiple bounding boxes.**

A properly formatted row may look like any of the following.

For patientIds with no predicted pneumonia / bounding boxes: `0004cfab-14fd-4e49-80ba-63a80b6bddd6`,

For patientIds with a single predicted bounding box: `0004cfab-14fd-4e49-80ba-63a80b6bddd6,0.5 0 0 100 100`

For patientIds with multiple predicted bounding boxes: `0004cfab-14fd-4e49-80ba-63a80b6bddd6,0.5 0 0 100 100 0.5 0 0 100 100`, etc.

###File descriptions
**stage_1_train.csv** - the training set. Contains `patientId`s and bounding box / target information.
**stage_1_sample_submission.csv** - a sample submission file in the correct format. Contains `patientId`s for the test set. Note that the sample submission contains one box per image, but there is no limit to the number of bounding boxes that can be assigned to a given image.
**stage_1_detailed_class_info.csv** - provides detailed information about the type of positive or negative class for each image.

###Data fields
- **patientId _**- A `patientId`. Each `patientId` corresponds to a unique image.
- **x_** - the upper-left `x` coordinate of the bounding box.
- **y_** - the upper-left `y` coordinate of the bounding box.
- **width_** - the `width` of the bounding box.
- **height_** - the `height` of the bounding box.
- **Target_** - the binary `Target`, indicating whether this sample has evidence of pneumonia.
