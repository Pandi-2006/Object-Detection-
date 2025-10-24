# Tesla Autopilot Clone: Object Detection Project

This project is an implementation of a real-time object detection system, similar to the vision component used in systems like Tesla Autopilot. It uses the YOLOv8 model to identify and track objects such as cars, pedestrians, and traffic lights in video footage.



## Project Overview

The goal of this project is to detect, classify, and track common road objects from a video stream. This is a fundamental task for any autonomous driving system, providing the "eyes" for the vehicle to understand its environment.

### Approach Used: YOLOv8

The core of this project is the **YOLOv8 (You Only Look Once)** object detection model, developed by Ultralytics.

* **Why YOLO?** YOLO is a state-of-the-art, single-stage object detector. It is renowned for its incredible speed and accuracy, making it ideal for real-time applications (like a car) where decisions must be made in milliseconds.
* **Pre-trained Model:** This project uses a YOLOv8 model pre-trained on the **COCO (Common Objects in Context)** dataset. This powerful dataset includes 80 common object classes, such as:
    * `person`
    * `bicycle`
    * `car`, `motorcycle`, `bus`, `truck`
    * `traffic light`
    * `stop sign`

## Dataset and Preprocessing

* **Input:** The model accepts standard video files (e.g., `.mp4`, `.avi`) or can be run on a live webcam feed.
* **Preprocessing:** The `ultralytics` library automatically handles all necessary preprocessing. Input frames are resized, normalized, and converted into the tensor format required by the YOLOv8 model before detection.

*(Optional - For Custom Training)*
* **Further Improvement:** To improve performance specifically for driving, the model could be fine-tuned on a specialized dataset like **BDD100K (Berkeley DeepDrive)** or **KITTI**, which contain a wider variety of road conditions and objects.

## Installation

To run this project, you need Python 3.8+ and the following libraries.

1.  **Create a virtual environment (Recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```

2.  **Install the required packages:**
    Create a file named `requirements.txt` and add the following:
    ```
    ultralytics
    opencv-python-headless
    requests
    ```
    Then, install them using pip:
    ```bash
    pip install -r requirements.txt
    ```

## How to Run the Project

The script is set up to automatically download a test video and save the processed output.

1.  **Run the script:**
    ```bash
    python object_detecion.ipynb
    ```
2.  **How it works:**
    * The script will first download a sample video named `driving.mp4`.
    * It will load the `yolov8n.pt` model.
    * It will process the video frame-by-frame, drawing bounding boxes on all detected objects.
    * A new video file named `output.mp4` (or `output_H264.mp4`) will be created in your project folder, containing the final video with all the detections.

## Results and Insights

The model successfully identifies common road objects (especially cars) in real-time with high confidence.

### Performance Evaluation Metrics

To evaluate this model properly, we use several key metrics:

* **IoU (Intersection over Union):** This measures the overlap between a predicted bounding box and the actual ground-truth box. An IoU > 0.5 is typically considered a successful detection.
* **Precision:** The accuracy of the positive predictions. (Of all the "cars" the model detected, how many were *actually* cars?)
* **Recall:** The model's ability to find all positive instances. (Of all the *actual* cars in the frame, how many did the model *find*?)
* **mAP (mean Average Precision):** The primary metric for object detection, which calculates the average precision across all classes and various IoU thresholds.

### Challenges and Potential Improvements

* **Challenge: Small/Distant Objects:** The model (especially the 'nano' version) can struggle to detect cars or pedestrians that are very far away.
    * **Improvement:** Use a larger model like `yolov8m.pt` (medium) or `yolov8l.pt` (large). This is a trade-off: they are more accurate but run slower.

* **Challenge: Occlusion:** Objects that are partially hidden (e.g., a car behind a truck) are sometimes missed.
    * **Improvement:** Implement **object tracking** (like DeepSORT or BoT-SORT). A tracker can follow an object and predict its position even if it's hidden for a few frames.

* **Challenge: Video Codecs:** The default `mp4v` codec used by OpenCV isn't always playable on all computers.
    * **Improvement:** The code can be modified to use a more universal codec like `avc1` (H.264) to ensure the output video is easy to play.
