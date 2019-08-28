# FaceDetection
First, we need to load the required XML classifiers. 
import cv2
faceCascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
This loads the face cascade into memory so it's ready for use. Remember, the cascade is just an XML file that contains the data to detect faces.
video_capture = cv2.VideoCapture(0)
This line sets the video source to the default webcam, which OpenCV can easily capture.
while True:
    #Capture frame-by-frame
    ret, frame = video_capture.read()


In here, We're capturing the video. The read() method reads one frame from the video source which is the Webcam and it returns:
The actual video frame read (one frame on each loop)
A return code 

If we run out of frames by any chance, The return code will tell us but in our scenario, We are not reading from a file it's our own Webcam so there's no possibility for us to run of out of frames.
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
We just converted our Webcam feed to Grayscale. ( Most of the operations in OpenCV are done in grayscale.)
Now we're kicking the jackpot bucket!
faces = faceCascade.detectMultiScale(
        gray,
        scaleFactor=1.5,
        minNeighbors=5,
        minSize=(30, 30),
        flags=cv2.CASCADE_SCALE_IMAGE
    )
Here's when our code detects faces in our frame. Let's drill down from each of the above.
The detectMultiScale function is a general function that detects objects. It detects faces since we're using it on the face cascade.
The first option is the grayscale image.
The second is the scaleFactor. Since some faces may be closer to the camera, they would appear bigger than the faces in the back. The scale factor compensates for this. (You can change it to 1.5)
The detection algorithm uses a moving window to detect objects. minNeighbors defines how many objects are detected near the current one before it declares the face found. minSize, meanwhile, gives the size of each window.

The function returns a list of rectangles in which it believes it found a face. Next, we will loop over where it thinks it found something.
for (x, y, w, h) in faces:
        cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
This function returns 4 values: the x and y location of the rectangle, and the rectangle's width and height (w , h).
We use these values to draw a rectangle using the built-in rectangle() function.

cv2.imshow('FaceDetection', frame)
This is the frame name (Kind of similar to the title of the Webcam popup)
> Now, I want to take a photo of my frame by Pressing the Space bar and I need to exit my program by clicking 'ESC'.
#ESC Pressed
if k%256 == 27: 
        break
#SPACE pressed
elif k%256 == 32:       
        img_name = "facedetect_webcam_{}.png".format(img_counter)
        cv2.imwrite(img_name, frame)
        print("{} written!".format(img_name))
        img_counter += 1
img_counter is a variable to count and increment after each photo was taken. We should declare this before declaring our While loop on the beginning (While=True) 
To Clean everything up;
# When everything is done, release the capture
video_capture.release()
cv2.destroyAllWindows()
