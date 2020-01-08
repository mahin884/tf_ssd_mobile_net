# Convert a TensorFlow Model - Solution

Download & Extract:
```
wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz`
tar -xvf ssd_mobilenet_v2_coco_2018_03_29.tar.gz
```

First, you can start by checking out the additional documentation specific to TensorFlow
models from the Model Detection Zoo [here](https://docs.openvinotoolkit.org/latest/_docs_MO_DG_prepare_model_convert_model_tf_specific_Convert_Object_Detection_API_Models.html).

I noticed three additional arguments that were important here:

- `--tensorflow_object_detection_api_pipeline_config`
- `--tensorflow_use_custom_operations_config`
- `--reverse_input_channels`

The first of these just needs the `pipeline.config` file that came with the downloaded model.

The second of these needs a JSON support file for TensorFlow models. I found that the
`ssd_v2_support.json` extension worked with the MobileNet model here.

The final of these is due to the TensorFlow models being trained on RGB images, but the
Inference Engine otherwise defaulting to BGR.

Now, given that I was in the directory with the frozen model file from TensorFlow, here was the 
full path to convert my model:

```
python /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model frozen_inference_graph.pb --tensorflow_object_detection_api_pipeline_config pipeline.config --reverse_input_channels --tensorflow_use_custom_operations_config /opt/intel/openvino/deployment_tools/model_optimizer/extensions/front/tf/ssd_v2_support.json
```
the opt folder is situated at the root directory. It contains the intel openvino model optimizer. This address changes base on your setup. In the class room directory the address is - ` /opt/intel/openvino/deployment_tools/model_optimizer/mo.py`.


This is pretty long! I would suggest considering setting a path environment variable for the Model Optimizer if you are working locally on a Linux-based machine. You could do something like this:

```export MOD_OPT=/opt/intel/openvino/deployment_tools/model_optimizer```

And then when you need to use it, you can utilize it with `\$MOD_OPT/mo.py` instead of entering the full long path each time. In this case, that would also help shorten the path to the ssd_v2_support.json file used.