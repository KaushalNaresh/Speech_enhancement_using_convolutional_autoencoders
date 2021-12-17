# Speech_enhancement_using_convolutional_autoencoders
This repository explores speech enhancement using Convolutional Autoencoders.

>## Table of Contents
1. [Description](#description)
2. [Model](#model)
3. [Output](#output)
4. [Further Queries](#further-queries)

>## Description

<div style="text-align: justify">
  
In this repository we explore how convolutional autoencoders can be used in speech enhancement. For experimental purposes I have taken [MS-SNSD dataset](https://github.com/microsoft/MS-SNSD). You can use this [drive link](https://drive.google.com/drive/folders/1Pzp3zh7JbEig59Oom_gxhmQ2MGx5ShCa?usp=sharing) which has noisy-speech and their corresponding noise and clean .wav files generated using the script provided in the MS-SNSD repository. I have mainly used librosa to work with sound files and to design Convolutional-Autoencoders Keras and Tensorflow libraries are used.  

</div>

>## Model

<div style="text-align: justify">
  
  Audios can be represented in many ways but the 2 most important forms that I have used in my project are the waveforms and log-spectrograms. To begin with I first converted all .wav files into their corresponding time-series data. This kind of data has no information regarding frequency components. So to analyse the frquency components we always seek help from Fourier transormations but what kind of transformation to use. Here I have used short-time fourier transform (STFT) which gives the clear picture of how frequency components evolve with time. Since humans preception of sound is logarithmic so we will convert our spectrograms into log-spectrograms i.e. convert power into decibels. We can interpret these spectrograms as images and feed them to Convolutional autoencoders for denoising. Our model takes noisy-speech spectrograms (X) as input and noise-spectrograms (Y) as output. We can generate clean speech spectrograms using (X-Y). These clean-speach spectrograms generated from the model can be converted back to time series data and .wav file using librosa.
  
![alt text](https://github.com/KaushalNaresh/Speech_enhancement_using_convolutional_autoencoders/blob/main/images/Unet_output.PNG)
  
 </div>
