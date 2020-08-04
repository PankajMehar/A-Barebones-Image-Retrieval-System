# A Barebones Image Retrieval System

<div align="center"><img src="https://i.ibb.co/ZXtwJjV/Webp-net-resizeimage.png" width="100" height="100"></img></div>

This project presents a simple framework to retrieve images similar to a query image. The framework is as follows:

- Train a CNN model (A) on a set of labeled images with Triplet Loss ( I used [this one).](https://www.tensorflow.org/addons/api_docs/python/tfa/losses/TripletSemiHardLoss)

- Use the trained CNN model (A) to extract features from the validation set.
- Train a kNN model (B) on these extracted features with k set to the number of neighbors wanted.
- Grab an image (I) from the validation set and extract its features using the same CNN model (A).
- Use the same kNN model (B) to calculate the nearest neighbors of I.

I used the Flowers dataset for experiments. I tried the above approach to a scenario where I had only 184 examples from the Flowers dataset and it worked well.

Here's a sample result:

![](https://i.ibb.co/ZVrLT3b/image.png)

  

## Training specifics

I fine-tuned pre-trained models for the task of minimizing the Triplet Loss. I experimented with the following models (pre-trained on ImageNet):
- VGG16
- MobileNetV2
- ResNet50
- BigTransfer (also referred to as BiT) which is essentially a ResNet50 but pre-trained on a larger dataset

While using the first three models I used the following Transformers' inspired callback -

![](https://i.ibb.co/kSFRtGb/image.png)

I referred to the code of this callback from [here](https://nbviewer.jupyter.org/github/GoogleCloudPlatform/training-data-analyst/blob/master/courses/fast-and-lean-data-science/keras_flowers_gputputpupod_tf2.1.ipynb).

While using the BiT model I used what is referred to as the BiT-HyperRule. BiT models come in [different variants](https://tfhub.dev/google/collections/bit/1), I used this variant - `m-r50x1` . Refer to [this blog post](https://blog.tensorflow.org/2020/05/bigtransfer-bit-state-of-art-transfer-learning-computer-vision.html) to know more about BiT and this rule in general.

  

## Visualization of the embedding space of a limited validation set

***(The models were trained on 184 examples)***

![](https://i.ibb.co/ZdrgY7B/image.png)

## Training progress

***(The models were trained on 184 examples)***

![](https://i.ibb.co/w6G3Wp5/image.png)

The improvements with BiT is quite prominent.