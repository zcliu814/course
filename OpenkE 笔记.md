# OpenKe教程 或者直接在github写  markdown说明文件？

安装

1. 安装 TensorFlow 或 PyTorch
2. 克隆 OpenKE 存储库

```
$  git clone https://github.com/thunlp/OpenKE

$ cd OpenKE
```

1. 编译C++文件

```
$ bash make.sh
```



# 数据

| 对于训练，数据集包含三个文件：                       |                                                              |
| :--------------------------------------------------- | ------------------------------------------------------------ |
|                                                      | `train2id.txt`：训练文件，第一行是训练的三元组数。然后以下行都是格式（e1，e2，rel）。`entity2id.txt`：所有实体和相应的 ID，每行一个。第一行是实体数。`relation2id.txt`：所有关系和相应的 ID，每行一个。第一行是关系数。 |
| 对于测试，数据集包含额外的两个文件（总共五个文件）： |                                                              |
|                                                      | `test2id.txt`：测试文件，第一行是用于测试的三元组数。然后以下行都是格式（e1，e2，rel）。`valid2id.txt`：验证文件，第一行是用于验证的三元组数。然后以下行都是格式（e1，e2，rel）。 |



# 如何训练

要计算知识图谱嵌入，请先导入数据集并设置训练配置参数，然后训练模型并导出结果。例如，我们编写一个example_train_transe.py来训练TransE：

```
import config
import numpy as np

con = config.Config()
#Input training files from benchmarks/FB15K/ folder.
con.set_in_path("./benchmarks/FB15K/")

con.set_work_threads(4)
con.set_train_times(500)
con.set_nbatches(100)
con.set_alpha(0.001)
con.set_margin(1.0)
con.set_bern(0)
con.set_dimension(50)
con.set_ent_neg_rate(1)
con.set_rel_neg_rate(0)
con.set_opt_method("SGD")

#Models will be exported via torch.save() automatically.
con.set_export_files("./res/model.vec.pt")
#Model parameters will be exported to json files automatically.
con.set_out_files("./res/embedding.vec.json")
#Initialize experimental settings.
con.init()
#Set the knowledge embedding model
con.set_model(models.TransE)
#Train the model.
con.run()
```

## 步骤 1：导入数据集

我们设置数据集的路径：

```
con.set_in_path("benchmarks/FB15K/")
```

我们从基准/FB15K/文件夹导入知识图谱。数据由前面提到的三个基本文件组成

> - `train2id.txt`
> - `entity2id.txt`
> - `relation2id.txt`

验证和测试文件是必需的，用于评估训练结果，但是，它们对于训练并非必不可少：

```
con.set_work_threads(8)
```

## 步骤二：设置配置参数

我们设置参数，包括数据遍历轮次、学习率、批量大小以及实体和关系嵌入的维度：

```
con.set_train_times(500)
con.set_nbatches(100)
con.set_alpha(0.5)
con.set_dimension(200)
con.set_margin(1)
```

对于负抽样，我们可以破坏实体和关系来构造负三元组。 将使用传统的抽样方法，并将使用（Wang等人，2014）中表示为“bern”的方法。`set_bern(0)``set_bern(1)`

我们可以选择合适的梯度下降优化算法来训练模型：

```
con.set_optimizer("SGD")
```

## 步骤 3：导出结果

模型将每隔几轮自动导出一次。此外，模型参数最终将导出到 json 文件：`torch.save()`

```
con.set_export_files("./res/model.vec.pt")

con.set_out_files("./res/embedding.vec.json")
```

## 步骤 4：训练模型

我们设置知识图谱嵌入模型并开始训练过程：

```
con.init()
con.set_model(models.TransE)
con.run()
```



# 如何测试

要评估模型，请先导入数据集并设置基本配置参数，然后设置模型参数并测试模型。例如，我们编写一个example_test_transe.py来测试 TransE。

测试模型有三种方法。

## 从导入文件加载模型

设置导入文件，OpenKE-PyTorch 将通过 torch.load（） 自动加载模型：

```
import config
import models
import numpy as np
import json

con = config.Config()
con.set_in_path("./benchmarks/FB15K/")
con.set_test_flag(True)
con.set_work_threads(4)
con.set_dimension(100)
con.set_import_files("./res/model.vec.pt")
con.init()
con.set_model(models.TransE)
icon.test()
```

## 从 JSON 文件中读取模型参数

从 json 文件中读取模型参数并手动加载参数：

```
import config
import models
import numpy as np
import json

con = config.Config()
con.set_in_path("./benchmarks/FB15K/")
con.set_test_flag(True)
con.set_work_threads(4)
con.set_dimension(100)
con.init()
con.set_model(models.TransE)
f = open("./res/embedding.vec.json", "r")
content = json.loads(f.read())
f.close()
con.set_parameters(content)
con.test()
```

## 手动加载模型

通过 torch.load（） 手动加载模型：

```
import config
import models
import numpy as np
import json

con = config.Config()
con.set_in_path("./benchmarks/FB15K/")
con.set_test_flag(True)
con.set_work_threads(4)
con.set_dimension(100)
con.init()
con.set_model(models.TransE)
con.import_variables("./res/model.vec.pt")
con.test()
```

# 如何获取嵌入矩阵

有四种方法可以获取嵌入矩阵。

## 设置导入文件

设置导入文件，OpenKE将通过torch.load（）自动加载模型：

```
import json
import numpy as py

import config
import models
con = config.Config()
con.set_in_path("./benchmarks/FB15K/")
con.set_test_flag(True)
con.set_work_threads(4)
con.set_dimension(100)
con.set_import_files("./res/model.vec.pt")
con.init()
con.set_model(models.TransE)
# Get the embeddings (numpy.array)
embeddings = con.get_parameters("numpy")
# Get the embeddings (python list)
embeddings = con.get_parameters()
```

## 从 JSON 文件中读取参数

从 json 文件中读取模型参数并手动加载参数：

```
import json
import numpy as py
import config
import models
con = config.Config()
con.set_in_path("./benchmarks/FB15K/")
con.set_test_flag(True)
con.set_work_threads(4)
con.set_dimension(100)
con.init()
con.set_model(models.TransE)
f = open("./res/embedding.vec.json", "r")
embeddings = json.loads(f.read())
f.close()
```

## 手动加载模型参数

通过以下方式手动加载模型：`torch.load()`

```
con = config.Config()
con.set_in_path("./benchmarks/FB15K/")
con.set_test_flag(True)
con.set_work_threads(4)
con.set_dimension(100)
con.init()
con.set_model(models.TransE)
con.import_variables("./res/model.vec.pt")
# Get the embeddings (numpy.array)
embeddings = con.get_parameters("numpy")
# Get the embeddings (python list)
embeddings = con.get_parameters()
```

## 训练后获取嵌入

训练模型后立即获取嵌入：

```
...
...
...
#Models will be exported via tf.Saver() automatically.
con.set_export_files("./res/model.vec.pt")
#Model parameters will be exported to json files automatically.
con.set_out_files("./res/embedding.vec.json")
#Initialize experimental settings.
con.init()
#Set the knowledge embedding model
con.set_model(models.TransE)
#Train the model.
con.run()
#Get the embeddings (numpy.array)
embeddings = con.get_parameters("numpy")
#Get the embeddings (python list)
embeddings = con.get_parameters()
```