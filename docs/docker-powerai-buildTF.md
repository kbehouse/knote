---
id: docker-powerai-buildTF
title: Build TF for PowerAI
sidebar_label: Build TF
---

* 由於PowerAI的CPU是Power系列CPU(記得是Power8)
* 因此很多新東西必須`自己編譯`
* 當然，先試試看使用apt-get，如果不行再想辦法自己編譯
* 以下，是以前我編Tensorflow(tf)的方法，如果你們需要新版的tf，可以嘗試以下方法

## Get CPU Architecture

取得CPU的架構代號
ppc64le
> 會發現跟一般x86的CPU不同，x86是x86_64

```
uname -m
```

## Use docker to build 

建議先使用Dokcer來編譯，完成後再安裝到主系統

You could use the docker for build ppcle64 package, NOT COMMEND build in the main system

```
$ docker images

REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
ppc64le-tf-keras-nb        include_build       7b05882cd800        6 weeks ago         15GB
```

you could use the ppc64le-tf-keras-nb:include_build to build
```
$ docker run -d --runtime=nvidia  -p 6002:22 -p 6003:8888  -e NVIDIA_VISIBLE_DEVICES=0 ppc64le-tf-keras-nb:include_build 
```

## Install Bazel

from https://github.com/ppc64le/build-scripts/blob/master/bazel/bazel_ubuntu_16.04.sh
replace url to newer https://github.com/bazelbuild/bazel/releases

```
cd bazel/output/
export PATH=$(pwd):$PATH  # set bazel to path
```


## Install Tensorflow

```
cd tensorflow/
./configure

bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
sudo pip3 install /tmp/tensorflow_pkg/tensorflow*.whl
```
