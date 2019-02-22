---
layout: post
title: "Build TF on win"
tags: [tensorflow, other]
date: 2018-06-23 00:00:00
comments: true
---  

## 准备工作

win：windows server 2016（win10应该也没问题，成功build和系统关系不大）

cMake：latest

TF源码：master -> 1.10（until 2018.9.1）

eigen：latest

python：3.5.0

visual studio：2015

（[可供参考的视频](https://www.youtube.com/watch?v=gj_4Yv94LgQ)）

<!--more-->

## 1. build tf   

1. 终端进入TensorFlow的cmake文件夹,例如`E:\tf-r.1.10\tensorflow\tensorflow\contrib\cmake`

2. 新建一个文件夹并进入`build`

3. 执行：

   ```powershell
   cmake .. -A x64 -DCMAKE_BUILD_TYPE=Release ^ 
   More? -DSWIG_EXECUTABLE=E:/swigwin-3.0.12/swig.exe ^ 
   More? -DPYTHON_EXECUTABLE=C:/Users/yanfu/AppData/Local/Programs/Python/Python35/python.exe ^
   More? -DPYTHON_LIBRARIES=C:/Users/yanfu/AppData/Local/Programs/Python/Python35/libs/python35.lib ^
   More? -Dtensorflow_BUILD_SHARED_LIB=ON
   ```

4. MSBuild：

   ```powershell
   MSBuild ^
   /m:1 ^
   /p:CL_MPCount=1 ^
   /p:Configuration=Release ^
   /p:Platform=x64 ^
   /p:PreferredToolArchitecture=x64 ALL_BUILD.vcxproj ^
   /filelogger
   ```

5. PS.MSBuild 是增量build的，期间很可能部分项目会有提示内存不足的错误，多按照上面的MSBuild命令build几次直到0error，build完成大概需要2~3小时，这个得看机器内存和处理器。完成后在以下目录`E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\Release  `生成`tensorflow.dll, tensorflow.lib`，表示build完成；

   1. 执行`Release\tf_tutorials_example_trainer.exe  `检验是否build成功；
   2. 执行`Release\tf_label_image_example.exe`进一步检验是否build成功，这一步检验需要model，dic文件，可在[此处](https://github.com/firdauslubis88/tensorflow/tree/master/build/tensorflow/examples/label_image/data)下载到，放置在`E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\tensorflow\examples\label_image\data `即可。

## 2. New VS C++ project

用VS2015新建空的visual C++ 项目（**选择为Release，x64**），右键项目修改properties：

1. 修改C/C++下的`Additional Include Directories，`添加:

   ```
   E:\tf-r1.10\tensorflow
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\eigen_archive
   E:\tf-r1.10\tensorflow\third_party\eigen3
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\protobuf\src\protobuf\src
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\nsync\public
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\zlib_archive
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\gif_archive\giflib-5.1.4
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\png_archive
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\jpeg_archive
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\lmdb
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\gemmlowp\src\gemmlowp
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\jsoncpp\src\jsoncpp
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\farmhash_archive
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\farmhash_archive\util
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\highwayhash
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\cub\src\cub
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\re2\install\include
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\sqlite
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\grpc\src\grpc\include
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\snappy\src\snappy
   ```

2. 修改C/C++的`Preprocesser Definitions`,添加：

   ```
   COMPILER_MSVC
   NOMINMAX
   TENSORFLOW_EXPORTS
   ```

3. 修改Linker的Input下的`Additional Dependencies`，添加`Addtional Denpendencies`:

   ```
   E:\tf-r1.10\tensorflow
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\eigen_archive
   E:\tf-r1.10\tensorflow\third_party\eigen3
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\protobuf\src\protobuf\src
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\nsync\public
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\zlib_archive
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\gif_archive\giflib-5.1.4
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\png_archive
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\jpeg_archive
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\lmdb
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\gemmlowp\src\gemmlowp
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\jsoncpp\src\jsoncpp
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\farmhash_archive
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\farmhash_archive\util
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\highwayhash
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\cub\src\cub
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\re2\install\include
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\external\sqlite
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\grpc\src\grpc\include
   E:\tf-r1.10\tensorflow\tensorflow\contrib\cmake\build\snappy\src\snappy
   ```

4. 在`main.cpp`下测试官方的测试代码：

   ```CPP
   #include "tensorflow/cc/client/client_session.h"
   #include "tensorflow/cc/ops/standard_ops.h"
   #include "tensorflow/core/framework/tensor.h"
   
   int main() {
       using namespace tensorflow;
       using namespace tensorflow::ops;
       
       Scope root = Scope::NewRootScope();
       // Matrix A = [3 2; -1 0]
       auto A = Const(root, { { 3.f, 2.f },{ -1.f, 0.f } });
       // Vector b = [3 5]
       auto b = Const(root, { { 3.f, 5.f } });
       // v = Ab^T
       auto v = MatMul(root.WithOpName("v"), A, b, MatMul::TransposeB(true));
       std::vector<Tensor> outputs;
       ClientSession session(root);
       // Run and fetch v
       TF_CHECK_OK(session.Run({ v }, &outputs));
       // Expect outputs[0] == [19; -3]
       LOG(INFO) << outputs[0].matrix<float>();
       return 0;
   }
   ```

5. 编译项目，然后在`C:\Users\yanfu\Documents\Visual Studio 2015\Projects\tensorflow-native-test\x64\Release `下用终端直接执行`tensorflow-native-test.exe`：

   ```
   2018-09-02 08:03:49.370879: I E:\tf-r1.10\tensorflow\tensorflow\core\platform\cpu_feature_guard.cc:141] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX AVX2
   2018-09-02 08:03:49.382443: I main.cpp:20] 19
   -3
   ```

   表示C++项目已经可以正确使用编译出来的TF库了。

#### Ps.Python生产model文件，在C++项目使用  

最开始的目的是想用 python去生产TF的pb格式（在这个需求场景下推荐这个格式）的model文件，然后在C++的项目中使用，但是后来项目不需要TF的model了，所以就放弃了。python导出pb格式的文件的方法为：

```python
import tensorflow as tf
import os
from tensorflow.python.framework import graph_util

with tf.Session(graph=tf.Graph()) as sess:
    x = tf.placeholder(tf.int32, name='x')
    y = tf.constant([666], name='y')
    w = tf.Variable(0, name='w')
    _op = tf.multiply(x, w)
    op = tf.add(_op, y, name='op_to_store')

    sess.run(tf.global_variables_initializer())

    # convert_variables_to_constants 需要指定output_node_names，list()，可以多个
    constant_graph = graph_util.convert_variables_to_constants(
        sess, sess.graph_def, ['op_to_store'])

    # 测试 OP
    feed_dict = {x: 10}
    print(sess.run(op, feed_dict))

    # 写入序列化的 PB 文件
    with tf.gfile.FastGFile('./model.pb', mode='wb') as f:
        f.write(constant_graph.SerializeToString())
```

