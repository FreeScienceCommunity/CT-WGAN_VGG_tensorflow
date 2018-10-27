# WGAN_VGG_tensorflow
Low Dose CT Image Denoising Using a Generative Adversarial Network with Wasserstein Distance and Perceptual Loss<br>

* WGAN_VGG
>	* paper : https://arxiv.org/pdf/1708.00961.pdf
>	* original code:  
>     * vgg : https://github.com/machrisaa/tensorflow-vgg  
>     * WGAN : https://github.com/jiamings/wgan


## I/O (DICOM file -> .npy)
* Input data Directory  
  * DICOM file extension = [<b>'.IMA'</b>, '.dcm']
> $ os.path.join(dcm_path, patent_no, [LDCT_path|NDCT_path], '*.' + extension)

## Ntwork architecture  
![Network architecture](https://github.com/hyeongyuy/WGAN_VGG_tensorflow/blob/master/img/network.jpg) 
* Generator
> * 8 conv layers (with relu)
> * kernel size : 3*3 
> * filters 
>   * first_7 : 32
>   * last : 1
* discriminator
> * str :  
>   * 6 conv layer(with leaky relu)  
>   -> fully connected : 1024 (with leaky relu)  
>   -> fully connected : 1 (cross entropy)  
> * filters:
>   * first_2 : 64
>   * mid_2 : 128
>   * last_2  : 256
>   * kernel size : 3*3
* Perceptual loss
> * pre-trained VGG net
> * feed : ground truth (generated image)
> * loss: L2 (ground truth)
> * gradient : only generator
## Training detail  
![procedure](https://github.com/hyeongyuy/WGAN_VGG_tensorflow/blob/master/img/procedure.jpg)
> * mini batch size L : 128
> * opt : Adam(alpha = 1e-5, beta1 = 0.5, beta2 = 0.9)
> * discriminator iter : 4
> * lambda(WGAN weight penalty) : 10
> * lambda1(VGG weight) : 0.1, lambda2 : 0.1
## Different
> [X] epoch : 100  -> iteration  
> [X] remove mostly air images  -> no remove (Hounsfield unit scale -> normalize 0 ~ 1)

## Main file(main.py) Parameters
* Training detail
> * num_iter : iterations (default = 200000)
> * alpha : learning rate (default=1e-5)
> * batch_size : batch size (default=128)
> * d_iters : discriminator iteration (default=4)
> * lambda_ : Gradient penalty term weight (default=10)
> * lambda_1 : Perceptual loss weight (in WGAN_VGG network) (default=0.1)
> * #lambda_2 : MSE loss weight(in WGAN_VGG network) (default=0.1, not used)
> * beta1 : Adam optimizer parameter (default=0.5)
> * beta2 : Adam optimizer parameter (default=0.9)

## Run
* train
> python main.py
* test
> python main.py --phase=test

## pretrained vgg
> * https://github.com/machrisaa/tensorflow-vgg
> * https://mega.nz/#!xZ8glS6J!MAnE91ND_WyfZ_8mvkuSa2YcA7q-1ehfSm-Q1fxOvvs
