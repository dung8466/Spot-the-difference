
## About The Project

[Spot The Difference](https://github.com/dung8466/Spot-the-difference)

This is the midterm project of Image Processing (INT3404_1) at UET.

Project  requirements:
* From one image create multiple images (3+) with a degree of complexity
* Find the difference between the original image and the newly created images

### Project Solution
#### Create multiple images
##### Delete green color from image
```python
low_green = np.array([25, 52, 72])
high_green = np.array([102, 255, 255])
# convert BGR to HSV
imgHSV = cv2.cvtColor(img0, cv2.COLOR_BGR2HSV)
# create the Mask
mask = cv2.inRange(imgHSV, low_green, high_green)
# inverse mask
mask = 255-mask
res = cv2.bitwise_and(img0, img0, mask=mask)
```
##### Blur image
```python
img2 = cv2.medianBlur(img1, 5)
erosion = cv2.erode(img2, kernel, iterations=1)
output = cv2.dilate(erosion, kernel, iterations=1)
```
##### Convert color(from black to blue)
```python
img3 = img1
img_hsv=cv2.cvtColor(img3, cv2.COLOR_BGR2HSV)
# lower mask (0-10)
lower_red = np.array([50,50,0])
upper_red = np.array([255,255,50])
mask0 = cv2.inRange(img_hsv, lower_red, upper_red)
# upper mask (170-180)
lower_red = np.array([50,50,50])
upper_red = np.array([255,255,180])
mask1 = cv2.inRange(img_hsv, lower_red, upper_red)
# join my masks
mask = mask0+mask1
output_img = img3.copy()
output_img[np.where(mask!=0)] = [125, 50, 50]
```
#### Find the difference
The solution of all images is the same: 
* Convert original image and new one to grayscale
* Calculate absolute difference between two arrays
* Apply threshold. Apply both THRESH_BINARY and THRESH_OTSU
* Dilation
* Calculate contours
* Calculate bounding box around contour
* Draw rectangle - bounding box on image
* Show images with rectangles on differences
```python
gray1 = cv2.cvtColor(img1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
diff = cv2.absdiff(gray1, gray2)
cv2.imshow("diff(img1, img2)", diff)
thresh = cv2.threshold(diff, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)[1]
cv2.imshow("Threshold", thresh)
kernel = np.ones((5,5), np.uint8) 
dilate = cv2.dilate(thresh, kernel, iterations=2) 
cv2.imshow("Dilate", dilate)
contours = cv2.findContours(dilate.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
contours = imutils.grab_contours(contours)
for contour in contours:
    if cv2.contourArea(contour) > 100:
        x, y, w, h = cv2.boundingRect(contour)
        cv2.rectangle(img2, (x, y), (x+w, y+h), (0,0,255), 2)
x = np.zeros((img_height,10,3), np.uint8)
```
## Result
* Create new images
![Lv0](/screenshot/Lv0.png)
![Lv1](/screenshot/Lv1.png)
![Lv2](/screenshot/Lv2.png)
* Find the difference
![Lv0 Result](/screenshot/Lv0Result.png)
![Lv1 Result](/screenshot/Lv1Result.png)
![Lv2 Result](/screenshot/Lv2Result.png)

### Prerequisites

Things you need:
* Make sure [python](https://www.python.org/) and [pip](https://pip.pypa.io/en/stable/installation/) are installed
* Jupyter notebook
```sh
pip install notebook
```
* opencv
```sh
pip install opencv-python
```
* numpy
```sh
pip install numpy
```
* imutils
```sh
pip install imutils
```
### Try this project

Clone the repo
   ```sh
   git clone https://github.com/dung8466/Spot-the-difference.git
   ```
Run jupyter notebook
   ```sh
   jupyter notebook
   ```
Select this project folder and run img_diff.ipynb.



<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<!-- CONTACT -->
## Contact

Nguyễn Tiến Dũng - email: ntd8466@gmail.com

Project Link: [Spot-the-difference](https://github.com/dung8466/Spot-the-difference)
