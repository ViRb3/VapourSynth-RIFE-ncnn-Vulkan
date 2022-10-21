# RIFE

[![CI](https://github.com/HomeOfVapourSynthEvolution/VapourSynth-RIFE-ncnn-Vulkan/actions/workflows/CI.yml/badge.svg)](https://github.com/HomeOfVapourSynthEvolution/VapourSynth-RIFE-ncnn-Vulkan/actions/workflows/CI.yml)
![downloads](https://img.shields.io/github/downloads/HomeOfVapourSynthEvolution/VapourSynth-RIFE-ncnn-Vulkan/total.svg)

Real-Time Intermediate Flow Estimation for Video Frame Interpolation, based on [rife-ncnn-vulkan](https://github.com/nihui/rife-ncnn-vulkan).


## Usage
    rife.RIFE(vnode clip[, int model=5, int factor_num=2, int factor_den=1, int fps_num=None, int fps_den=None, string model_path=None, int gpu_id=None, int gpu_thread=2, bint tta=False, bint uhd=False, bint sc=False, bint skip=False, float skip_threshold=60.0, bint list_gpu=False])

- clip: Clip to process. Only RGB format with float sample type of 32 bit depth is supported.

By default models are exported with ensemble=False and Fast=True

- model: Model to use.
  - 0 = rife
  - 1 = rife-HD
  - 2 = rife-UHD
  - 3 = rife-anime
  - 4 = rife-v2
  - 5 = rife-v2.3
  - 6 = rife-v2.4
  - 7 = rife-v3.0
  - 8 = rife-v3.1
  - 9 = rife-v4 (ensemble=False / fast=True)
  - 10 = rife-v4 (ensemble=True / fast=False)
  - 11 = [rife-v4.1](https://github.com/mirrorsysu/rife-ncnn-vulkan/tree/model_4_1) (ensemble=False / fast=True)
  - 12 = rife-v4.1 (ensemble=True / fast=False)
  - 13 = rife-v4.2 (ensemble=False / fast=True)
  - 14 = rife-v4.2 (ensemble=True / fast=False)
  - 15 = rife-v4.3 (ensemble=False / fast=True)
  - 16 = rife-v4.3 (ensemble=True / fast=False)
  - 17 = rife-v4.4 (ensemble=False / fast=True)
  - 18 = rife-v4.4 (ensemble=True / fast=False)
  - 19 = rife-v4.5 (ensemble=False)
  - 20 = rife-v4.5 (ensemble=True)
  - 21 = rife-v4.6 (ensemble=False)
  - 22 = rife-v4.6 (ensemble=True)

  ## My custom models

  - 23 = sudo_rife4 (ensemble=False / fast=True)
  - 24 = sudo_rife4 (ensemble=True / fast=False)
  - 25 = sudo_rife4 (ensemble=True / fast=True)

- factor_num, factor_den: Factor of target frame rate. For example `factor_num=5, factor_den=2` will multiply input clip FPS by 2.5. Only rife-v4 model supports custom frame rate.

- fps_num, fps_den: Target frame rate. Only rife-v4 model supports custom frame rate. Supersedes `factor_num`/`factor_den` parameter if specified.

- model_path: RIFE model path. Supersedes `model` parameter if specified.

- gpu_id: GPU device to use.

- gpu_thread: Thread count for interpolation. Using larger values may increase GPU usage and consume more GPU memory. If you find that your GPU is hungry, try increasing thread count to achieve faster processing.

- tta: Enable TTA(Test-Time Augmentation) mode.

- uhd: Enable UHD mode.

- sc: Avoid interpolating frames over scene changes. You must invoke `misc.SCDetect` on YUV or Gray format of the input beforehand so as to set frame properties.

- skip: Skip interpolating static frames. Requires [VMAF](https://github.com/HomeOfVapourSynthEvolution/VapourSynth-VMAF) plugin.

- skip_threshold: PSNR threshold to determine whether the current frame and the next one are static.

- list_gpu: Simply print a list of available GPU devices on the frame and does no interpolation.

## Compilation

```bash
brew install meson ninja llvm libomp pkg-config molten-vk vulkan-headers ncnn
PKG_CONFIG_PATH="$PWD/pkgconfig/" \
	CC=/opt/homebrew/opt/llvm/bin/clang \
	CXX=/opt/homebrew/opt/llvm/bin/clang++ \
	meson build
cd build
ninja
```
