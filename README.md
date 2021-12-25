# Speech_enhancement_using_convolutional_autoencoders
This repository explores speech enhancement using Convolutional Autoencoders.

>## Table of Contents
1. [Description](#description)
2. [Methodology](#methodology)
3. [References](#references)

>## Description
  
In this repository we explore how UNet model or Convolutional Autoencoders with skip connections can be used for speech enhancement. The u-net is convolutional network architecture for fast and precise segmentation of images it was first used in biomedical engineering. But we can take advantage of this network for denoising noisy speech data. Intutively we can consider log-spectrograms as 2D images fed to this network from which relevant features are encoded and then converted back to another spectrogram with reduced noise.   

>## Methodolgy

Steps involved in denoising Noisy speech data:

1. Generating noisy speech files by adding differnt kinds of noise to clean speech files. 
2. Convert noisy speech files to time series data (Waveform model).
3. Convert time series data to log-spectrograms.
4. Training U-Net model to learn noise spectrograms.
5. Generating clean speech spectrogram and converting those back to .wav files.
6. Calculate WER (Word error rate), MER (Match error rate) and WIL (Word information lost) to evaluate performance of the model.

### Generating noisy speech files 

We can use script provided by [MS-SNSD](https://github.com/microsoft/MS-SNSD) to generate required noisy speech files. I have already generated few for my project you can access those through this [drive link](https://drive.google.com/drive/folders/1Pzp3zh7JbEig59Oom_gxhmQ2MGx5ShCa?usp=sharing).

###  Convert noisy speech files to time series data

To load .wav files for computation we can use *librosa.load* with a sampling rate of 8000. For computational purposes we will limit the size of each extracted audio files to 2\*8064 (~2 sec) and stack them in 2-D array of size (number_of_audio_files X 8064). 


![alt text](https://github.com/KaushalNaresh/Speech_enhancement_using_convolutional_autoencoders/blob/main/images/Waveform.PNG)

### Convert time series data to log-spectrograms

Time series data has no information regarding frequency so we will use fourier transformation for this. In particular we will calculate short time fourier transformation of time series data to output complex matrix (spectrogram) which gives the clear picture of how different frequency components are evolving with time. Since humans preception of sound is logarithmic so we will convert our spectrograms into log-spectrograms i.e. convert power into decibels. For model input we only need magnitude part and for converting back spectrograms to audio files complex part is required. We will generate spectrograms for both noisy speech files and clean speech files.

![alt text](https://github.com/KaushalNaresh/Speech_enhancement_using_convolutional_autoencoders/blob/main/images/Spectrograms.PNG)


### Training U-Net model to learn noise spectrograms

U-Net model at its core is convolutional autoencoder with skip connections. In this model max-pooling is used in encoding part and up-sampling is used in decoding part. Skip connections are added from encoder to decoder to tackle vanishing gradient issue. U-Net model takes noisy speech spectrogram as input and noise speech spectrogram as output which is equal to noisy speech spectrogram - clean speech spectrogram. 

![alt text](https://github.com/KaushalNaresh/Speech_enhancement_using_convolutional_autoencoders/blob/main/images/Unet_Model.png)

### Generating clean speech spectrogram and converting those back to .wav files

To Generate clean speech spectrograms we will use noise spectrograms generated from out model and subtract it from noisy speech spectrograms given as input. To convert log-spectrogram back to audio file we will first generate complex matrix by multiplying magnitude and its respective phase spectrograms and then use *librosa.core.istft* to generate time series data which can be converted back to .wav file using *soundfile.write*

![alt text](https://github.com/KaushalNaresh/Speech_enhancement_using_convolutional_autoencoders/blob/main/images/Unet_output.PNG)

[Input audio file](https://github.com/KaushalNaresh/Speech_enhancement_using_convolutional_autoencoders/blob/main/Output/input.wav)  

[Output audio file](https://github.com/KaushalNaresh/Speech_enhancement_using_convolutional_autoencoders/blob/main/Output/output.wav)

###  Calculate metrics to evaluate performance of the model

Finally to evaluate the performace of our model we will use WER (Word error rate), MER (Match error rate) and WIL (Word information lost) metrics. Thanks to my friend Simon who has prepared one notebook for the same [drive link](https://github.com/KaushalNaresh/Speech_enhancement_using_convolutional_autoencoders/blob/main/Source_code/Benchmark.ipynb).

 
>## References

1. Research papers for speech-enhancement [Link](https://paperswithcode.com/task/speech-enhancement)
2. Speech Enhancement In Multiple-Noise Conditions using Deep Neural Networks by Anurag Kumar, Dinei Florencio. [Link](https://arxiv.org/pdf/1605.02427.pdf)
3. A FULLY CONVOLUTIONAL NEURAL NETWORK FOR SPEECH ENHANCEMENT by Se Rim Park and Jin Won Lee. [Link](https://arxiv.org/pdf/1609.07132.pdf)
4. Speech Denoising DNN [Link](https://github.com/achaitu/SpeechDenoisingDNN?utm_source=catalyzex.com)
5. Sound Of AI youtube channel [Link](https://www.youtube.com/c/ValerioVelardoTheSoundofAI)
6. Digital signal processing [Link](https://brianmcfee.net/dstbook-site/content/intro.html)
7. Speech Enhancement [Link](https://github.com/vbelz/Speech-enhancement)
8. Unet Model [Link](https://analyticsindiamag.com/my-experiment-with-unet-building-an-image-segmentation-model/)
9. Why Skip Connections are needed in Unet [Link](https://theaisummer.com/skip-connections/)
10. Understanding Semantic Segmentation with Unet [Link](https://towardsdatascience.com/understanding-semantic-segmentation-with-unet-6be4f42d4b47)
11. Understanding Up Sampling [Link](https://naokishibuya.medium.com/up-sampling-with-transposed-convolution-9ae4f2df52d0)
12. Understanding Convolutional Neural Networks [Link](https://cs231n.github.io/convolutional-networks/)
13. Padding in CNN [Link](https://analyticsindiamag.com/guide-to-different-padding-methods-for-cnn-models/)
14. Denoising-images [Link](https://omdena.com/blog/denoising-images/)
