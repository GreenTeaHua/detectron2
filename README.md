<img src=".github/Detectron2-Logo-Horz.svg" width="300" >

Detectron2 is Facebook AI Research's next generation software system
that implements state-of-the-art object detection algorithms.
It is a ground-up rewrite of the previous version,
[Detectron](https://github.com/facebookresearch/Detectron/),
and it originates from [maskrcnn-benchmark](https://github.com/facebookresearch/maskrcnn-benchmark/).

<div align="center">
  <img src="https://user-images.githubusercontent.com/1381301/66535560-d3422200-eace-11e9-9123-5535d469db19.png"/>
</div>

### What's New
* It is powered by the [PyTorch](https://pytorch.org) deep learning framework.
* Includes more features such as panoptic segmentation, densepose, Cascade R-CNN, rotated bounding boxes, etc.
* Can be used as a library to support [different projects](projects/) on top of it.
  We'll open source more research projects in this way.
* It [trains much faster](https://detectron2.readthedocs.io/notes/benchmarks.html).

See our [blog post](https://ai.facebook.com/blog/-detectron2-a-pytorch-based-modular-object-detection-library-/)
to see more demos and learn about detectron2.

## Installation

See [INSTALL.md](INSTALL.md).  
------------------------------------------------------------------
### for win10  
0. cuda10.1+ cudann +pytorch1.3+vs2019toolkit  
test:
python -c "import torch; from torch.utils.cpp_extension import CUDA_HOME; print(torch.cuda.is_available(), CUDA_HOME)"
-----
1. git clone https://github.com/facebookresearch/detectron2.git  
2. d:/CBNet/detectron2  
3. 修改 1：  
    C:\Miniconda3\envs\py36}\Lib\site-packages\torch\include\torch\csrc\jit\argument_spec.h(190)  
    static constexpr size_t DEPTH_LIMIT = 128;  
      change to -->  
    static const size_t DEPTH_LIMIT = 128;  
    修改2：  
    C:\Miniconda3\envs\py36}\Lib\site-packages\torch\include\pybind11\cast.h(1449)  
    explicit operator type&() { return *(this->value); }  
      change to -->  
    explicit operator type&() { return *((type*)this->value); }  
4. detectron2/layers/csrc/ROIAlign/ROIAlign_cuda.cu  line337  
   dim3 grid(std::min(((int)output_size + 512 -1) / 512, 4096));   //网上  
  //dim3 grid(std::min(at::cuda::ATenCeilDiv(output_size, 512L), 4096L));  原版  
   dim3 grid(std::min(at::cuda::ATenCeilDiv((long)output_size, 512L),   4096L));//https://github.com/conansherry/detectron2/blob/master/detectron2/layers/csrc/ROIAlign/ROIAlign_cuda.cu  
   dim3 grid(std::min(at::cuda::ATenCeilDiv((long)grad.numel(), 512L), 4096L));// line 393  

   D:/CBNet/detectron2/detectron2/layers/csrc/ROIAlignRotated/ROIAlignRotated_cuda.cu(351):  
   dim3 grid(std::min(at::cuda::ATenCeilDiv((long)output_size, 512L), 4096L));  
   D:/CBNet/detectron2/detectron2/layers/csrc/ROIAlignRotated/ROIAlignRotated_cuda.cu(407):  
   dim3 grid(std::min(at::cuda::ATenCeilDiv((long)grad.numel(), 512L), 4096L));  
5. 开启 vs2019 x64 命令行。  

6. python setup.py build develop  
7. test    
--------------------------------------------------------------------------------------------------------  
## Quick Start

See [GETTING_STARTED.md](GETTING_STARTED.md),
or the [Colab Notebook](https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5).

Learn more at our [documentation](https://detectron2.readthedocs.org).
And see [projects/](projects/) for some projects that are built on top of detectron2.

## Model Zoo and Baselines

We provide a large set of baseline results and trained models available for download in the [Detectron2 Model Zoo](MODEL_ZOO.md).


## License

Detectron2 is released under the [Apache 2.0 license](LICENSE).

## Citing Detectron

If you use Detectron2 in your research or wish to refer to the baseline results published in the [Model Zoo](MODEL_ZOO.md), please use the following BibTeX entry.

```BibTeX
@misc{wu2019detectron2,
  author =       {Yuxin Wu and Alexander Kirillov and Francisco Massa and
                  Wan-Yen Lo and Ross Girshick},
  title =        {Detectron2},
  howpublished = {\url{https://github.com/facebookresearch/detectron2}},
  year =         {2019}
}
```
