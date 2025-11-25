# TensorFlow Lite C++ multi device/vip_core example

This example shows how you can build and run  TensorFlow Lite models on multi-device or multi-core(VIP9X00) . The models located in: (https://github.com/sunshinemyson/TIM-VX/releases)

Multi-device: There are multiple NPU devices on the same SOC.
Multi-Core: There are multiple VIP_COREs in one NPU device, eg:VIP9400 means there are 4 VIP_COREs in the NPU device.

This example applies to scenarios:
1. Multiple NPU devices on the same SOC.
   When multiple NPU devices are instantiated on a single SOC, the target NPU device for inference can be specified
   using the "device_id" parameter.
2. An NPU Device with a MULTI-VIP configuration on a SOC.
   For example, if a SOC contains a VIP9400(4 VIPCOREs) NPU device, inference can be assigned to a specific subset of
   the 4 VIP cores by specifying the "core_index" and "core_count" parameters. This allows using "core_count" cores
   starting from the designated "core_index" for excutuion.

#### Step 1. Build
##requirements
Vivante SDK >= 6.4.22
ovxlib >= 1.2.26
viplite >=2.0.0
tim-vx  >= 7b24f4d(commit)

1. Turn option TFLITE_ENABLE_MULTI_DEVICE to On in ./CMakeLists.txt or Add -DTFLITE_ENABLE_MULTI_DEVICE when build cmake
2. TIM_VX should open TIM_VX_ENABLE_PLATFORM

#### Step 2. Run

    The config.txt is used for store models information.Every line repreasents one model information, the format is:

    model_location   run_repeat_num   [device_id:core_index,core_count] input_data

   If input_data is NULL, we will run model with random data. for example:

    ${WORKESPACE}/mobilenet_v2_quant.tflite 1 [0:0,4] NULL
    ${WORKESPACE}/inception_v3_quant.tflite  1 [0:0,4]   ./input_data.bin

```sh
export VSIMULATOR_CONFIG=VIP9400O_PID0XD9
export VIVANTE_SDK_DIR=${driver_location}
export LD_LIBRARY_PATH=${tim_vx_lib}:${driver_location}/lib:$LD_LIBRARY_PATH
./multi_device <patch_to_libvx_delegate.so> <config.txt>
```
