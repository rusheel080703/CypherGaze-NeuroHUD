# 🤖 CypherGaze: NeuroHUD

**A real-time, AI-powered emotion detector with a futuristic cyberpunk HUD overlay, built with OpenCV and ONNX.**

This project captures a live webcam feed, performs real-time face detection, analyzes facial expressions using a deep learning model, and renders the detected emotion (e.g., HAPPY, SAD, ANGER) inside a custom-designed, glitch-style Head-Up Display (HUD).

### 📸 Project Demo

| HAPPY | ANGRY | SURPRISE |
| :---: | :---: | :---: |
| ![Happy Detection](-AI-Face-Emotion-Persona-Overlay-main/ai_face_persona/assets/happy.png) | ![Angry Detection](-AI-Face-Emotion-Persona-Overlay-main/ai_face_persona/assets/angry.png) | ![Surprise Detection](-AI-Face-Emotion-Persona-Overlay-main/ai_face_persona/assets/surprise.png) |

---

## 📋 Table of Contents

1.  [🧠 Core Functionality & Real-Time Pipeline](#-core-functionality--real-time-pipeline)
2.  [🛠 Tech Stack](#-tech-stack)
3.  [💻 Setup & Execution](#-setup--execution)
4.  [📁 Project Structure (Detailed)](#-project-structure-detailed)
5.  [🧑‍💻 Author](#-author)

---

## 🧠 Core Functionality & Real-Time Pipeline

The application operates as a continuous, multi-stage pipeline that processes each frame from the webcam in real-time.

1.  **Capture:** The `main.py` script initializes the primary webcam feed using `cv2.VideoCapture`. It then begins a `while` loop to read frames continuously.

2.  **Face Detection:** Each captured frame is passed to the `FaceDetector` class instance. This module uses OpenCV's Deep Neural Network (`cv2.dnn`) functionality to find and return the bounding boxes for all faces present in the frame.

3.  **Emotion Analysis & Inference:**
    * **Pre-processing:** For each detected face, the bounding box coordinates are used to extract the Region of Interest (ROI). This face ROI is then pre-processed by the `EmotionModel` class: it's converted to grayscale, resized to the model's required input dimensions, and normalized.
    * **Inference:** The processed image array is fed into an **ONNX Runtime** inference session, which loads and executes the `emotion_model.onnx`.
    * **Decoding:** The model's output (an array of logits) is decoded. An `argmax` function finds the index with the highest score, which is then mapped to a human-readable emotion label from the class list: `['NEUTRAL', 'HAPPY', 'SURPRISE', 'SAD', 'ANGER', 'DISGUST', 'FEAR']`.

4.  **HUD Rendering:** The final frame, the face bounding box, and the predicted emotion string are passed to the `draw_overlay` function in `overlay_utils.py`. This utility performs two key actions:
    * **Border:** It loads the transparent `hud_border.png` and uses **Pillow (PIL)** and OpenCV for precise alpha blending to composite it onto the frame.
    * **Text:** It uses `Pillow`'s `ImageDraw` to render the emotion text using the custom `glitch_font.ttf`, which is then blended onto the frame.

5.  **Display:** The final, composited frame (video + HUD) is displayed in a window using `cv2.imshow()`. The loop continues until the user presses **'q'** to quit.

---

## 🛠 Tech Stack

| Component | Technology | Role in Project |
| :--- | :--- | :--- |
| **AI Inference** | **ONNX Runtime** | Runs the pre-trained `.onnx` model for fast, cross-platform emotion classification. |
| **Computer Vision** | **OpenCV** | Handles webcam I/O, `cv2.dnn` for face detection, image processing (resize, grayscale), and final frame display. |
| **Image Rendering** | **Pillow (PIL)** | Used specifically for its powerful font rendering engine to draw custom `.ttf` fonts onto image overlays. |
| **Numerical** | **Numpy** | Facilitates array manipulation, pre-processing, and handling image data as numerical arrays. |
| **Core Language**| **Python 3.x** | The core language orchestrating all modules and the main application loop. |

---

## 💻 Setup & Execution

### 1️⃣ Prerequisites

* Python 3.8 or newer.
* A webcam compatible with OpenCV.

### 2️⃣ Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/rusheel080703/cyphergaze-neurohud.git](https://github.com/rusheel080703/cyphergaze-neurohud.git)
    ```
2.  **Navigate to the project's execution directory:**
    * *Note: The main script is inside a nested folder.*
    ```bash
    cd cyphergaze-neurohud/-AI-Face-Emotion-Persona-Overlay-main/ai_face_persona
    ```
3.  **Install all required dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    

### 3️⃣ Execution

From within the `ai_face_persona` directory, run the `main.py` script:

```bash
python main.py
````

**Output:** A window will launch, displaying your webcam feed with the real-time emotion HUD overlaid. Press the **'q'** key on your keyboard to stop the application.

-----

## 📁 Project Structure (Detailed)

| File/Folder | Description |
| :--- | :--- |
| `main.py` | **Main application entry point.** Orchestrates the full pipeline: initializes the camera, calls the detector and model, and passes data to the renderer. |
| `face_detector.py` | A class-based module that **loads a Caffe-based face detector** (`cv2.dnn`) and contains the `detect_faces` method to find face bounding boxes in frames. |
| `emotion_model.py` | A class-based module that **loads the ONNX model** and manages the `onnxruntime` inference session. Includes pre-processing logic and emotion label mapping. |
| `overlay_utils.py` | A utility module that **performs all rendering** for the HUD. It uses `Pillow` for text rendering and `OpenCV` for alpha blending images. |
| `assets/emotion_model.onnx` | The **pre-trained neural network** (in ONNX format) for 7-class emotion classification (Neutral, Happy, Surprise, etc.). |
| `assets/hud_border.png` | The **transparent PNG asset** for the cyberpunk HUD border overlay. |
| `assets/glitch_font.ttf` | The custom **"glitch" style font** (`.ttf`) file for rendering the emotion text in a stylized way. |
| `requirements.txt` | A list of all required Python packages (`numpy`, `opencv-python`, `Pillow`, `onnxruntime`). |

-----

## 🧑‍💻 Author

**Rusheel Vijay Sable**

  * **Role:** Full Stack & AI Developer | Computer Vision Enthusiast
  * **Motto:** "Building augmented reality, one emotion at a time."

