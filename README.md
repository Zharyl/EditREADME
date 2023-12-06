# EditREADME
Driver Drowsiness Detector
Staying awake while driving is crucial for safety on the road. Being alert ensures quick reflexes, sharp focus, and better decision-making, reducing the risk of accidents. Drowsiness impairs reaction times and awareness, making it dangerous to operate a vehicle. There are countless of people who drive day at night. The public vehicle drivers like bus drivers, taxi drivers, and long-distance travelers suffers from lack of sleep, making it dangerous to driven when drowsy or sleepy. This python project “Driver Drowsiness Detector” aims to alert the drivers who are feeling drowsy or sleepy. To start the code, the libraries were installed in CMD prompt to properly utilized the code in the jupyter notebook module. OpenCV were installed using pip install opencv-python for face and eye detection, imutils were installed using pip install imutils to get the landmarks of the eye, pip install scipy to calculate the distance between eye landmarks, pip install pygame to play the alarm sound, pip install dlib and installing its wheel file from the internet for the face detection , and importing cv2 for computer vision tasks.  
To detect the frontal face, it has a built in function in detect = dlib.get_frontal_face_detector() and predicting it with predict = dlib.shape_predictor("shape_predictor_68_face_landmarks.dat") and insert the dataset for the facial landmarks that will detect the eye aspect ratio based on the coordinates of the landmarks that is detected around the eyes. Euclidean distance equation were used. A, B and C are the distances of the points around the eyes. The eye aspect ratio equation is (A+B)/2*C.  

def eye_aspect_ratio(eye):
	A = distance.euclidean(eye[1], eye[5])
	B = distance.euclidean(eye[2], eye[4])
	C = distance.euclidean(eye[0], eye[3])
	ear = (A + B) / (2.0 * C)
	return ear
 
thresh is set to 0.25 to determine the drowsiness based on the eye aspect ratio, frame_check is set to 20 it shows the frames showing drowsiness before triggering an alert. Detect and predict are utilized using dlib for face detection and facial landmark prediction. LStart, lEnd, rStart, and rEnd are indices for the left and right eye landmarks that are obtained by using face_utils.FACIAL_LANDMARKS_68_IDXS. Cv2.VideoCapture (0) initializes video capture to access the default camera index 0

thresh = 0.25
frame_check = 20
detect = dlib.get_frontal_face_detector()
predict = dlib.shape_predictor("shape_predictor_68_face_landmarks.dat")

(lStart, lEnd) = face_utils.FACIAL_LANDMARKS_68_IDXS["left_eye"]
(rStart, rEnd) = face_utils.FACIAL_LANDMARKS_68_IDXS["right_eye"]
cap=cv2.VideoCapture(0)
flag=0

while True:
	ret, frame=cap.read()
	frame = imutils.resize(frame, width=450)
	gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
	subjects = detect(gray, 0)
	for subject in subjects:
		shape = predict(gray, subject)
		shape = face_utils.shape_to_np(shape)
		leftEye = shape[lStart:lEnd]
		rightEye = shape[rStart:rEnd]
		leftEAR = eye_aspect_ratio(leftEye)
		rightEAR = eye_aspect_ratio(rightEye)
		ear = (leftEAR + rightEAR) / 2.0
		leftEyeHull = cv2.convexHull(leftEye)
		rightEyeHull = cv2.convexHull(rightEye)
		cv2.drawContours(frame, [leftEyeHull], -1, (0, 255, 0), 1)
		cv2.drawContours(frame, [rightEyeHull], -1, (0, 255, 0), 1)
		if ear < thresh:
			flag += 1
			print (flag)
			if flag >= frame_check:
				cv2.putText(frame, "****************ALERT!****************", (10, 30),
					cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 0, 255), 2)
				cv2.putText(frame, "****************ALERT!****************", (10,325),
					cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 0, 255), 2)
				mixer.music.play()
		else:
			flag = 0
	cv2.imshow("Frame", frame)
	key = cv2.waitKey(1) & 0xFF
	if key == ord("q"):
		break
cv2.destroyAllWindows()
cap.release()


This code segment captures frames from a video source—likely a webcam—constantly while running in an infinite loop. In each iteration, a frame is read, resized for processing, converted to grayscale, and then faces within the picture are detected using a facial detection model. It predicts facial landmarks for every face that is recognized, concentrating on the landmarks of the left and right eyes. It calculates the eye aspect ratio based on these landmarks to see if the eyes are sleepy. It sounds an alert and shows a warning message on the frame if the estimated eye aspect ratio drops below a predetermined threshold for a predetermined number of frames. The processed frame is shown in a window for in-the-moment monitoring, along with drawn eye outlines and possible warnings. The loop keeps running until the user hits the 'q' key, at which point all windows are closed and the video capture resources are released. This code seems to be a component of a system that uses eye movement analysis in real-time video feeds to identify and warn users of possible drowsiness.
The alerting sound will only be shown when the eye is partially closed and totally closed. Alert system will not be enabled if the eyes are blinking. 

