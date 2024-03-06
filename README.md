本项目改自 [seamless_communication](https://github.com/facebookresearch/seamless_communication)，保留了其中的 T2TT 部分，可以通过 visual studio 打开 CMakeLists.txt 文件直接编译。

bugs:

- [ ] 中文和其它语言互译会出乱码，英文与法文等拉丁语系得互译没问题

todo: 

- [ ] 封装成动态链接库

# unity.cpp

## Introduction
[GGML](https://github.com/ggerganov/ggml) is an open source library in C to enable large model inference on various hardware platforms. We implemented unity.cpp in ggml. Now it supports SeamlessM4T model for X2T tasks - Speech-to-text translation (S2TT), Acoustic speech recognition (ASR), Text-to-text translation (T2TT).  

The project is still active in development. Contributions are welcome!

## Build
To build the interactive console for S2TT & ASR & T2TT, 
```

cd seamless_communication/ggml
mkdir build; cd build
cmake -DGGML_OPENBLAS=ON \
    -DBUILD_SHARED_LIBS=On \
	  -DCMAKE_BUILD_TYPE=Release \
	  -DCMAKE_CXX_FLAGS="-g2 -fno-omit-frame-pointer" \
    ..
make -j4 unity # Interactive Console

```
For more build commands see [Makefile](Makefile). 

## CLI usage

### T2TT
Launching command:
```
OPENBLAS_NUM_THREADS=8 ./bin/unity --model nllb-200_dense_1b.ggml --text
```
In the console, enter "input_text tgt_lang" - input text and target langauge, separated by space. Note that the language code should align with [NLLB BCP-47 code](https://github.com/facebookresearch/flores/blob/main/flores200/README.md#languages-in-flores-200), NOT 3-letter language code as S2TT task with Seamless. Unifying this is on todo list. 


### Model downloads 

Converted ggml models could be downloaded from 
|SeamlessM4T_large | SeamlessM4T_medium | NLLB_dense_1b | NLLB_distill_600m |
|-------- | -------- | ------- | ------- |
| [model](dl.fbaipublicfiles.com/seamless/models/seamlessM4T_large.ggml) | [model](dl.fbaipublicfiles.com/seamless/models/seamlessM4T_medium.ggml) |  [model](dl.fbaipublicfiles.com/seamless/models/nllb-200_dense_1b.ggml) | [model](dl.fbaipublicfiles.com/seamless/models/nllb-200_dense_distill_600m.ggml)

For more details of NLLB models, please check https://github.com/facebookresearch/fairseq/tree/nllb.

## Fairseq2 model conversion 
Models from fairseq2 checkpoints could be converted to ggml automatically with [ggml_convert.py](ggml_convert.py). 
```
python ggml_convert.py -m MODEL_NAME
```
where MODEL_NAME corresponds to asset cards in fairseq2 / seamless_communication, e.g. seamlessM4T_medium, seamlessM4T_large

## Python bindings
We also utilize ggml python bindings for better dev experience. For examples of running unity.cpp in python, refer to tests in [test_unity_cpp.py](test_unity_cpp.py). 

## [Optional]Dependencies
### OpenBLAS
We strongly suggest building with OpenBLAS, as we've seen 8x speedup on test machine. 

