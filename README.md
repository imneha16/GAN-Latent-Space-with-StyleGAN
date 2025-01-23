# Exploring-the-GAN-Latent-Space-with-StyleGAN
The goal of this project is to implement StyleGAN using PyTorch, train it on the given dataset and explore its latent space to generate high-quality facial images. 
2. Model Architecture
2.1 Generator
	The StyleGAN generator is different from typical GANs in many ways.
Mapping Network. A fully connected network that maps the input latent vector z to an intermediate latent space w. This decouples the latent from the generator and enables better disentanglement.
Adaptive Instance Normalization (AdaIN). Features modification was introduced in each convolutional layer. AdaIN does this through a combination of normalized feature maps with the style vector ww. It allows for fine control over such generated attributes like pose, texture, or lighting.
Progressive Growing. The generator progressively increases the resolution of output images starting from 4×4, increasing through 8×8, 16×16 and stopping at 128×128. This approach allows for a more stable training process with lower computational complexity for higher resolutions.
2.2 Discriminator
	The discriminator assesses the realism of the images and gives higher scores to real images. It mirrors the generator's progressive architecture, adapting to the resolution at each training phase. Gradient penalties and feature blending ensure robust performance across resolutions.
2.3 Loss Functions
Wasserstein GAN Loss with Gradient Penalty. A modified GAN loss that improves training stability and avoids vanishing gradients.
Generator Loss. It encourages the generator to produce images that are indistinguishable from real ones.
Discriminator Loss. Optimized to maximize the difference between real and generated samples while enforcing Lipschitz continuity through gradient penalties.
2.4 Hyperparameters
Latent Space Dimensionality. Z_DIM=512, W_DIM=512
Learning Rates. 1e−3 is for general parameters and 1e−5 is for mapping layers.
Batch Sizes. Progressively reduced as the resolution increased to avoid memory bottlenecks.
3. Experimental Setup
3.1 Dataset
	The dataset consists facial images preprocessed to fit into the progressive training pipeline. The images were first down sampled to 4×4 resolution and then scaled upward during training at higher resolutions of up to 128×128.
3.2	Training Details
Alpha Blending. It regulated how a resolution would be replaced by another through progressive interpolation between outputs of the present and the next higher resolution.
Optimizers. It uses Adam optimizers with the setting of (β1,β2)=(0,0.99) for both generator and discriminator.
Epochs. The number of training epochs depended on resolution, the higher the resolution the more epochs it required for convergence.
4. Results and Analysis
4.1 Latent Space Exploration
Linear Interpolation. By interpolating random latent vectors it showed that generated features such as gender, facial expressions, and hairstyles produced smooth transitions.
Feature Disentanglement. AdaIN layers showed clear disentanglement that allowed for independent control over attributes such as background, texture and pose.
4.2 Quantitative Metrics
Fréchet Inception Distance (FID). The FID scores improved as training progressed to higher resolutions. At 4 × 4: FID=120.5 and at 64 × 64: FID=43.4. The lower FID scores indicated better alignment between real and generated image distributions.
Perceptual Path Length (PPL). PPL scores indicated smooth latent space interpolation, thus confirming the disentanglement power of StyleGAN.

