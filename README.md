# FitMate - A Virtual Gym Trainer App

FitMate is a virtual personal gym trainer application that utilizes computer vision and machine learning technologies to track your exercise form in real-time. It provides instant feedback on your poses, angles, and repetitions, helping users improve their workout efficiency and form.

## Table of Contents

- [About The Project](#about-the-project)
  - [Built With](#built-with)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Contact](#contact)
- [Acknowledgments](#acknowledgments)

---

## About The Project

FitMate is designed to act as a virtual personal gym trainer, leveraging advanced pose estimation techniques to detect users' exercise poses and provide real-time feedback. It distinguishes user poses by evaluating pose landmarks and vector geometry, offering valuable insights to improve form and efficiency.

### Key Features:
- Pose detection for various exercises.
- Real-time feedback on pose accuracy, angles, and repetition counts.
- Helps users enhance workout form, reduce the risk of injury, and optimize performance.

### Built With

- [Flask](https://flask.palletsprojects.com)
- [Mediapipe](https://mediapipe.dev/)
- [OpenCV](https://opencv.org/)
- [Bootstrap](https://getbootstrap.com/)

---
## Website Recording
[![Watch the video](https://img.youtube.com/vi/CMliL6Nh_Fw/hqdefault.jpg)](https://www.youtube.com/watch?v=CMliL6Nh_Fw)


## Getting Started

Follow the instructions below to set up and run the application on your local machine.

### Prerequisites

Ensure you have the following installed on your system:
- Python 3.10.2 or later

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/syedalihn/FitMate.git
   ```

2. **Navigate to the project directory**
   ```bash
   cd FitMate
   ```

3. **Install required Python packages**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the application**
   ```bash
   python app.py
   ```

5. **Access the application in your browser**
   - Go to: `http://localhost:3000`

---

### Workflow Explanation of the Flask Application

This Flask application serves as a virtual personal trainer, utilizing computer vision and machine learning techniques to count repetitions for various exercises like curls, squats, pull-ups, and more. The following is a step-by-step breakdown of how the code works:

---

### 1. **Import Libraries and Initialize Flask**
The necessary libraries are imported:
- `Flask`: To handle routing and serving web pages.
- `cv2`: OpenCV for video capture and frame processing.
- `mediapipe`: For pose detection and drawing utilities.
- `numpy`: For mathematical operations, especially to calculate the angle of joints.

The `Flask` application object is created using `app = Flask(__name__)`.

---

### 2. **Helper Function: `calculate_angle`**
The `calculate_angle` function takes three points (a, b, c), representing joint coordinates, and calculates the angle at point `b`. This is achieved through trigonometry using the arctangent of the slopes between the points.

**How it works:**
- Convert points `a`, `b`, and `c` into numpy arrays.
- Use `np.arctan2` to calculate the difference in slopes and derive the angle in radians.
- Convert radians into degrees and return the calculated angle.

---

### 3. **Video Frame Processing for Curls: `curls_frame()`**
This function continuously captures frames from the webcam, processes them using Mediapipe's pose detection, and renders feedback (angles and repetition counts) on the frames. It also flips the video horizontally for a mirrored effect.

**How it works:**
- **Capture Frame:** It reads a video frame from the webcam and flips it to provide a mirror view.
- **Pose Detection:** Convert the frame to RGB and pass it to `pose.process()` to detect body landmarks.
- **Extract Landmarks:** Try to extract specific joint landmarks for both left and right arms (shoulder, elbow, and wrist).
- **Angle Calculation:** Calculate the angle at the elbow joint for both arms using the `calculate_angle` function.
- **Repetition Logic:** Count repetitions based on arm angles:
  - When the elbow angle is >170 degrees, the arm is "down".
  - When the angle is <15 degrees and the previous state was "down", increment the repetition count and set the state to "up".
- **Rendering Feedback:** The angles and repetition counts are rendered onto the frame using OpenCV's `putText` function, and a rectangle box is drawn to display the current stage and reps for both left and right arms.
- **Render Landmarks:** The body pose landmarks are drawn on the frame using Mediapipe's drawing utilities.

The processed frame is saved as a JPEG image and sent to the browser using Flask's `Response` object with a `multipart/x-mixed-replace` mimetype to simulate live video streaming.

---

### 4. **Flask Routes for Web Pages**
Several routes are defined to render different HTML templates and video feeds for different exercises:

- **`/`:** This route renders the homepage, which is `index.html`.
- **`/curls`, `/squats`, `/pullups`, `/raise`, `/jacks`, `/pushups`:** These routes render specific HTML templates for each type of exercise. Each HTML file corresponds to an exercise that the user can select.
- **Video Feed Routes:**
  - **`/curls_feed`, `/squats_feed`, `/pullups_feed`, etc.:** These routes provide the live video feed for each exercise, calling the appropriate video frame processing function (e.g., `curls_frame()`, `squats_frame()`, etc.) and using Flask’s `Response` to stream the video to the browser in real-time.

---

### 5. **How the Application Executes**

#### Step-by-Step Execution:
1. **Starting the Application:**
   - When the user runs the application (`python app.py`), Flask starts a local server and listens on `localhost` (default is `port 5000`).
   
2. **Loading the Homepage:**
   - When the user opens `http://localhost:5000`, the homepage (`index.html`) is rendered, allowing the user to navigate to different exercise pages like curls, squats, etc.

3. **Selecting an Exercise:**
   - The user selects an exercise, for example, curls. This action triggers the `/curls` route, rendering `curls.html`.

4. **Video Feed:**
   - Once the user is on the curls page, the embedded video element on the page requests the `/curls_feed` route, which calls the `curls_frame()` function.
   - The video feed is processed in real-time, detecting the user's pose using Mediapipe and providing feedback (repetition count, angle, etc.) on the displayed video.

5. **Repetition Counting:**
   - As the user performs the exercise (e.g., bicep curls), the elbow angle is calculated for each frame, and the repetition count is updated based on the change in arm angle.
   - The updated count is rendered on the video feed.

6. **Real-Time Updates:**
   - The video feed is streamed back to the user's browser, providing real-time feedback on form and progress.

---

### 6. **Flask Application Run**
Finally, the application runs with `app.run()`, which starts the server and handles requests.

---

This structure ensures that users can easily interact with the application, perform various exercises, and receive live feedback on their form and progress. Each exercise page is tied to its specific video feed and processing logic, enabling real-time tracking and feedback for multiple types of workouts.
## Usage

FitMate can be used in various settings such as homes, offices, or any personal space. Simply set up your camera, perform your exercises, and receive real-time feedback on your form and repetitions.

---

## Roadmap

- [ ] Add a changelog
- [ ] Add more exercise types for pose detection
- [ ] Enhance feedback accuracy for complex movements
- [ ] Add integration with fitness tracking APIs
- [ ] Improve user interface with Bootstrap templates

See the [open issues](https://github.com/syedalihn/FitMate/issues) for a full list of proposed features and known issues.

---

## Contributing

Contributions make the open-source community a fantastic place to learn and grow. Any contributions you make to **FitMate** are greatly appreciated.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## Contact

- Project Link: [https://github.com/syedalihn/FitMate](https://github.com/syedalihn/FitMate)

---

## Acknowledgments

Special thanks to the following tools and libraries:

- [Mediapipe](https://mediapipe.dev/)
- [Flask](https://flask.palletsprojects.com)
- [OpenCV](https://opencv.org/)
- [Bootstrap](https://getbootstrap.com/)
- [GitHub Pages](https://pages.github.com/)

---

Feel free to use and modify this template according to your project's needs!
