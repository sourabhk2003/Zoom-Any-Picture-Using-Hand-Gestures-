# Zoom Any Picture Using Hand Gestures (Python OpenCV Project)

This project showcases a Python application that allows you to zoom in and out of any picture using hand gestures. It utilizes the OpenCV library along with the cvzone module to detect and track hand gestures, enabling you to control the zoom level of an image effortlessly.

Let's start...............

To make this project you need to follow this step:

---

## 📦 Installation

Install the required packages using pip:

```bash
pip install cvzone==1.4.1
pip install mediapipe==0.8.3.1
```

---

## 🚀 Deployment

To deploy this project run:

```python
import cv2
from cvzone.HandTrackingModule import HandDetector

# Initialize webcam
cap = cv2.VideoCapture(0)
cap.set(3, 1280)
cap.set(4, 720)

# Initialize hand detector
detector = HandDetector(detectionCon=0.7)

# Load image once
img1 = cv2.imread("one.jpg")
if img1 is None:
    print("Error: Image 'one.jpg' not found.")
    exit()

startDis = None
scale = 0
cx, cy = 200, 200

while True:
    success, img = cap.read()
    if not success:
        break

    hands, img = detector.findHands(img)

    if len(hands) == 2:
        hand1, hand2 = hands[0], hands[1]
        f1, f2 = detector.fingersUp(hand1), detector.fingersUp(hand2)

        if f1 == [1, 1, 1, 1, 1] and f2 == [1, 1, 1, 1, 1]:
            if startDis is None:
                length, info, img = detector.findDistance(hand1["center"], hand2["center"], img)
                startDis = length

            length, info, img = detector.findDistance(hand1["center"], hand2["center"], img)
            scale = int((length - startDis) // 2)
            cx, cy = info[4:]

    else:
        startDis = None

    try:
        h1, w1, _ = img1.shape
        newH, newW = ((h1 + scale) // 2) * 2, ((w1 + scale) // 2) * 2
        resized_img = cv2.resize(img1, (newW, newH))

        top = max(cy - newH // 2, 0)
        bottom = min(cy + newH // 2, img.shape[0])
        left = max(cx - newW // 2, 0)
        right = min(cx + newW // 2, img.shape[1])

        resized_crop = resized_img[0:bottom - top, 0:right - left]
        img[top:bottom, left:right] = resized_crop

    except:
        pass

    cv2.imshow("Gesture Zoom Viewer", img)
    key = cv2.waitKey(1) & 0xFF
    if key == ord('q'):
        break
    if key == ord('r'):
        startDis = None
        scale = 0

cap.release()
cv2.destroyAllWindows()
```

---

## 🙋‍♂️ You Can Follow Me

- **LinkedIn:** [https://www.linkedin.com/in/sourabh-kushwah/](https://www.linkedin.com/in/sourabh-kushwah/)
- **YouTube:** [https://www.youtube.com/@ClassicIndianreels](https://www.youtube.com/@ClassicIndianreels)
- **Gmail:** jaimahankal0786@gmail.com
- **Instagram:** [https://www.instagram.com/abamahakal/](https://www.instagram.com/abamahakal/)

If you have any confusion, please feel free to contact me. Thank you 🙏

---

## 📝 License

This script is released under the **MIT License**.  
Feel free to use, modify, and distribute it as you wish.  
If you find any bugs or have suggestions for improvement, please submit an issue or a pull request.
