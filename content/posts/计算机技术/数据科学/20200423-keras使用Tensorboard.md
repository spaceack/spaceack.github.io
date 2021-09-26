---
title: keras使用tensorboard
author: Spaceack
date: 2020-04-23 21:00:00
update:  2020-04-23  21:00:00
categories: ["数据处理"]
tags: 
  - tensorboard
  - keras
---

``` python
# 导入库
from keras.callbacks import TensorBoard
# 创建tensorboard对象， 结果保存在logs目录下
tensorboard = TensorBoard(log_dir='logs/{}'.format(NAME),histogram_freq=1,write_grads=True)
# 在模型生成器函数作为回调参数

model.fit_generator(
        generator=train_generator,
        epochs=100,
        steps_per_epoch=2200 // BATCH_SIZE,
        validation_data=validation_generator,
        validation_steps=200 // BATCH_SIZE,
        callbacks=[tensorboard]
    )

```

在浏览器中展示数据
```bash
tensorboard --logdir=logs
```