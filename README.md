[![Build Status](https://travis-ci.org/Swall0w/torchstat.svg?branch=master)](https://travis-ci.org/Swall0w/torchstat)

# torchstat
This is a lightweight neural network analyzer based on PyTorch.
It is designed to make building your networks quick and easy, with the ability to debug them.
**Note**: This repository is currently under development. Therefore, some APIs might be changed.

This tools can show

* Total number of network parameters
* Theoretical amount of floating point arithmetics (FLOPs)
* Theoretical amount of multiply-adds (MAdd)
* Memory usage

## Installing
There're two ways to install torchstat into your environment.
* Install it via pip.
```bash
$ pip install torchstat
```

* Install and update using **setup.py** after cloning this repository.
```bash
$ python3 setup.py install
```

## A Simple Example
If you want to run the torchstat asap, you can call it as a CLI tool if your network exists in a script.
Otherwise you need to import torchstat as a module.

### CLI tool
```bash
$ torchstat --file example.py --model Net
[MAdd]: Dropout2d is not supported!
[Flops]: Dropout2d is not supported!
      module name  input shape output shape     params memory(MB)           MAdd         Flops duration[%]
0           conv1    3 224 224   10 220 220      760.0       1.85   72,600,000.0  36,784,000.0      60.11%
1           conv2   10 110 110   20 106 106     5020.0       0.86  112,360,000.0  56,404,720.0      35.08%
2      conv2_drop   20 106 106   20 106 106        0.0       0.86            0.0           0.0       0.34%
3             fc1        56180           50  2809050.0       0.00    5,617,950.0   2,809,000.0       4.25%
4             fc2           50           10      510.0       0.00          990.0         500.0       0.22%
total                                        2815340.0       3.56  190,578,940.0  95,998,220.0     100.00%
==========================================================================================================
Total params: 2,815,340
----------------------------------------------------------------------------------------------------------
Total memory: 3.56MB
Total MAdd: 190.58MMAdd
Total Flops: 96.0MFlops
```

If you're not sure how to use a specific command, run the command with the -h or –help switches.
You'll see usage information and a list of options you can use with the command.

### Module
```python
from torchstat import stat
from torchvision.models as models

model = models.resnet18()
stat(model, (3, 224, 224))
```

## Features
**Note**: These features work only nn.Module. Modules in torch.nn.functional are not supported yet.
- [x] FLOPs
- [x] Number of Parameters
- [x] Total memory
- [x] Madd(FMA)
- [ ] Model summary(detail, layer-wise)
- [ ] Export score table
- [ ] MemRead
- [ ] MemWrite

For the supported layers, check out [the details](./detail.md).


## Requirements
* Python 3.6+
* Pytorch 0.4.0+
* Pandas 0.23.4+
* NumPy 1.14.3+

## References
Thanks to @sovrasov for the initial version of flops computation, @ceykmc for the backbone of scripts.
* [flops-counter.pytorch](https://github.com/sovrasov/flops-counter.pytorch)
* [pytorch_model_summary](https://github.com/ceykmc/pytorch_model_summary)
* [chainer_computational_cost](https://github.com/belltailjp/chainer_computational_cost)
* [convnet-burden](https://github.com/albanie/convnet-burden).
