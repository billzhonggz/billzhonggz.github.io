---
layout: default
---

# Scene Synthesis and Manipulation

> This project was done during Junru's master study at CUHK. Supervised by Prof. Bolei Zhou and Mr. Ceyuan Yang at Department of Information Engineering of CUHK.

![](/assets/img/scene-manipulation/overview.png)

## Overview

Like the draw above, in this task, we want to train a *generative model* which allows us to synthesize and manipulate realistic images with controls. From this simple description, we can find the following challenges,

- we need a generative model, and get it trained properly,
- we need some method to manipulate the synthesized images from the model above,
- abridge the gap between synthesized images and real photo spaces.

To overcome these, we made use of some previous works like StyleGAN2 [1], HiGAN [2], and pix2pixHD [3], to build up the pipeline and get this task done.

In general, the pipeline is divided to the following steps,

1. train a generator with some real-world photos,
2. get synthesized photos from the above trained generator,
3. use the synthesized images and latent codes to train HiGAN [2] boundaries in some attributes,
4. move the latent codes along the boundaries, get *manipulated* latent codes,
5. use the manipulated latent codes, feed to generator, get manipulated *synthesized images*,
6. train image translators (pix2pixHD [3]) with *manipulated* and *original* synthesized images,
7. use translators to translate *real world photos* with the changes in synthesized image space.

The key part in this pipeline is HiGAN [2] and a similar work InterFaceGAN [4]. They proposed a novel method to manipulate synthesize images directly on latent codes of a pre-trained generator rather than train new generators for manipulations.

In this particular task, we used streetview images as the training set. But the model is proved to generalize to other scenes manipulation tasks like indoor scenes [2].

## Train Boundaries

![](/assets/img/scene-manipulation/train-boundaries.png)

After we have 500,000 synthesized images from the trained generator, we trained boundaries on the attributes listed in SUNAttribute dataset [8] with Places365 CNN attribute predictor pre-trained on Places365 dataset [7]. In this step, the attribute predictor response for generating scores on each attribute for each synthesized image.

For the training of the boundaries, we randomly sampled 2000 latent codes and corresponding images and scores for each boundary. The boundaries was treated as binary classification tasks in order to find the SVM hyperplane that separate *exist* and *not exist* for one attribute in the space of latent codes (we use W latent space in StyleGAN2 [1]).

## Manipulating with Boundaries

![](/assets/img/scene-manipulation/manipulate-latent-codes.png)



## References

[1] T. Karras, S. Laine, M. Aittala, J. Hellsten, J. Lehtinen and T. Aila, "Analyzing and Improving the Image Quality of StyleGAN," arXiv perprint, arXiv: 1912.04958. 

[2] C. Yang, Y. Shen and B. Zhou, "Semantic Hierarchy Emerges in Deep Generative Representations for Scene Synthesis," arXiv preprint arXiv:1911.09267, 2019. 

[3] T.-C. Wang, M.-Y. Liu, J.-Y. Zhu, A. Tao, J. Kautz and B. Catanzaro, "High-Resolution Image Synthesis and Semantic Manipulation with Conditional GANs," in CVPR, 2018. 

[4] Y. Shen, J. Gu, X. Tang and B. Zhou, "Interpreting the Latent Space of GANs for Semantic Face Editing," in CVPR, 2020. 

[5] F. Zhang, B. Zhou, L. Liu, Y. Liu, H. H. Feng, H. Lin and C. Ratti, "Measuring human perceptions of a large-scale urban region using machine," Landscape and Urban Planning, vol. 180, pp. 148-160, 2018. 

[6] Y. Viazovetskyi, V. Ivashkin and E. Kashin, "StyleGAN2 Distillation for Feed-forward Image Manipulation," arXiv preprint arXiv:2003.03581, 2020. 

[7] B. Zhou, A. Lapedriza, A. Khosla, A. Oliva and A. Torralba, "Places: A 10 Million Image Database for Scene Recognition," IEEE Transactions on Pattern Analysis and Machine Intelligence, vol. 40, no. 6, pp. 1452-1464, 2018. 

[8] G. Patterson, C. Xu, H. Su and J. Hays, "The SUN attribute database: Beyond categories for deeper scene understanding," International Journal of Computer Vision, vol. 108, no. 1-2, pp. 59-81, 2014. 

[9] T. Karras, S. Laine and T. Aila, "A Style-Based Generator Architecture for Generative Adversarial Networks," arXiv preprint arXiv: 1812.04948, 2018. 