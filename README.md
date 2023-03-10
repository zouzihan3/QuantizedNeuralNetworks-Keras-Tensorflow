# 笔记
@2023年3月10日16:01:46
探索低精度float训练时看到的，一个QNN代码和他的论文。

# Training Quantized Neural Networks

## Introduction
Train your own __Quantized Neural Networks (QNN)__ - networks trained with quantized weights and activations - in __Keras / Tensorflow__.
If you use this code, please cite "B.Moons et al. "Minimum Energy Quantized Neural Networks", Asilomar Conference on Signals, Systems and Computers, 2017". Take a look at our [presentation](https://www.researchgate.net/publication/320775013_Presentation_on_Minimum_Energy_Quantized_Neural_Networks_Asilomar_2017) or at the paper on [arxiv](https://arxiv.org/abs/1711.00215).

This code is based on a [lasagne/theano](https://github.com/MatthieuCourbariaux/BinaryNet) and a [Keras/Tensorflow](https://github.com/DingKe/BinaryNet) version of [BinaryNet](https://papers.nips.cc/paper/6573-binarized-neural-networks).

## Preliminaries
Running this code requires:
1. [Tensorflow](https://www.tensorflow.org/install/)
2. [Keras 2.0](https://keras.io/)
3. [pylearn2](http://deeplearning.net/software/pylearn2/) + the correct PYLEARN2_DATA_PATH in ./personal_config/shell_source.sh
3. A GPU with recent versions of [CUDA and CUDNN](https://developer.nvidia.com/cudnn)
4. Correct paths in ./personal_config/shell_source.sh

Make sure your backend='tensorflow' and image_data_format='channels_last' in the ~/.keras/keras.json file.

## Training your own QNN

This repo includes toy examples for CIFAR-10 and MNIST.
Training can be done by running the following: 

  ./train.sh <config_file> -o <override_parameters>

-o overrides parameters in the <config_file>.
  
The following parameters are crucial:
* network_type: 'float', 'qnn', 'full-qnn', 'bnn', 'full-bnn'
* wbits, abits: the number of bits used for weights and activations
* lr: the used learning rate. 0.01 is a typical good starting point
* dataset, dim, channels: variables depending on the used dataset
* nl<>: [the number of layers in block A, B, C](https://www.linkedin.com/in/bert-moons-41867143/)
* nf<>: [the number of filters in block A, B, C](https://www.linkedin.com/in/bert-moons-41867143/)

## Examples 
* This is how to train a 4-bit full qnn on CIFAR-10:

  ./train.sh config_CIFAR-10 -o lr=0.01 wbits=4 abits=4 network_type='full-qnn'
  
* This is how to train a qnn with 4-bit weights and floating point activations on CIFAR-10:

  ./train.sh config_CIFAR-10 -o lr=0.01 wbits=4 network_type='qnn'
  
* This is how to train a BinaryNet on CIFAR-10:

  ./train.sh config_CIFAR-10 -o lr=0.01 network_type='full-bnn'
  
*** 
The included networks have parametrized sizes and are split into three blocks (A-B-C), each with a number of layers (nl) and a number of filters per layer (nf).

* This is how to train a small 2-bit network on MNIST:

  ./train.sh config_MNIST -o nla=1 nfa=64 nlb=1 nfb=64 nlc=1 nfc=64 wbits=2 abits=2 network_type='full-qnn'
  
* This is how to train a large 8-bit network on CIFAR-10:

  ./train.sh config_CIFAR-10 -o nla=3 nfa=256 nlb=3 nfb=256 nlc=3 nfc=256 wbits=8 abits=8 network_type='full-qnn'






