# MXNet

[![Build Status](https://travis-ci.org/dmlc/MXNet.jl.svg?branch=master)](https://travis-ci.org/dmlc/MXNet.jl)
[![codecov.io](https://codecov.io/github/dmlc/MXNet.jl/coverage.svg?branch=master)](https://codecov.io/github/dmlc/MXNet.jl?branch=master)
[![Documentation Status](https://readthedocs.org/projects/mxnetjl/badge/?version=latest)](http://mxnetjl.readthedocs.org/en/latest/?badge=latest)
[![License](http://dmlc.github.io/img/apache2.svg)](LICENSE.md)
[![Join the chat at https://gitter.im/dmlc/mxnet](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/dmlc/mxnet?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)


MXNet.jl is the [dmlc/mxnet](https://github.com/dmlc/mxnet) [Julia](http://julialang.org/) package. MXNet.jl brings flexible and efficient GPU computing and state-of-art deep learning to Julia. Some highlight of features include:

* Efficient tensor/matrix computation across multiple devices, including multiple CPUs, GPUs and distributed server nodes.
* Flexible symbolic manipulation to composite and construct state-of-the-art deep learning models.

Here is an exmple of how training a simple 3-layer MLP on MNIST looks like:

```julia
using MXNet

mlp = @mx.chain mx.Variable(:data) =>
  mx.MLP([128, 64, 10])            =>
  mx.SoftmaxOutput(name=:softmax)

# data provider
batch_size = 100
include(Pkg.dir("MXNet", "examples", "mnist", "mnist-data.jl"))
train_provider, eval_provider = get_mnist_providers(batch_size)

# setup model
model = mx.FeedForward(mlp, context=mx.cpu())

# optimizer
optimizer = mx.SGD(lr=0.1, momentum=0.9, weight_decay=0.00001)

# fit parameters
mx.fit(model, optimizer, train_provider, n_epoch=20, eval_data=eval_provider)
```

For more details, please refer to the [document](http://mxnetjl.readthedocs.org/) and [examples](examples).
