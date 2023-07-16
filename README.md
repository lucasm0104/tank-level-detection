# Tank-level-detection

![GitHub last commit](https://img.shields.io/github/last-commit/matheusslr/tank-level-detection)
![GitHub language count](https://img.shields.io/github/languages/count/matheusslr/tank-level-detection)
![Github repo size](https://img.shields.io/github/repo-size/matheusslr/tank-level-detection)
![Github stars](https://img.shields.io/github/stars/matheusslr/tank-level-detection)

![level of water](/img/level.gif)

> This is a computer vision project with the main purpose of detecting the level of water in a transparent tank using a camera.

## Prerequisites

Before you begin, make sure you have the following dependencies installed:

- Python 3.x
- OpenCV (cv2) 4.7.x
- Numpy 1.2x

## How to run the project

Follow the steps below to run the project on your local machine:

Execute the following commands from the project root folder:

### Clone this repository

```bash
git clone https://github.com/matheusslr/Tank-level-monitoring-with-computer-vision
```

### Install the dependencies

```bash
pip install numpy opencv-python
```

### Run the project

```bash
python .\waterTankLevel.py
```

## Code Explanation

### 1. Importing Libraries
```python
import numpy as np
import cv2
```
The code starts by importing the necessary libraries: `numpy` for numerical computations and `cv2` for computer vision operations.

### 2. Opening the Camera
```python
cap = cv2.VideoCapture(0)
if not cap.isOpened():
    print("Cannot open camera")
    exit()
```
The code creates a ``VideoCapture`` object to access the camera. It checks if the camera is opened successfully. If not, it prints an error message and exits the program.

### 3. Main Loop
```python
while True:
    ret, img = cap.read()
    if not ret:
        print("Can't receive frame (stream end?). Exiting ...")
        break
```
The code enters a loop to continuously read frames from the camera. It uses the ``read()`` method of the ``VideoCapture`` object to retrieve the current frame. If no frame is received (stream end), it breaks out of the loop.

### 4. Image Processing
```python
    img = img[0:500, 200:380]
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    edges = cv2.Canny(gray, 169, 25)
```
In this section, the code performs some image processing operations on the captured frame. It crops the image to a specific region of interest, converts it to grayscale using the ``cvtColor()`` function, and applies the Canny edge detection algorithm to detect edges in the image.

### 5. Line Detection
```python
    lines = cv2.HoughLines(edges, 1, np.pi/180, 50)
    levels = []
    try:
        for line in lines:
            for rho, theta in line:
                # ...
```
Using the edges obtained from the previous step, the code applies the Hough transform using the ``HoughLines()`` function. This function detects lines in the image. The detected lines are stored in the lines variable.

Inside the loop, the code iterates over each line and extracts the rho and theta values. It calculates the line's endpoints and draws it on the original image. Additionally, it calculates a level value based on the line's position and adds it to the ``levels`` list.

### 6. Level Calculation
```python
    level = (469 - y2) * 0.0217
    if level > 1 and level < 10:
        cv2.line(img, (x1, y1), (x2, y2), (0, 0, 255), 1)
        levels.append(level)
```
The level value is calculated based on the y-coordinate of the line's endpoint (``y2``). It is then checked to ensure it falls within a certain range. If the level is within the range, the line is drawn on the image, and the level value is added to the ``levels`` list.

### 7. Output and User Interaction
```python
    if len(levels) > 0:
        print(levels[0])
    levels = None
```
If there are level values in the ``levels`` list, the code prints the first value. After that, the ``levels`` list is reset to None.

```python
    cv2.line(gray, (0, 469), (400, 469), (0, 0, 255), 1)
    cv2.imshow("output", img)
    cv2.imshow("edges", edges)
```
The code draws a reference line on the grayscale image using the ``line()`` function. It also displays the original image (``img``) and the edges image (``edges``) using the ``imshow()`` function.

```python
    key = cv2.waitKey(10)
    if key == 27:
        cv2.imwrite('img/teste.jpg', img)
        break
```
The code waits for a key press using the ``waitKey()`` function. If the key pressed is the 'Esc' key (key code 27), it saves the current image as "img/teste.jpg" using the ``imwrite()`` function and breaks out of the loop.


### 8. Cleaning Up
```python
cv2.destroyAllWindows()
cv2.VideoCapture(0).release()
```
Finally, the code releases the camera capture and closes all open windows using the ``destroyAllWindows()`` function.

## How to Contribute

If you want to contribute to this project, follow the steps below:

1. Fork this repository.
2. Create a branch: `git checkout -b <branch_name>`.
3. Make your changes and confirm them: `git commit -m '<commit_message>'`
4. Send to the original branch: `git push origin <project_name> / <location>`
5. Create the pull request.

Alternatively, consult the GitHub documentation on [how to create a pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request).

## Difficulties and improvements

- Difficulties:
    - Adjusting the illumination incident on the tank to detect the level line;
    - Set parameters of the ``Canny`` and ``HoughLines`` functions;
    - Positioning the camera statically, as the tank had no platform/structure to do so

- What could be improved
    - Setting the parameters of the ``Houghlines`` and ``Canny`` functions dynamically (even a trackbar would be useful);
    - An edge detector to dynamically crop the areas of interest;

## License

This project is under license. See [LICENSE](LICENSE) for more information.

## Back to the top

[⬆ Back to the top](#tank-level-detection)