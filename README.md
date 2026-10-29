# EXP - 1 IMAGE HANDLING AND PIXEL TRANSFORMATION USING OPEN CV

## Aim:
Write a Python program using OpenCV that performs the following tasks:
1) Read and Display an Image.  
2) Adjust the brightness of an image.  
3) Modify the image contrast.  
4) Generate a third image using bitwise operations.

## Software Required:
- Anaconda - Python 3.7
- Jupyter Notebook (for interactive development and execution)

## Algorithm:
### Step 1:
Load an image from your local directory and display it.

### Step 2:
Create a matrix of ones (with data type float64) to adjust brightness.

### Step 3:
Create brighter and darker images by adding and subtracting the matrix from the original image.  
Display the original, brighter, and darker images.

### Step 4:
Modify the image contrast by creating two higher contrast images using scaling factors of 1.1 and 1.2 (without overflow fix).  
Display the original, lower contrast, and higher contrast images.

### Step 5:
Split the image (boy.jpg) into B, G, R components and display the channels

## Program Developed By:
- **Name:** R P Loshini 
- **Register Number:** 212225230155

## Program:

```
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

#### 1. Read the image ('Eagle_in_Flight.jpg') using OpenCV imread() as a grayscale image.
```python
path_eagle = r"C:\Users\acer\DIPT\Eagle_in_Flight.jpg"
eagle_gray = cv2.imread(path_eagle, cv2.IMREAD_GRAYSCALE)
```

#### 2. Print the image width, height & Channel.
```python
h, w = eagle_gray.shape[:2]
c = 1 if len(eagle_gray.shape) == 2 else eagle_gray.shape[2]
print(f"Grayscale Eagle -> Width: {w}, Height: {h}, Channels: {c}")
```

#### 3. Display the image using matplotlib imshow().
```python
plt.imshow(eagle_gray, cmap='gray')
plt.title("Grayscale Eagle")
plt.axis('off')
plt.show()
```

#### 4. Save the image as a PNG file using OpenCV imwrite().
```python
path_png = r"C:\Users\acer\DIPT\Eagle_in_Flight.png"
cv2.imwrite(path_png, eagle_gray)
```

#### 5. Read the saved image above as a color image using cv2.cvtColor().
```python
eagle_saved = cv2.imread(path_png)
eagle_color = cv2.cvtColor(eagle_saved, cv2.COLOR_BGR2RGB)
```

#### 6. Display the Colour image using matplotlib imshow() & Print the image width, height & channel.
```python
h, w, c = eagle_color.shape
print(f"Color Eagle -> Width: {w}, Height: {h}, Channels: {c}")
plt.imshow(eagle_color)
plt.title("Color Format Eagle")
plt.axis('off')
plt.show()
```

#### 7. Crop the image to extract any specific (Eagle alone) object from the image.
```python
eagle_bgr = cv2.imread(path_eagle)
eagle_rgb = cv2.cvtColor(eagle_bgr, cv2.COLOR_BGR2RGB)
h, w, _ = eagle_rgb.shape
cropped_eagle = eagle_rgb[int(h*0.1):int(h*0.8), int(w*0.2):int(w*0.8)]
```

#### 8. Resize the image up by a factor of 2x.
```python
resized_eagle = cv2.resize(cropped_eagle, None, fx=2.0, fy=2.0, interpolation=cv2.INTER_LINEAR)
```

#### 9. Flip the cropped/resized image horizontally.
```python
flipped_eagle = cv2.flip(resized_eagle, 1)

fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(cropped_eagle)
axes[0].set_title("Cropped")
axes[0].axis('off')

axes[1].imshow(resized_eagle)
axes[1].set_title("Resized 2x")
axes[1].axis('off')

