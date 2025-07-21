# Realistic Person-Scene Compositing using Light and Shadow Analysis

This project presents a comprehensive framework for seamlessly integrating a person and their natural shadow from one image into a completely different background scene. By focusing on the physics of light and shadow, the algorithm moves beyond simple cut-and-paste techniques to achieve photorealistic results.

-----

## 🌟 Key Features

  * **Shadow-Preserving Extraction**: Isolates the person from their original background while carefully retaining their cast shadow, which is crucial for a realistic composite.
  * **Intelligent Light Analysis**: Automatically estimates the direction and intensity of the primary light sources in both the source and target images.
  * **Shadow Transformation & Realism**: Realistically transforms the extracted shadow—rotating, scaling, and skewing it—to match the lighting environment of the new scene. It also adjusts for shadow softness (hard vs. soft shadows).
  * **Advanced Color Harmonization**: Synchronizes the color and tonal profiles of the person with the new background using statistical color transfer and histogram matching to ensure a natural blend.
  * **Seamless Edge Blending**: Utilizes feathered masking and alpha compositing to eliminate hard, artificial edges, making the integration appear smooth and believable.
  * **Atmospheric Effects**: Simulates depth by applying an atmospheric haze effect for subjects placed at a distance in the new scene.

-----

## 🛠️ How It Works: The 5-Step Process

The algorithm achieves its realistic output through a structured, multi-step process that analyzes and harmonizes both images before integration.

### Step 1: Person and Shadow Extraction

First, the person and their corresponding shadow are meticulously isolated from the original image.

  * **Person Masking**: An AI-powered tool (`rembg`) generates a precise mask of the person.
  * **Shadow Detection**: Adaptive thresholding and morphological operations are applied to isolate dark regions. Crucially, only the shadows physically connected to the person's silhouette are kept, ensuring no irrelevant dark areas are extracted.
  * **Combined Output**: The person mask and the shadow mask are merged to create a single foreground element.

| Original Image | Person Mask | Shadow Mask | Extracted Person + Shadow |
| :---: | :---: | :---: | :---: |
|  |  |  |  |

### Step 2: Background Scene Analysis

To ensure the inserted person looks like they belong, the algorithm conducts a thorough analysis of the target scene's lighting.

  * **Shadow Detection**: The image is converted to the **LAB color space** to accurately detect all existing shadows.
  * **Shadow Classification**: Detected shadows are classified as "hard" (sharp edges) or "soft" (blurry edges) by analyzing their edge characteristics with a Canny edge detector. This information dictates how the new shadow will be rendered.

| Background Scene | All Shadows | Hard Shadows | Soft Shadows |
| :---: | :---: | :---: | :---: |
|  |  |  |  |

### Step 3: Light Source Triangulation

The direction and intensity of the dominant light source are estimated for both images.

  * **Vector Analysis**: For images with clear shadows, the algorithm analyzes the vector from an object's base to its shadow's tip.
  * **Brightness Analysis**: If shadows aren't clear, a quadrant-based brightness analysis approximates the light source's general direction.
  * **Result**: The system calculates a light direction angle and intensity for both scenes, which is essential for transforming the shadow.

*Person light: 331° @ 0.38 intensity*
*Background light: 32° @ 0.06 intensity*

### Step 4: Color Harmonization and Blending

To avoid a jarring, "pasted-on" look, the foreground element's color profile is adjusted to match the background.

  * **Statistical Color Transfer**: The person's color profile is matched to the background using the **Reinhard color transfer algorithm** in the LAB color space.
  * **Histogram Matching**: The tonal distribution of the person is aligned with the background's histogram.
  * **Luminance Adjustment**: The person's brightness is scaled to match the new scene.

| Original Person | Histogram Matched | Color Transferred |
| :---: | :---: | :---: |
|  |  |  |

### Step 5: Final Compositing and Refinement

The final step involves placing the harmonized person and transformed shadow into the scene and perfecting the blend.

  * **Shadow Transformation**: The shadow is rotated, scaled, and skewed based on the light analysis from Step 3. Its opacity and blur are adjusted to match the background's hard or soft shadows.
  * **Edge Blending**: A feathered mask (using Gaussian blur) is applied to the person and shadow to soften the transition to the background, creating a seamless final image.

| Original Shadow | Transformed Shadow | Soft Shadow |
| :---: | :---: | :---: |
|  |  |  |

-----

## 🚀 Final Result

The culmination of these steps is a high-quality composite where the person and their shadow are believably integrated into the new environment.

| Original Background | Harmonized Person | Final Composite |
| :---: | :---: | :---: |
|  |  |  |

-----

## 💻 Technical Details

This project leverages a combination of classical computer vision techniques and modern AI-powered tools.

  * **Core Libraries**: OpenCV, Pillow, scikit-image, NumPy
  * **AI-Powered Segmentation**: `rembg` library
  * **Key Algorithms**:
      * **Shadow Detection**: Adaptive Thresholding in the LAB color space
      * **Color Matching**: Reinhard et al.'s statistical color transfer method
      * **Edge Blending**: Alpha compositing with Gaussian feathering
      * **Noise Reduction**: Morphological Operations (opening and closing)

-----

## 🔮 Future Directions

This work lays a strong foundation for several exciting advancements:

1.  **Deep Learning for Shadows**: Employing generative neural networks (GANs) for more accurate and dynamic shadow generation.
2.  **Complex Lighting**: Expanding the system to handle scenes with multiple light sources.
3.  **3D Scene Awareness**: Integrating depth estimation to cast shadows more accurately on uneven surfaces and complex geometry.
4.  **Material-Based Rendering**: Recognizing surface properties (e.g., water, glass, grass) to adjust how light reflects and shadows are cast.
5.  **Automated Optimal Placement**: Using AI to analyze the scene and suggest the most aesthetically pleasing and physically plausible position for the subject.
