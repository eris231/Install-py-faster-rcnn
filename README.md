# Install-py-faster-rcnn

List out some problem may encounter in installation

Basically follows the py-faster-rcnn guide

https://github.com/rbgirshick/py-faster-rcnn

OS Ubuntu 16.04

First clone the py-faster-rcnn repo with caffe-fast-rcnn recursively
```
git clone --recursive https://github.com/rbgirshick/py-faster-rcnn
```
Go to the following directory and make it
```
cd py-faster-rcnn/lib
make
```
If encounter this error
```
python setup.py build_ext --inplace
Traceback (most recent call last):
  File "setup.py", line 58, in <module>
    CUDA = locate_cuda()
  File "setup.py", line 55, in locate_cuda
    raise EnvironmentError('The CUDA %s path could not be located in %s' % (k, v))
EnvironmentError: The CUDA lib64 path could not be located in /usr/lib64
Makefile:2: recipe for target 'all' failed
make: *** [all] Error 1
```
https://stackoverflow.com/questions/39108859/oserror-the-cuda-lib64-path-could-not-be-located-in-usr-lib64

Go edit setup.py
```
vim setup.py
```
Find the lib64 part
```
cudaconfig = {'home':home, 'nvcc':nvcc,
                  'include': pjoin(home, 'include'),
                  'lib64': pjoin(home, 'lib64')}
```
Change into this
```
cudaconfig = {'home':home, 'nvcc':nvcc,
                  'include': pjoin(home, 'include'),
                  'lib64': pjoin(home, 'lib')}
```
After make py-faster-rcnn, next is to make caffe

Go edit the Makefile.config
```
cd caffe-fast-rcnn/
cp Makefile.config.example Makefile.config
vim Makefile.config
```
Add /usr/include/hdf5/serial behind INCLUDE_DIRS

And uncomment WITH_PYTHON_LAYER := 1

Otherwise there might be error in making or running demo.py

https://github.com/BVLC/caffe/issues/2347
```
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
WITH_PYTHON_LAYER := 1
```
make with multithreads (8 here), and make pycaffe
```
make -j8 && make pycaffe
```
Go run the script to download models
```
cd py-faster-rcnn/ ./data/scripts/fetch_faster_rcnn_models.sh
```
Go tools/ to run demo.py to test
```
cd tools/
python demo.py
```

## Here list some error may encounter

```
ImportError: No module named gpu_nms
```
Forget to make py-faster-rcnn

Solution

https://github.com/rbgirshick/py-faster-rcnn/issues/8

```
cd py-faster-rcnn/lib
make
```

```
ImportError: No module named _caffe
```
Forget to make pycaffe

Solution

https://github.com/BVLC/caffe/issues/263

```
make pycaffe
```

```
Check failed: registry.count(type) == 1 (0 vs. 1) Unknown layer type: Python
```
Forget to uncomment WITH_PYTHON_LAYER := 1

Solution

https://github.com/rbgirshick/fast-rcnn/issues/31
```
vim Makefile.config
```
Find 
```
WITH_PYTHON_LAYER := 1
```
uncommenting WITH_PYTHON_LAYER := 1 and make clean build