axes[2].imshow(flipped_eagle)
axes[2].set_title("Flipped")
axes[2].axis('off')
plt.show()
```

#### 10. Read in the image ('Dog.jpg').
```python
path_apollo = r"C:\Users\acer\DIPT\Apollo-11-launch.jpg"
apollo_bgr = cv2.imread(path_apollo)
apollo_rgb = cv2.cvtColor(apollo_bgr, cv2.COLOR_BGR2RGB)
annotated_apollo = apollo_rgb.copy()
h, w, _ = annotated_apollo.shape
```

#### 11. Add the following text to the dark area at the bottom of the image (centered on the image):
```python
text = 'Apollo 11 Saturn V Launch, July 16, 1969'
font_face = cv2.FONT_HERSHEY_PLAIN
(text_w, text_h), _ = cv2.getTextSize(text, font_face, 1.2, 1)
cv2.putText(annotated_apollo, text, ((w - text_w) // 2, h - 20), font_face, 1.2, (255, 255, 255), 1)
```

#### 12. Draw a magenta rectangle that encompasses the launch tower and the rocket.
```python
cv2.rectangle(annotated_apollo, (int(w * 0.35), int(h * 0.05)), (int(w * 0.65), int(h * 0.85)), (255, 0, 255), 2)
```

#### 13. Display the final annotated image.
```python
plt.imshow(annotated_apollo)
plt.title("Annotated Apollo 11 Image")
plt.axis('off')
plt.show()
```

#### 14. Read the image ('Boy.jpg').
```python
path_boy = r"C:\Users\acer\DIPT\boy.jpg"
boy_bgr = cv2.imread(path_boy)
boy_rgb = cv2.cvtColor(boy_bgr, cv2.COLOR_BGR2RGB)
img_float = boy_rgb.astype(np.float64)
```

#### 15. Adjust the brightness of the image.
```python
matrix_ones = np.ones(boy_rgb.shape, dtype=np.float64) * 50.0
```

#### 16. Create brighter and darker images.
```python
img_brighter = np.clip(cv2.add(img_float, matrix_ones), 0, 255).astype(np.uint8)
img_darker = np.clip(cv2.subtract(img_float, matrix_ones), 0, 255).astype(np.uint8)
```

#### 17. Display the images (Original Image, Darker Image, Brighter Image).
```python
fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(boy_rgb)
axes[0].set_title("Original Image")
axes[0].axis('off')

axes[1].imshow(img_darker)
axes[1].set_title("Darker Image")
axes[1].axis('off')

axes[2].imshow(img_brighter)
axes[2].set_title("Brighter Image")
axes[2].axis('off')

plt.tight_layout()
plt.show()
```

#### 18. Modify the image contrast.
```python
matrix1 = np.ones(boy_rgb.shape, dtype=np.float64) * 1.1
matrix2 = np.ones(boy_rgb.shape, dtype=np.float64) * 1.2

img_higher1 = (img_float * matrix1).astype(np.uint8)
img_higher2 = (img_float * matrix2).astype(np.uint8)
```

#### 19. Display the images (Original, Lower Contrast, Higher Contrast).
```python
fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(boy_rgb)
axes[0].set_title("Original Image")
axes[0].axis('off')

axes[1].imshow(img_higher1)
axes[1].set_title("Contrast Scale 1.1")
axes[1].axis('off')

axes[2].imshow(img_higher2)
axes[2].set_title("Contrast Scale 1.2")
axes[2].axis('off')

plt.tight_layout()
plt.show()
```

#### 20. Split the image (boy.jpg) into the B,G,R components & Display the channels.
```python
r, g, b = cv2.split(boy_rgb)

fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(r, cmap='Reds')
axes[0].set_title("Red Channel")
axes[0].axis('off')

axes[1].imshow(g, cmap='Greens')
axes[1].set_title("Green Channel")
axes[1].axis('off')

axes[2].imshow(b, cmap='Blues')
axes[2].set_title("Blue Channel")
axes[2].axis('off')
plt.show()
```

#### 21. Merged the R, G, B , displays along with the original image
```python
boy_merged_rgb = cv2.merge([r, g, b])

fig, axes = plt.subplots(1, 2, figsize=(8, 4))
axes[0].imshow(boy_rgb)
axes[0].set_title("Original Image")
axes[0].axis('off')

axes[1].imshow(boy_merged_rgb)
axes[1].set_title("Merged R, G, B Image")
axes[1].axis('off')
plt.show()
```

#### 22. Split the image into the H, S, V components & Display the channels.
```python
boy_hsv = cv2.cvtColor(boy_bgr, cv2.COLOR_BGR2HSV)
h, s, v = cv2.split(boy_hsv)

fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(h, cmap='hsv')
axes[0].set_title("Hue (H) Channel")
axes[0].axis('off')

axes[1].imshow(s, cmap='gray')
axes[1].set_title("Saturation (S) Channel")
axes[1].axis('off')

axes[2].imshow(v, cmap='gray')
axes[2].set_title("Value (V) Channel")
axes[2].axis('off')
plt.show()
```

#### 23. Merged the H, S, V, displays along with original image.
```python
boy_merged_hsv = cv2.merge([h, s, v])
boy_hsv_to_rgb = cv2.cvtColor(boy_merged_hsv, cv2.COLOR_HSV2RGB)

fig, axes = plt.subplots(1, 2, figsize=(8, 4))
axes[0].imshow(boy_rgb)
axes[0].set_title("Original Image")
axes[0].axis('off')

axes[1].imshow(boy_hsv_to_rgb)
axes[1].set_title("Merged H, S, V Image")
axes[1].axis('off')
plt.show()
```

## Output:
<img width="489" height="845" alt="image" src="https://github.com/user-attachments/assets/7c799ea3-086e-48e1-8428-7d00888757aa" />
<img width="941" height="279" alt="image" src="https://github.com/user-attachments/assets/f383965e-25af-4df2-8586-9d2f159c34ab" />
<img width="502" height="309" alt="image" src="https://github.com/user-attachments/assets/c39e4dbc-ffbb-467a-9a5a-c2d30e9d8fb8" />
<img width="1077" height="290" alt="image" src="https://github.com/user-attachments/assets/350da33f-2e2d-4991-98e0-e6e5224d0e26" />
<img width="1079" height="291" alt="image" src="https://github.com/user-attachments/assets/cb994a80-4ecc-401c-89c5-61bb2f72e2b9" />
<img width="942" height="486" alt="image" src="https://github.com/user-attachments/assets/5ae69754-a219-4600-8334-f40aea8d686b" />
<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/426bf066-6042-4c88-85b7-b17022d8dac7" />



## Result:
Thus, the images were read, displayed, brightness and contrast adjustments were made, and bitwise operations were performed successfully using the Python program.

