# BirdClef 2023

## **比赛背景**

鸟类是生物多样性变化的极好指标，因为它们具有高度流动性并且具有多样化的栖息地要求。因此物种组合和鸟类数量的变化可以表明恢复项目的成功或失败。然而经常对大面积进行传统的基于观察者的鸟类生物多样性调查是昂贵的，而且在后勤方面具有挑战性。

## **比赛任务**

对于本次比赛，您将使用您的机器学习技能通过声音识别东非鸟类。具体来说，您将开发计算解决方案来处理连续的音频数据并通过它们的叫声识别物种。

## **数据描述**

- train_audio/训练数据，由 xenocanto.org 用户上传的个别鸟类叫声的简短录音组成。
- test_soundscapes/当您提交笔记本时test_soundscapes 目录将填充大约 200 条用于评分的录音。它们时长 10 分钟，采用 ogg 音频格式。
- train_metadata.csv训练数据的元数据
- sample_submission.csv提交样例

## 机器学习说明

- 注意label保持一致，由于**os.listdir**命令在Windows下和Linux下的返回的文件名顺序不一致，label基于index进行映射就会导致label不一致从而使得模型的acc为0。

​		解决方案也很简单：Sort排序。

- 只采取了MCFF一个特征，使用简单的CNN模型，可以到达0.9的准确度。

- 音频可以绘制如下图：

  - 幅度和时间关系图
  - 频率和幅度关系图
  - 频率和时间、幅度图，使用颜色表示幅度，也叫语谱图。

- 常用特征：

  - 梅尔频率倒谱系数(**MCFF**),提取过程比较复杂。

    ![img](https://ylifs-1259594924.cos.ap-guangzhou.myqcloud.com/imgs/202303181511973.webp)

  - 过零率

## 提交注意事项

这个地方是最坑的，官方说明隐藏的内容太多了，需要参照其他的提交文件。

- 提交不能使用GPU，不能联网。
- 通过引用外部数据集的方式调用自己训练的模型，由于不能使用GPU，从0开始训练一个模型时间上是不可能的，因为提交限时120分钟。
- 提交最终是一个CSV文件，每行是每个类别的概率输出。
- 当提交时，推荐保存**Version**再提交，这时候测试文件夹将会填充200多个10分钟的语音文件，需要判断每5秒中是否有鸟叫，如果有是哪种鸟，**row_id**就是文件名+秒数。
- 建议Kaggle比赛先搞明白怎么提交，每个比赛的提交方式都不一致。

## 本项目的提交思路

**submit.ipynb**为此项目的提交文件。遵循以下流程：

- 加载模型
- 读取录音文件
- 修改录音的采样率，读取为numpy数组，然后分割成120段。
- 提取每一段的**MCFF**特征，再填充到另外一个数组。
- 将数组提交模型进行推理
- 将推理结果SoftMax再依次录入CSV

## 提交结果

![img](https://ylifs-1259594924.cos.ap-guangzhou.myqcloud.com/imgs/202303181512427.png)