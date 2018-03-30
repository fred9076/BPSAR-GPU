# BPSAR-GPU
The Back-projection (Beam Formation) Imaging Algorithm for Synthetic Aperture Radar based on CUDA acceleration.2

![BPA for Ship](/Docu/BP_Ship.png) ![BPA for Flight](/Docu/a380_AJTF_whole.png)[1]

(`Figures generated by the BPSAR-GPU. SAR raw data simulated by electromagnetic software developed in [2]`)

**Author**: heTiHoUDAi.
***

## Introduction

The backprojection (BP) or so called Beam Formation (BF) Imaging Algorithm is designed in this project. The BP algorithm firstly is stated in the CT area, then the same idea is used in the synthetic aperture radar (SAR).

The Key idea of BP algorithm is coherently calculating the contribution of each pulse to each pixels, which comprehensively states in Duersch PhD dissertation [3] or Jakowatz's paper [4]. Comparing to the well known Range-Doppler algorithm for ISAR/SAR, the most important advantage of the BP algorithm is non-distortion in wide angle, in other word, extremely high cross range resolution. However, the BP algorithm is extremely time-consumption. Even though several BP algorithm based on the sub-aperature algorithm by UCBerkely, the BPA is still to slow for very large scene.

Thus, in this project, I designed the BPA algorithm based on the CUDA acceleration.
***

## Theory Description.

If the transmitted signal is linear-frequency modulation, then, the fourier transform of it is the scaled range signal s_r_i(t), in which, i donates the ith pulse and r means range signal. 

For given BP algorithm imaging map I(x,y), in which x and y is discrete signal, the complex data of I(x,y) is the coherent contribution of every pulse. For example, at pulse n, the radar locates in (x_r,y_r). The distance between the point in the imaging map (x0,y0) and the (x_r,y_r) is d. Then the data s_r_n(d) is contribuated by the scatterer in the x0,y0. Do the coherent accumulation for given point for every node, we have the image of the given point

I(x,y) = \sim_{i=1}^N 2f0/pi s_r_i(d((x,y),(x_r,y_r)))

When the BP is processing a 2-D image, we need 3 cycles for the whole problem. Thus, it is a O(N^3) problem. Even though there is a lot of fast BP-SAR imaging algorithm has been developed by USBerkely, it still can't give satisfied solution in very short time. Thanks to the development of the GPU, we can accelerate the BP-SAR imaging algorithm. 

Here, we use the GPU core to compute the pixel simultaneously, which will led to a very good result.

## Requirement

The project is compiled in VS2015 with CUDA 8.0. Due to the stability of CUDA, author can't guarantee that this BPSAR-GPU would work or give correct answer. 

The author's platform is Intel i7 4790k, 8GB and GTX970. There is a macro which, when the number of CUDA cores is not sufficient, user could adjust the number of simultaneously running cores. (`TODO: The code automaticaly recoginizes the graphic card and changes cores used.`) 

However, the code is still very memory-consumption, especially for the GPU memory. That is because the BPA needs to calculate the FFT for every pulse with N multiple points in order to give a better smooth image. Thus, when the code gives all-zero or all-non-sense result, please low down the multiplier N or consider change a GPU. (`TODO: Author is thinking adding new function to use the time in exchange the memory`)

## Input & Output File Format

TODO

***
**Reference**

[1] Wang, Z., Yang, W., Chen, Z., Zhao, Z., Hu, H., & Qi, C. (2018). A Novel Adaptive Joint Time Frequency Algorithm by the Neural Network for the ISAR Rotational Compensation. Remote Sensing, 10(2), 334.

[2] Yang, W., Kee, C. Y., & Wang, C. F. (2017). Novel Extension of SBR–PO Method for Solving Electrically Large and Complex Electromagnetic Scattering Problem in Half-Space. IEEE Transactions on Geoscience and Remote Sensing, 55(7), 3931-3940.

[3] Duersch, M. I. (2013). Backprojection for synthetic aperture radar. Brigham Young University.

[4] Jakowatz, C. V., Wahl, D. E., & Yocky, D. A. (2008, April). Beamforming as a foundation for spotlight-mode SAR image formation by backprojection. In Algorithms for Synthetic Aperture Radar Imagery XV (Vol. 6970, p. 69700Q). International Society for Optics and Photonics.
