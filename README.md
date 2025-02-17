# Deforestation Analysis of Jamanxim National Forest (2000-2019)

A detailed record of forest coverage changes on the Jamanix Forest from 2000 to 2019 using Computer Vision techniques.

<div> </div>
Satellite image analysis is a key tool for monitoring human-caused environmental impacts, such as deforestation in the Jamanxim National Forest, located in the Brazilian Amazon. This project presents a comparison of advanced Landsat image processing techniques for cleaning, reconstructing, segmenting, and quantifying deforested areas. The study provides a detailed record of forest coverage changes from 2000 to 2019.****

<p align="center">
  <img src="https://github.com/engares/jamanix-deforestation-analysis/blob/main/deforestation_evolution.gif?raw=true" alt="Deforestation Evolution">
</p>

### Image Preprocessing & Cloud Removal
Satellite images often contain cloud interference, which affects segmentation accuracy. The following methods were applied:

1. **Thresholding & Gaussian Mixture Model (GMM) Segmentation**  
   - **Thresholding:** Applied to isolate high-intensity pixels associated with cloud cover. The threshold value was dynamically adjusted to account for variations in lighting conditions.  
   - **Gaussian Mixture Model (GMM):** A probabilistic clustering approach that models the pixel intensity distribution using multiple Gaussian components.


2. **HSV Color Space Filtering**
   - Clouds were segmented by adjusting brightness in the **V channel** and isolating **low saturation regions**.
  
<p align="center">
  <img src="https://github.com/engares/jamanix-deforestation-analysis/blob/main/images/analysis/HSV_exploration.png" alt="Example of HSV analysis">
</p>
     
3. **Image Inpainting for Reconstruction**
   - **Navier-Stokes Method:** Uses fluid dynamics equations to propagate textures.
   - **Telea’s Method:** Fast interpolation-based approach.
  
### Deforestation Segmentation

To quantify deforestation, multiple segmentation techniques were compared:

<p align="center">
  <img src="https://github.com/engares/jamanix-deforestation-analysis/blob/main/images/analysis/deforestation_techniques.png" alt="Deforestation Seg. Techniques">
</p>

1. **Preprocessing**
   - **White Top-Hat Transformation** for background normalization:

     ```
     WTH(f) = f_orig - γ(f)
     ```
     where `γ(f)` represents the morphological opening of the image.

   - **Gamma Correction** for contrast enhancement (`γ = 1.5`).

2. **Segmentation Techniques**
   - **Otsu’s Thresholding:** Finds an optimal threshold to separate vegetation from soil by minimizing intra-class variance:

     ```
     σ_wT² = q_1T * σ_1T² + q_2T * σ_2T²
     ```
     where `q` represents class probabilities and `σ` is the variance.

   - **Gaussian Mixture Model (GMM):** Models pixel intensities with multiple Gaussian functions for improved differentiation.

3. **Morphological Refinement**
   - **Closing Operation:** Merges disconnected deforested areas.
   - **Opening Operation:** Removes small misclassified regions.

4. **Deforestation Area Calculation**
   - Conversion to **km²** using scale **51 pixels ≈ 20 km**.
   - Comparative analysis of **Otsu, GMM, and refined masks**.

### Tools & Libraries
- **Python 3.10**  
- **Libraries:** OpenCV, NumPy, Matplotlib, PIL, Scikit-learn  
- **Data:** Landsat Satellite Images (NASA) – 20 images (2000-2019)
