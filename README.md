# EditREADME
Driver Drowsiness Detector

Libraries Used:

•	scipy.spatial.distance: Used for calculating Euclidean distance between points.
•	imutils: Provides convenience functions for OpenCV, such as resizing and displaying images.
•	pygame.mixer: Used for playing an alert sound when drowsiness is detected.
•	dlib: Contains facial detection and shape prediction tools.
•	cv2 (OpenCV): Used for computer vision tasks.

Function Definitions:

1.	eye_aspect_ratio(eye): Calculates the eye aspect ratio given the coordinates of the eye landmarks. The eye aspect ratio is a measure of eye openness and is used for drowsiness detection.
Constants and Parameters:
•	thresh: Threshold for detecting drowsiness based on the eye aspect ratio.
•	frame_check: Number of consecutive frames with a low eye aspect ratio required to trigger an alert.

Facial Landmark Detection Setup:
•	detect: Uses dlib to detect frontal faces in a grayscale frame.
•	predict: Utilizes a pre-trained shape predictor model for facial landmark prediction.

Main Loop:

•	Captures frames from the webcam using OpenCV.
•	Resizes the frame for faster processing.
•	Converts the frame to grayscale for facial landmark detection.
•	Detects faces and predicts facial landmarks for each face in the frame.
•	Calculates the eye aspect ratio for both eyes and determines if the person is drowsy.
•	If the eye aspect ratio falls below the threshold for a certain number of consecutive frames (frame_check), an alert is triggered, and an alarm sound is played using pygame.mixer.music.play().

Display:

•	Draws contours around the eyes on the frame.
•	Displays an alert message on the frame when drowsiness is detected.

User Interaction:

•	Pressing "q" quits the application.

Note:
•	Make sure to have the necessary files (shape_predictor_68_face_landmarks.dat and music.wav) in the same directory as the script for it to work correctly.


