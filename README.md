# NeRF - Neural Radiance Field

This is the final project for my CSE 275: Deep Learning for 3D Data course at the University of California, San Diego (UCSD). In this project, I reproduced the Neural Radiance Field (NeRF) model for novel view synthesis of an interesting 3D scene on bottles. I followed the [original paper](https://arxiv.org/abs/2003.08934) and this [tutorial](https://towardsdatascience.com/its-nerf-from-nothing-build-a-vanilla-nerf-with-pytorch-7846e4c45666) to implement my NeRF model.

## Usage

First, navigate through the `nerf.ipynb` jupyter notebook, where I have all the code implemented in an organized fashion. Run the first two sections to setup the bottles dataset and the initial NeRF model and methods. To train, edit the hyperparameters in its respective section and run the blocks in the training section. 

Once the training is completed, run the blocks in the prediction section to generate the rgb maps and the depth maps of the five novel views in the prediction folder. The folder already contains the result I obtained from training the model for ~1.5 days on a RTX 4090.

## Model

The model in this project is a faithful implementation of the NeRF model in the original paper. I experimented with a higher configuration that has 12 layers (384 channels per layer) + 1 layer (192 channels) and the skip connection at the seventh layer.

## Result

This model was trained for 750k iterations in total, which took about 1.5 days on a RTX 4090. Different to the official NeRF code, I used a larger networks as discussed in the previous section and a larger batch size (number of random rays samples per training iteration) of 2500 versus 1024. I set my near and far bound to 0 and 5, respectively. After 750K iterations of training, the validation PSNR score reached an average of about 31 PSNR.

I've also experimented with the default configration used in the original NeRF paper. I trained it for 400K iterations on a RTX 4090 for 12 hours, reaching about 29 PSNR on validation. Training it further did not improve the accuracy.

Here is an example groundtruth image from the training set:
![gt](bottles/rgb/0_train_0030.png)

Here is a prediction of the model of a novel view in the test set:
![pred](prediction/2_test_0093.png)

There seem to still be noise in the prediction and the non-lambertion material is not perfectly reproduced. Yet, the texture on the leaves, garlic, and wood demonstrate the model's ability to capture fine detail and synthesize realistic textures from novel viewpoints.