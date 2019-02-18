# 内置 Tuner

NNI 提供了先进的调优算法，使用上也很简单。 下面是内置 Tuner 的简单介绍：

注意：点击 **Tuner 的名称**可跳转到算法的详细描述，点击**用法**可看到 Tuner 的安装要求、建议场景和使用样例等等。

| Tuner                                                                                                                                          | 算法简介                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [TPE](https://github.com/Microsoft/nni/blob/master/zh_CN/src/sdk/pynni/nni/hyperopt_tuner/README.md) [(用法)](#TPE)                              | Tree-structured Parzen Estimator (TPE) 是一种 sequential model-based optimization（SMBO，即基于序列模型优化）的方法。 SMBO 方法根据历史指标数据来按顺序构造模型，来估算超参的性能，随后基于此模型来选择新的超参。 [参考论文](https://papers.nips.cc/paper/4443-algorithms-for-hyper-parameter-optimization.pdf)                                                                                                                                                                                                                                                                                                          |
| [Random Search](https://github.com/Microsoft/nni/blob/master/zh_CN/src/sdk/pynni/nni/hyperopt_tuner/README.md) [(用法)](#Random)                 | 在超参优化时，随机搜索算法展示了其惊人的简单和效果。 建议当不清楚超参的先验分布时，采用随机搜索作为基准。 [参考论文](http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf)                                                                                                                                                                                                                                                                                                                                                                                                                          |
| [Anneal](https://github.com/Microsoft/nni/blob/master/zh_CN/src/sdk/pynni/nni/hyperopt_tuner/README.md) [(用法)](#Anneal)                        | 这种简单的退火算法从先前的采样开始，会越来越靠近发现的最佳点取样。 此算法是随机搜索的简单变体，利用了反应曲面的平滑性。 退火率不是自适应的。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| [Naive Evolution](https://github.com/Microsoft/nni/blob/master/zh_CN/src/sdk/pynni/nni/evolution_tuner/README.md) [(用法)](#Evolution)           | 朴素进化算法来自于大规模图像分类进化。 它会基于搜索空间随机生成一个种群。 在每一代中，会选择较好的结果，并对其下一代进行一些变异（例如，改动一个超参，增加或减少一层）。 进化算法需要很多次 Trial 才能有效，但它也非常简单，也很容易扩展新功能。 [参考论文](https://arxiv.org/pdf/1703.01041.pdf)                                                                                                                                                                                                                                                                                                                                                                             |
| [SMAC](https://github.com/Microsoft/nni/blob/master/zh_CN/src/sdk/pynni/nni/smac_tuner/README.md) [(用法)](#SMAC)                                | SMAC 基于 Sequential Model-Based Optimization (SMBO，即序列的基于模型优化方法)。 它会利用使用过的结果好的模型（高斯随机过程模型），并将随机森林引入到 SMBO 中，来处理分类参数。 SMAC 算法包装了 Github 的 SMAC3。 注意：SMAC 需要通过 `nnictl package` 命令来安装。 [参考论文，](https://www.cs.ubc.ca/~hutter/papers/10-TR-SMAC.pdf) [Github 代码库](https://github.com/automl/SMAC3)                                                                                                                                                                                                                                                         |
| [Batch tuner](https://github.com/Microsoft/nni/blob/master/zh_CN/src/sdk/pynni/nni/batch_tuner/README.md) [(用法)](#Batch)                       | Batch tuner allows users to simply provide several configurations (i.e., choices of hyper-parameters) for their trial code. After finishing all the configurations, the experiment is done. Batch tuner only supports the type choice in search space spec.                                                                                                                                                                                                                                                                                            |
| [Grid Search](https://github.com/Microsoft/nni/blob/master/src/sdk/pynni/nni/gridsearch_tuner/README.md) [(Usage)](#GridSearch)                | Grid Search performs an exhaustive searching through a manually specified subset of the hyperparameter space defined in the searchspace file. Note that the only acceptable types of search space are choice, quniform, qloguniform. The number q in quniform and qloguniform has special meaning (different from the spec in search space spec). It means the number of values that will be sampled evenly from the range low and high.                                                                                                               |
| [Hyperband](https://github.com/Microsoft/nni/blob/master/src/sdk/pynni/nni/hyperband_advisor/README.md) [(Usage)](#Hyperband)                  | Hyperband tries to use the limited resource to explore as many configurations as possible, and finds out the promising ones to get the final result. The basic idea is generating many configurations and to run them for the small number of STEPs to find out promising one, then further training those promising ones to select several more promising one.[Reference Paper](https://arxiv.org/pdf/1603.06560.pdf)                                                                                                                                 |
| [Network Morphism](https://github.com/Microsoft/nni/blob/master/src/sdk/pynni/nni/networkmorphism_tuner/README.md) [(Usage)](#NetworkMorphism) | Network Morphism provides functions to automatically search for architecture of deep learning models. Every child network inherits the knowledge from its parent network and morphs into diverse types of networks, including changes of depth, width, and skip-connection. Next, it estimates the value of a child network using the historic architecture and metric pairs. Then it selects the most promising one to train. [Reference Paper](https://arxiv.org/abs/1806.10282)                                                                     |
| [Metis Tuner](https://github.com/Microsoft/nni/blob/master/src/sdk/pynni/nni/metis_tuner/README.md) [(Usage)](#MetisTuner)                     | Metis offers the following benefits when it comes to tuning parameters: While most tools only predict the optimal configuration, Metis gives you two outputs: (a) current prediction of optimal configuration, and (b) suggestion for the next trial. No more guesswork. While most tools assume training datasets do not have noisy data, Metis actually tells you if you need to re-sample a particular hyper-parameter. [Reference Paper](https://www.microsoft.com/en-us/research/publication/metis-robustly-tuning-tail-latencies-cloud-systems/) |

<br />

## Usage of Builtin Tuners

Use builtin tuner provided by NNI SDK requires to declare the **builtinTunerName** and **classArgs** in `config.yml` file. In this part, we will introduce the detailed usage about the suggested scenarios, classArg requirements and example for each tuner.

Note: Please follow the format when you write your `config.yml` file. Some builtin tuner need to be installed by `nnictl package`, like SMAC.

<a name="TPE"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `TPE`

> Builtin Tuner Name: **TPE**

**Suggested scenario**

TPE, as a black-box optimization, can be used in various scenarios and shows good performance in general. Especially when you have limited computation resource and can only try a small number of trials. From a large amount of experiments, we could found that TPE is far better than Random Search.

**Requirement of classArg**

* **optimize_mode** (*maximize or minimize, optional, default = maximize*) - If 'maximize', tuners will return the hyperparameter set with larger expectation. If 'minimize', tuner will return the hyperparameter set with smaller expectation.

**Usage example:**

```yaml
# config.yml
tuner:
  builtinTunerName: TPE
  classArgs:
    optimize_mode: maximize
```

<br />

<a name="Random"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `Random Search`

> Builtin Tuner Name: **Random**

**Suggested scenario**

Random search is suggested when each trial does not take too long (e.g., each trial can be completed very soon, or early stopped by assessor quickly), and you have enough computation resource. Or you want to uniformly explore the search space. Random Search could be considered as baseline of search algorithm.

**Requirement of classArg:**

* **optimize_mode** (*maximize or minimize, optional, default = maximize*) - If 'maximize', tuners will return the hyperparameter set with larger expectation. If 'minimize', tuner will return the hyperparameter set with smaller expectation.

**Usage example**

```yaml
# config.yml
tuner:
  builtinTunerName: Random
  classArgs:
    optimize_mode: maximize
```

<br />

<a name="Anneal"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `Anneal`

> Builtin Tuner Name: **Anneal**

**Suggested scenario**

Anneal is suggested when each trial does not take too long, and you have enough computation resource(almost same with Random Search). Or the variables in search space could be sample from some prior distribution.

**Requirement of classArg**

* **optimize_mode** (*maximize or minimize, optional, default = maximize*) - If 'maximize', tuners will return the hyperparameter set with larger expectation. If 'minimize', tuner will return the hyperparameter set with smaller expectation.

**Usage example**

```yaml
# config.yml
tuner:
  builtinTunerName: Anneal
  classArgs:
    optimize_mode: maximize
```

<br />

<a name="Evolution"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `Naive Evolution`

> Builtin Tuner Name: **Evolution**

**Suggested scenario**

Its requirement of computation resource is relatively high. Specifically, it requires large initial population to avoid falling into local optimum. If your trial is short or leverages assessor, this tuner is a good choice. And, it is more suggested when your trial code supports weight transfer, that is, the trial could inherit the converged weights from its parent(s). This can greatly speed up the training progress.

**Requirement of classArg**

* **optimize_mode** (*maximize or minimize, optional, default = maximize*) - If 'maximize', tuners will return the hyperparameter set with larger expectation. If 'minimize', tuner will return the hyperparameter set with smaller expectation.

**Usage example**

```yaml
# config.yml
tuner:
  builtinTunerName: Evolution
  classArgs:
    optimize_mode: maximize
```

<br />

<a name="SMAC"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `SMAC`

> Builtin Tuner Name: **SMAC**

**Installation**

SMAC need to be installed by following command before first use.

```bash
nnictl package install --name=SMAC
```

**Suggested scenario**

Similar to TPE, SMAC is also a black-box tuner which can be tried in various scenarios, and is suggested when computation resource is limited. It is optimized for discrete hyperparameters, thus, suggested when most of your hyperparameters are discrete.

**Requirement of classArg**

* **optimize_mode** (*maximize or minimize, optional, default = maximize*) - If 'maximize', tuners will return the hyperparameter set with larger expectation. If 'minimize', tuner will return the hyperparameter set with smaller expectation.

**Usage example**

```yaml
# config.yml
tuner:
  builtinTunerName: SMAC
  classArgs:
    optimize_mode: maximize
```

<br />

<a name="Batch"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `Batch Tuner`

> Builtin Tuner Name: BatchTuner

**Suggested scenario**

If the configurations you want to try have been decided, you can list them in searchspace file (using `choice`) and run them using batch tuner.

**Usage example**

```yaml
# config.yml
tuner:
  builtinTunerName: BatchTuner
```

<br />

Note that the search space that BatchTuner supported like:

```json
{
    "combine_params":
    {
        "_type" : "choice",
        "_value" : [{"optimizer": "Adam", "learning_rate": 0.00001},
                    {"optimizer": "Adam", "learning_rate": 0.0001},
                    {"optimizer": "Adam", "learning_rate": 0.001},
                    {"optimizer": "SGD", "learning_rate": 0.01}, 
                    {"optimizer": "SGD", "learning_rate": 0.005}, 
                    {"optimizer": "SGD", "learning_rate": 0.0002}]
    }
}
```

The search space file including the high-level key `combine_params`. The type of params in search space must be `choice` and the `values` including all the combined-params value.

<a name="GridSearch"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `Grid Search`

> Builtin Tuner Name: **Grid Search**

**Suggested scenario**

Note that the only acceptable types of search space are `choice`, `quniform`, `qloguniform`. **The number `q` in `quniform` and `qloguniform` has special meaning (different from the spec in [search space spec](./SearchSpaceSpec.md)). It means the number of values that will be sampled evenly from the range `low` and `high`.**

It is suggested when search space is small, it is feasible to exhaustively sweeping the whole search space.

**Usage example**

```yaml
# config.yml
tuner:
  builtinTunerName: GridSearch
```

<br />

<a name="Hyperband"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `Hyperband`

> Builtin Advisor Name: **Hyperband**

**Suggested scenario**

It is suggested when you have limited computation resource but have relatively large search space. It performs well in the scenario that intermediate result (e.g., accuracy) can reflect good or bad of final result (e.g., accuracy) to some extent.

**Requirement of classArg**

* **optimize_mode** (*maximize or minimize, optional, default = maximize*) - If 'maximize', tuners will return the hyperparameter set with larger expectation. If 'minimize', tuner will return the hyperparameter set with smaller expectation.
* **R** (*int, optional, default = 60*) - the maximum STEPS (could be the number of mini-batches or epochs) can be allocated to a trial. Each trial should use STEPS to control how long it runs.
* **eta** (*int, optional, default = 3*) - `(eta-1)/eta` is the proportion of discarded trials

**Usage example**

```yaml
# config.yml
advisor:
  builtinAdvisorName: Hyperband
  classArgs:
    optimize_mode: maximize
    R: 60
    eta: 3
```

<br />

<a name="NetworkMorphism"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `Network Morphism`

> Builtin Tuner Name: **NetworkMorphism**

**Installation**

NetworkMorphism requires [pyTorch](https://pytorch.org/get-started/locally), so users should install it first.

**Suggested scenario**

It is suggested that you want to apply deep learning methods to your task (your own dataset) but you have no idea of how to choose or design a network. You modify the [example](https://github.com/Microsoft/nni/tree/master/examples/trials/network_morphism/cifar10/cifar10_keras.py) to fit your own dataset and your own data augmentation method. Also you can change the batch size, learning rate or optimizer. It is feasible for different tasks to find a good network architecture. Now this tuner only supports the computer vision domain.

**Requirement of classArg**

* **optimize_mode** (*maximize or minimize, optional, default = maximize*) - If 'maximize', tuners will return the hyperparameter set with larger expectation. If 'minimize', tuner will return the hyperparameter set with smaller expectation.
* **task** (*('cv'), optional, default = 'cv'*) - The domain of experiment, for now, this tuner only supports the computer vision(cv) domain.
* **input_width** (*int, optional, default = 32*) - input image width
* **input_channel** (*int, optional, default = 3*) - input image channel
* **n_output_node** (*int, optional, default = 10*) - number of classes

**Usage example**

```yaml
# config.yml
tuner:
  builtinTunerName: NetworkMorphism
    classArgs:
      optimize_mode: maximize
      task: cv
      input_width: 32
      input_channel: 3
      n_output_node: 10
```

<br />

<a name="MetisTuner"></a>

![](https://placehold.it/15/1589F0/000000?text=+) `Metis Tuner`

> Builtin Tuner Name: **MetisTuner**

Note that the only acceptable types of search space are `choice`, `quniform`, `uniform` and `randint`.

**Installation**

Metis Tuner requires [sklearn](https://scikit-learn.org/), so users should install it first. User could use `pip3 install sklearn` to install it.

**Suggested scenario**

Similar to TPE and SMAC, Metis is a black-box tuner. If your system takes a long time to finish each trial, Metis is more favorable than other approaches such as random search. Furthermore, Metis provides guidance on the subsequent trial. Here is an [example](https://github.com/Microsoft/nni/tree/master/examples/trials/auto-gbdt/search_space_metis.json) about the use of Metis. User only need to send the final result like `accuracy` to tuner, by calling the nni SDK.

**Requirement of classArg**

* **optimize_mode** (*'maximize' or 'minimize', optional, default = 'maximize'*) - If 'maximize', tuners will return the hyperparameter set with larger expectation. If 'minimize', tuner will return the hyperparameter set with smaller expectation.

**Usage example**

```yaml
# config.yml
tuner:
  builtinTunerName: MetisTuner
  classArgs:
    optimize_mode: maximize
```