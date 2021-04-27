# learned-image-compression-using-gan

In neural image compression, we attempt to reduce an image to a smaller representation
such that we can recreate the original image as closely as possible. See [`Full Resolution Image Compression with Recurrent Neural Networks`](https://arxiv.org/abs/1608.05148) for more details on using neural networks
for image compression.

In this example, we train an encoder to compress images to a compressed binary
representation and a decoder to map the binary representation back to the image.
We treat both systems together (encoder -> decoder) as the generator.

A typical image compression trained on L1 pixel loss will decode into
blurry images. We use an adversarial loss to force the outputs to be more
plausible.

This example also highlights the following infrastructure challenges:

* When you have custom code to keep track of your variables

Some other notes on the problem:

* Since the network is fully convolutional, we train on image patches.
* Bottleneck layer is floating point during training and binarized during
  evaluation.

### Results

#### No adversarial loss
<img src="compression_non adversarial loss.png" title="No adversarial loss" width="500" />

#### Adversarial loss
<img src="compression_adverserial loss.png" title="With adversarial loss" width="500" />

### Architectures

#### Compression Network

The compression network is a DCGAN discriminator for the encoder and a DCGAN
generator for the decoder from [`Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks`](https://arxiv.org/abs/1511.06434).
The binarizer adds uniform noise during training then binarizes during eval, as in
[`End-to-end Optimized Image Compression`](https://arxiv.org/abs/1611.01704).

#### Discriminator

The discriminator looks at 70x70 patches, as in
[`Image-to-Image Translation with Conditional Adversarial Networks`](https://arxiv.org/abs/1611.07004).
