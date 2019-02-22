---
layout: post
title: "Kaggle - MNIST手写体识别"
tags: [ml, kaggle]
comments: true
date: 2016-10-24 00:00:00
---

## 0 加载所需用的库

```python
import tensorflow as tf
import numpy as np
import pandas as pd
```  

<!--more-->  

## 1 数据整理  
label的`one_hot`编码、归一化并将train数据集后20%作为测试集：

```python
### data ###

train_df = pd.read_csv("/Volumes/X-leisure/Kaggle/MNIST识别/train.csv")

# normalization
# normal_train = train_df.ix[:,1:].apply(lambda x:x/255,axis=0) # more elegent
normal_train = train_df.ix[:,1:].div(255)

# label's one-hot encoding
train_dummies_label = pd.get_dummies(train_df['label'])
train_oneHot_df = pd.concat([train_df['label'],train_dummies_label],axis=1)

new_train_data = pd.concat([train_dummies_label,normal_train],axis=1).astype('float32')
# save the one-hot label and normalized data
# new_train_data.to_csv('/Volumes/X-leisure/Kaggle/MNIST识别/new_train_data.csv', index=False)
# new_train_data = pd.read_csv('/Volumes/X-leisure/Kaggle/MNIST识别/new_train_data.csv')

# dataframe to ndarray
test_images = new_train_data.ix[33900:,10:].values
test_labels = new_train_data.ix[33900:,:10].values
```  
## 2 tensorflow模型建立
假设函数采用softmax，loss采用cross_entropy：  

```py
### model ###
def add_layer(inputs, in_size, out_size, activaton_function=None,name_W='Weights',name_b='biases'):
    global W, b
    W = tf.Variable(tf.random_normal([in_size, out_size]), dtype=tf.float32, name=name_W)
    b = tf.Variable(tf.zeros([1, out_size]), dtype=tf.float32, name=name_b)
    Wx_plus_b = tf.matmul(inputs, W) + b
    if activaton_function is None:
        outputs = Wx_plus_b
    else:
        outputs = activaton_function(Wx_plus_b)
    return outputs


def calc_accuracy(v_xs, v_ys):
    global y_pre
    y_pre_value = sess.run(y_pre, feed_dict={x: v_xs})
    correct_prediction = tf.equal(
        tf.argmax(y_pre_value, 1), tf.argmax(v_ys, 1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    result = sess.run(accuracy, feed_dict={x: v_xs, y: v_ys})
    return result


# define placeholder
x = tf.placeholder(tf.float32, [None, 784])
y = tf.placeholder(tf.float32, [None, 10])
y_pre = add_layer(x, 784, 10, activaton_function=tf.nn.softmax)

# the error between prediction and real data
cross_entropy = tf.reduce_mean(-tf.reduce_sum(y * tf.log(y_pre), reduction_indices=[1]))
train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)
```  
## 3 模型的训练及保存  

```py
%%time
sess = tf.Session()
sess.run(tf.initialize_all_variables())
saver = tf.train.Saver()

for i in range(10000):
    if i%340 == 0:
        continue
    else:
        batch_xs = new_train_data.ix[(i-1)%340*100:i%340*100,10:].values
        batch_ys = new_train_data.ix[(i-1)%340*100:i%340*100,:10].values
#     if i%421 == 0:
#         continue
#     else:
#         batch_xs = new_train_data.ix[(i-1)%421*100:i%421*100,10:].values
#         batch_ys = new_train_data.ix[(i-1)%421*100:i%421*100,:10].values
        sess.run(train_step, feed_dict={x: batch_xs, y: batch_ys})
        if i % 1000 == 0:
            now_accuracy = calc_accuracy(test_images, test_labels)
            print('train_step: %s   now_accuracy: %s'%(i,now_accuracy))


save_path = saver.save(sess, '/Users/fuyangzhen/Desktop/kaggle_MNIST_net.ckpt')
print('Save file to path:', save_path)
```
结果如下：  

```py
train_step: 1000   now_accuracy: 0.877901
train_step: 2000   now_accuracy: 0.897284
train_step: 3000   now_accuracy: 0.906173
train_step: 4000   now_accuracy: 0.912099
train_step: 5000   now_accuracy: 0.917531
train_step: 6000   now_accuracy: 0.915309
train_step: 7000   now_accuracy: 0.917531
train_step: 8000   now_accuracy: 0.92284
train_step: 9000   now_accuracy: 0.923951
Save file to path: /Users/fuyangzhen/Desktop/kaggle_MNIST_net.ckpt
CPU times: user 35.3 s, sys: 3.22 s, total: 38.5 s
Wall time: 29.1 s
```  
## 4 预测test集合  
```py
### test ###
test_df = pd.read_csv("/Volumes/X-leisure/Kaggle/MNIST识别/test.csv")
test_val = test_df.div(255).values
res = sess.run(y_pre, feed_dict={x: test_val})
result = sess.run(tf.argmax(res, 1))
result_df = pd.DataFrame(np.array([np.arange(1,28001),result]).T, columns=['ImageId','Label'])
result_df.head()
result_df.to_csv('/Volumes/X-leisure/Kaggle/MNIST识别/result1.csv',index = False)
```  
### Ps.加载已保存模型继续使用  

```py
### saver restore ### 
# W = tf.Variable(tf.zeros([784, 10]), dtype=tf.float32, name='Weights')
# b = tf.Variable(tf.zeros([1, 10]), dtype=tf.float32, name='biases')
# saver = tf.train.Saver()
# with tf.Session() as sess:
#     saver.restore(sess,'/Users/fuyangzhen/Desktop/kaggle_MNIST_net.ckpt')
#     print('W:',sess.run(W))
#     print('b:',sess.run(b))
```