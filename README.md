# Mnist Dataset Image Classification (Computer Vision)
**The objective of this Comp Vis github project is to showcase the custom implementation of subsets (1 block) of popular conv net architectures, specifically vggnet as well as resnet. Additionally, a pretrained resnet model was loaded and fine-tuned to provide a comparison basis between fine-tuning pretrained models and custom models.**

## Scoring Metric Choice
**Accuracy** is utilised as the scoring metric since from the Exploratory Data Analysis, the labels are approximately balanced. It is evident that all 0 - 9 digits are very balanced. As such, accuracy is considered for the evaluation metric

## Modelling Approaches 
**Simple and basic Exploratory Data Analysis** is performed to have a better understanding of the images in the dataset. 

**The notebook has numerically labelled the different approaches using custom 1 block vggnet and with respective improvements. Subseqeuntly, custom implementation of resnet with the respective improvements is performed. Finally, fine-tuning of pretrained resnet was performed.**

1. **Custom 1 block VggNet + Batch Normalisation + Data Augmentation**
A 1 block vggnet was implemented as it is a straightforward conv net model to apply and trial before considering the use of more complicated models. The blocks had indicated **overfitting** from this model evaluation. 

As such, **batchnorm2d** was introduced as it has implicit regularisation effects by injecting "noise" additively or multiplicatively during the standardisation process. **Spatial Dropout2d (for conv layers) was not implemented** as it is advised against to include both batchnorm2d and dropout together, since there can be disharmony in the variance shift when using them together. (When dropout remove some neurons, a bad estimate of mean and variacne for the activation is obtained)

**Data Augmentation** is thus performed to introduce transformations to the original images so that the model can learn from more varied images and generalize better. This helps to reduce the overfitting. The **transformations introduced are intended to be modest and practical** as extreme transformations are not meaningful to the model. **No transformations are performed on the validation set** to prevent introducing randomness during evaluation. **Online data transformation is performed since images are newly transformed at each and every iteration compared to a fixed offline data augmentation** that also increases disk space.

2. **Custom 1 block ResNet + Batch Normalisation + Data Augmentation vs Pretrained ResNet + Data Augmentation**
A custom 1 block ResNet was implemented as it is one of the more popular cnn architecture with good performance. Unfortunately, the accuracy score of this 1 block ResNet is lower compared to the VggNet. **A plausible reasoning is that it is only a single layer and the popularity behind ResNet is due to the mitigation of vanishing/exploding gradients despite have many blocks of layers**. 

Pre-trained ResNet that was fine-tuned on the last fully connected layer had the lowest score among all the above iterative improvements as **it is suspected that the data distribution of the pretrained ImageNet is vastly different from the Mnist dataset.** **More importantly, it is critical to implement fine-tuning of the pre-trained model correctly. As such, the 1st Conv2d layer has been configured to have a parameter of in_channels = 1 to accept a single channel iamge as ImageNet dataset are color images (3 channels). The images are required to be normalized (0-1) before applying another round of normalization using mean = [0.485, 0.456, 0.406] and std = [0.229, 0.224, 0.225]. Since it is a single channel, the values in mean and std are summed and divided (mean) for the single channel**

**Vggnet with batchnorm and data augmentation should be used for inference**

Side note (for future updates): Images should have been normalised to 0-1 range as having values in the original range (0-255) can lead to exploding gradients.
