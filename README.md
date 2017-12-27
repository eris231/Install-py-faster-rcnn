# Install-py-faster-rcnn
OS Ubuntu 16.04
```
git clone --recursive https://github.com/rbgirshick/py-faster-rcnn
```

```
cd py-faster-rcnn/lib
make
```

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
```
vim setup.py
```

```
cudaconfig = {'home':home, 'nvcc':nvcc,
                  'include': pjoin(home, 'include'),
                  'lib64': pjoin(home, 'lib64')}
```

```
cudaconfig = {'home':home, 'nvcc':nvcc,
                  'include': pjoin(home, 'include'),
                  'lib64': pjoin(home, 'lib')}
```

```
cd caffe-fast-rcnn/
cp Makefile.config.example Makefile.config
vim Makefile.config
```
https://github.com/BVLC/caffe/issues/2347
```
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
WITH_PYTHON_LAYER := 1
```

```
make -j8 && make pycaffe
```

```
cd py-faster-rcnn/ ./data/scripts/fetch_faster_rcnn_models.sh
```

```
cd tools/
python demo.py
```


```
ImportError: No module named gpu_nms
```
https://github.com/rbgirshick/py-faster-rcnn/issues/8
```
cd py-faster-rcnn/lib
make
```

```
ImportError: No module named _caffe
```
https://github.com/BVLC/caffe/issues/263
```
make pycaffe
```

```
Check failed: registry.count(type) == 1 (0 vs. 1) Unknown layer type: Python
```
https://github.com/rbgirshick/fast-rcnn/issues/31
```
vim Makefile.config
uncommenting WITH_PYTHON_LAYER := 1 and making clean build
```
