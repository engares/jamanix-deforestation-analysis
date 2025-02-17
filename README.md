# Deforestation Analysis of Jamanxim National Forest (2000-2019)

A detailed record of forest coverage changes on the Jamanix Forest from 2000 to 2019 using Computer Vision techniques.

---

<div> </div>
Satellite image analysis is a great tool for monitoring human-caused environmental impacts, such as deforestation in the Jamanxim National Forest, located in the Brazilian Amazon. This project presents a comparison of advanced Landsat image processing techniques for cleaning, reconstructing, segmenting, and quantifying deforested areas changes from 2000 to 2019.
<br></br>

<p align="center">
  <img src="https://github.com/engares/jamanix-deforestation-analysis/blob/main/deforestation_evolution.gif?raw=true" 
       alt="Deforestation Evolution" 
       width="400">
</p>

---

### Image Preprocessing & Cloud Removal
Satellite images often contain cloud interference, which affects segmentation accuracy. The following methods were applied:

1. **Thresholding & Gaussian Mixture Model (GMM) Segmentation**  
   - **Thresholding:** Applied to isolate high-intensity pixels associated with cloud cover. The threshold value was dynamically adjusted to account for variations in lighting conditions.  
   - **Gaussian Mixture Model (GMM) (Bi et al., 2017):** A probabilistic clustering approach that models the pixel intensity distribution using multiple Gaussian components. The probability density function (PDF) for a GMM with Gaussian components is given by:
   
        ```math
        P(x) = \sum_{i=1}^{k} w_i \mathcal{N}(x | \mu_i, \sigma_i^2)
        ```

        where the sum of `w_i` are the weights of each Gaussian component, such that N() is the Gaussian distribution with mean `μᵢ` and variance `σᵢ²`. The `Expectation-Maximization (EM)` algorithm is used to estimate the parameters iteratively.


<p align="center">
  <img src="https://github.com/engares/jamanix-deforestation-analysis/blob/main/images/analysis/cloud_segmentation.png" alt="Cloud removal via tresholding"width="600">
</p>


2. **HSV Color Space Filtering**
   - Clouds were segmented by adjusting brightness in the **V channel** and isolating low saturation regions on the HSV space:
       - Hue (H): Represents color type.
       - Saturation (S): Represents color intensity.
       - Value (V): Represents brightness.

<p align="center">
  <img src="https://github.com/engares/jamanix-deforestation-analysis/blob/main/images/analysis/HSV_exploration.png" alt="Example of HSV analysis" width="600">
</p>
     
3. **Image Inpainting for Reconstruction**, both from OpenCV:
   - **Navier-Stokes Method (Bertalmio et al. 2001):** Uses fluid dynamics equations to propagate textures.
   - **Telea’s Method (Telea A. 2004):** Fast interpolation-based approach.
  
<br></br>

---

  
### Deforestation Segmentation

To quantify deforestation, multiple segmentation techniques were compared:

<p align="center">
  <img src="https://github.com/engares/jamanix-deforestation-analysis/blob/main/images/analysis/deforestation_techniques.png" alt="Deforestation Seg. Techniques" width="600">
</p>

1. **Preprocessing**
   - **White Top-Hat Transformation** for background normalization:

     ```math
     WTH(f) = f_o - γ(f)
     ```
     where `γ(f)` represents the morphological opening of the image and `f_o` the original image.

   - **Gamma Correction** for contrast enhancement (`γ = 1.5`).

2. **Segmentation Techniques**
   - **Otsu’s Thresholding:** Finds an optimal threshold to separate vegetation from soil by minimizing intra-class variance:

     ```math
     σ_wT² = q_1T * σ_1T² + q_2T * σ_2T²
     ```
     where `q` represents class probabilities and `σ` is the variance.

   - **Gaussian Mixture Model (GMM):** Used too here for segmentation of the deforestated area

3. **Morphological Refinement**
   - **Closing Operation:** Merges disconnected deforested areas.
   - **Opening Operation:** Removes small misclassified regions.

4. **Deforestation Area Calculation**
   - Conversion to **km²** using scale **51 pixels ≈ 20 km**.
   - Comparative analysis of **Otsu, GMM, and refined masks**.

### Tools & Libraries
- Python 3.10, OpenCV, NumPy, Matplotlib, PIL, Scikit-learn  
- Data: Landsat Satellite Images (NASA) – 20 images (2000-2019)

--- 

### Results

As seen in the graph, the deforested area has shown a continuous increasing trend, surpassing 7000 km² in recent years. All three segmentation methods exhibit similar trends, with minor variations in magnitude. Otsu’s method tends to slightly overestimate the affected area, while refined segmentation produces more conservative estimates, aligning closely with GMM and reducing extreme fluctuations.

Significant deforestation acceleration is observed post-2005, with sharp peaks between 2015 and 2019, suggesting increased forest degradation activities. Reconstruction techniques introduced noise, leading to both under- and over-estimations, particularly visible in the **2018 dip** and the 2014 peak, both heavily influenced by cloud cover.

<p align="center">
  <img src="https://github.com/engares/jamanix-deforestation-analysis/blob/main/images/analysis/evolution.png" alt="Deforestation Seg. Techniques" width="600">
</p>

Refer to the paper (spanish) for additional info:

--- 


### Bibliography and inspiration: 

- Yvette. (2019, 28 agosto). La deforestación en Brasil alcanzó su nivel más alto en los últimos diez años. Noticias Ambientales. https://es.mongabay.com/2018/12/brasil-deforestacion-incendios-en-la-amazonia/
- M. Bertalmio, A. L. Bertozzi and G. Sapiro, "Navier-stokes, fluid dynamics, and image and video inpainting," Proceedings of the 2001 IEEE Computer Society Conference on Computer Vision and Pattern -Recognition. CVPR 2001, Kauai, HI, USA, 2001, pp. I-I, doi: 10.1109/CVPR.2001.990497
- Telea, A. (2004). An Image Inpainting Technique Based on the Fast Marching Method. Journal Of Graphics Tools, 9(1), 23-34. https://doi.org/10.1080/10867651.2004.10487596
- Bi, H., Tang, H., Yang, G., Shu, H., & Dillenseger, J. (2017). Accurate image segmentation using Gaussian mixture model with saliency map. Pattern Analysis And Applications, 21(3), 869-878. https://doi.org/10.1007/s10044-017-0672-1

