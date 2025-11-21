Real-Time Drowsiness Detector (Google Colab)
This project is a real-time drowsiness detection system designed to run entirely within a Google Colab notebook. It uses your webcam, OpenCV, and the Dlib library to monitor your eye movements. If your eyes remain closed for a set duration, it triggers an audible alarm to alert you.
 Key Features
  * **Real-Time Detection:** Uses your webcam to monitor your face in real-time.
  * **Facial Landmark Detection:** Employs Dlib's 68-point facial landmark predictor to locate the eyes.
  * **Eye Aspect Ratio (EAR):** Calculates the EAR to determine if the eyes are open or closed.
  * **Audible Alert:** Plays an alarm sound if drowsiness (eyes closed) is detected for a consecutive number of frames.
  * **Colab-Ready:** Includes all necessary helper functions to access the webcam directly from a Google Colab notebook.
How It Works
The core of this system is the **Eye Aspect Ratio (EAR)**, a concept introduced by Tereza Soukupová and Jan Čech in their 2016 paper.
1.  **Face Detection:** The system first detects a face in the webcam feed using Dlib's frontal face detector.
2.  **Landmark Prediction:** It then locates the 68 key facial landmarks (mouth, nose, eyes, etc.) on the detected face.
3.  **Eye Isolation:** It isolates the specific landmarks corresponding to the left and right eyes.
4.  **EAR Calculation:** The Eye Aspect Ratio is calculated for each eye using the formula:
    $$EAR = \frac{||p_2 - p_6|| + ||p_3 - p_5||}{2 \cdot ||p_1 - p_4||}$$
    Where $p_1$ through $p_6$ are the landmark points of the eye.
      * When an eye is **open**, the EAR value is relatively high and stable.
      * When an eye **blinks** or **closes**, the EAR value drops rapidly towards zero.
5.  **Drowsiness Check:** The system checks if the average EAR of both eyes falls below a specific **threshold** (`thresh`).
      * If the EAR is below the threshold, a counter (`flag`) is incremented.
      * If the EAR is above the threshold, the counter is reset.
      * If the counter exceeds a certain number of consecutive frames (`frame_check`), the system concludes that the user is drowsy and plays the **alarm**.
How to Use This Notebook
This project is designed to be run cell by cell in Google Colab.
1.  **Cell 1: Install Dependencies**
      * Run the first code cell to install the required Python libraries: `dlib` and `imutils`.
2.  **Cell 2: Download Assets**
      * Run the second cell. This downloads two essential files:
          * `shape_predictor_68_face_landmarks.dat`: The pre-trained Dlib model for detecting facial landmarks.
          * `alarm.wav`: The audio file that will be played as an alert.
3.  **Cell 3: Define Helper Functions**
      * Run the third cell. This defines all the necessary functions, including:
          * `video_stream()`, `video_frame()`, `video_release()`: JavaScript-based functions to access your browser's webcam from within Colab.
          * `eye_aspect_ratio()`: The function that implements the EAR formula.
4.  **Cell 4: Run the Main Program**
      * Run the final, main cell.
      * **Grant Permission:** Your browser will pop up a request asking for permission to use your camera. You **must click "Allow"** for the program to work.
      * **Initialization:** You will see "Video stream starting..." and "Initialization complete." A video feed from your webcam will appear in the output.
      * **Detection:** The program is now actively monitoring your face. Green contours will be drawn around your eyes, and the current EAR value will be displayed.
      * **Test It:** Try closing your eyes for a few seconds. You should see the "ALERT\!" message on the screen and hear the alarm.
      * **Stop:** To stop the program, **click on the video feed** in the Colab output. This will stop the webcam stream and clean up the output.
Configuration
You can easily adjust the sensitivity of the detector by changing the two constants at the beginning of the main code cell (Cell 4).
```python
# ADJUST THESE VALUES FOR YOUR FACE AND LIGHTING
thresh = 0.25
frame_check = 5
```
  * `thresh` (Threshold): This is the EAR value below which the eye is considered "closed."
      * **If the alarm triggers too easily** (e.g., during normal blinks), try **lowering** this value (e.g., to `0.22`).
      * **If it fails to detect closed eyes**, try **raising** this value (e.g., to `0.27`).
  * `frame_check` (Frame Check): This is the number of *consecutive frames* your eyes must be considered "closed" before the alarm sounds.
      * **Increase this value** (e.g., to `15`) to make the detector less sensitive to long blinks and more robust for genuine drowsiness.
      * **Decrease this value** (e.g., to `3`) for a faster, more sensitive response.
Dependencies
* Python 3
* dlib
* imutils
* OpenCV (`cv2`)
* SciPy (`scipy.spatial.distance`)
  * NumPy
  * Google Colab environment (for webcam access functions)
