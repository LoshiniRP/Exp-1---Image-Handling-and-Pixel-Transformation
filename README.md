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

#### 1. Read the image ('Dog.jpg') using OpenCV imread() as a grayscale image.
```python
image_path = r"C:\Users\acer\DIPT\Dog.jpg"
dog_gray = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)
```

#### 2. Print the image width, height & Channel.
```python
if dog_gray is not None:
    height, width = dog_gray.shape[:2]
    channels = 1 if len(dog_gray.shape) == 2 else dog_gray.shape[2]
    print(f"Dog Grayscale -> Width: {width}, Height: {height}, Channels: {channels}")
```

#### 3. Display the image using matplotlib imshow().
```python
plt.imshow(dog_gray, cmap='gray')
plt.title("Dog Image - Grayscale")
plt.axis('off')
plt.show()
```

#### 4. Save the image as a PNG file using OpenCV imwrite().
```python
save_path_png = r"C:\Users\acer\DIPT\Dog_Grayscale.png"
cv2.imwrite(save_path_png, dog_gray)
print(f"Saved grayscale image as: {save_path_png}")
```

#### 5. Read the saved image above as a color image using cv2.cvtColor().
```python
dog_saved = cv2.imread(save_path_png)
dog_color = cv2.cvtColor(dog_saved, cv2.COLOR_BGR2RGB)
```

#### 6. Display the Colour image using matplotlib imshow() & Print the image width, height & channel.
```python
height, width, channels = dog_color.shape
print(f"Dog Color -> Width: {width}, Height: {height}, Channels: {channels}")

plt.imshow(dog_color)
plt.title("Dog Image - Color Format")
plt.axis('off')
plt.show()
```

#### 7. Crop the image to extract any specific (Eagle alone) object from the image.
```python
dog_bgr = cv2.imread(image_path)
dog_rgb = cv2.cvtColor(dog_bgr, cv2.COLOR_BGR2RGB)
h, w, _ = dog_rgb.shape
cropped_dog = dog_rgb[int(h*0.1):int(h*0.8), int(w*0.2):int(w*0.8)]
```

#### 8. Resize the image up by a factor of 2x.
```python
resized_dog = cv2.resize(cropped_dog, None, fx=2.0, fy=2.0, interpolation=cv2.INTER_LINEAR)
```

#### 9. Flip the cropped/resized image horizontally.
```python
flipped_dog = cv2.flip(resized_dog, 1)

fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(cropped_dog)
axes[0].set_title("7. Cropped Dog Region")
axes[0].axis('off')

axes[1].imshow(resized_dog)
axes[1].set_title("8. Resized 2x")
axes[1].axis('off')

axes[2].imshow(flipped_dog)
axes[2].set_title("9. Horizontally Flipped")
axes[2].axis('off')

plt.tight_layout()
plt.show()
```

#### 10. Read in the image ('Dog.jpg').
```python
annotated_dog = dog_rgb.copy()
h, w, _ = annotated_dog.shape
```

#### 11. Add the following text to the dark area at the bottom of the image (centered on the image):
```python
text = 'Dog Image Processing Test'
font_face = cv2.FONT_HERSHEY_PLAIN
font_scale = 1.5
font_thickness = 2

(text_w, text_h), baseline = cv2.getTextSize(text, font_face, font_scale, font_thickness)
text_x = (w - text_w) // 2
text_y = h - 20

cv2.putText(annotated_dog, text, (text_x, text_y), font_face, font_scale, (255, 255, 255), font_thickness, cv2.LINE_AA)
```

#### 12. Draw a magenta rectangle that encompasses the launch tower and the rocket.
```python
pt1 = (int(w * 0.25), int(h * 0.15))
pt2 = (int(w * 0.75), int(h * 0.85))
cv2.rectangle(annotated_dog, pt1, pt2, (255, 0, 255), 3)
```

#### 13. Display the final annotated image.
```python
plt.figure(figsize=(6, 6))
plt.imshow(annotated_dog)
plt.title("13. Annotated Dog Image")
plt.axis('off')
plt.show()
```

#### 14. Read the image ('Dog.jpg').
```python
img_float = dog_rgb.astype(np.float64)
```

#### 15. Adjust the brightness of the image.
```python
brightness_scalar = 50.0
matrix_ones = np.ones(dog_rgb.shape, dtype=np.float64) * brightness_scalar
```

#### 16. Create brighter and darker images.
```python
img_brighter = np.clip(cv2.add(img_float, matrix_ones), 0, 255).astype(np.uint8)
img_darker = np.clip(cv2.subtract(img_float, matrix_ones), 0, 255).astype(np.uint8)
```

#### 17. Display the images (Original Image, Darker Image, Brighter Image).
```python
fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(dog_rgb)
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
matrix1 = np.ones(dog_rgb.shape, dtype=np.float64) * 1.1
matrix2 = np.ones(dog_rgb.shape, dtype=np.float64) * 1.2

img_higher1 = (img_float * matrix1).astype(np.uint8)
img_higher2 = (img_float * matrix2).astype(np.uint8)
```

#### 19. Display the images (Original, Lower Contrast, Higher Contrast).
```python
fig, axes = plt.subplots(1, 3, figsize=(12, 4))
axes[0].imshow(dog_rgb)
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
r, g, b = cv2.split(dog_rgb)

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

plt.tight_layout()
plt.show()
```

#### 21. Merged the R, G, B , displays along with the original image
```python
dog_merged_rgb = cv2.merge([r, g, b])

fig, axes = plt.subplots(1, 2, figsize=(8, 4))
axes[0].imshow(dog_rgb)
axes[0].set_title("Original Image")
axes[0].axis('off')

axes[1].imshow(dog_merged_rgb)
axes[1].set_title("Merged R, G, B Image")
axes[1].axis('off')

plt.tight_layout()
plt.show()
```

#### 22. Split the image into the H, S, V components & Display the channels.
```python
dog_hsv = cv2.cvtColor(dog_bgr, cv2.COLOR_BGR2HSV)
h, s, v = cv2.split(dog_hsv)

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

plt.tight_layout()
plt.show()
```

#### 23. Merged the H, S, V, displays along with original image.
```python
dog_merged_hsv = cv2.merge([h, s, v])
dog_hsv_to_rgb = cv2.cvtColor(dog_merged_hsv, cv2.COLOR_HSV2RGB)

fig, axes = plt.subplots(1, 2, figsize=(8, 4))
axes[0].imshow(dog_rgb)
axes[0].set_title("Original Image")
axes[0].axis('off')

axes[1].imshow(dog_hsv_to_rgb)
axes[1].set_title("Merged H, S, V Image")
axes[1].axis('off')

plt.tight_layout()
plt.show()
```

## Output:
<img width="429" height="807" alt="image" src="https://github.com/user-attachments/assets/b67f0e9b-ec4b-4674-8f68-a6ca068288d4" />
<img width="946" height="357" alt="image" src="https://github.com/user-attachments/assets/f62d99a9-16d8-4e93-9404-6fbf8a5df4ad" />
<img width="370" height="463" alt="image" src="https://github.com/user-attachments/assets/5ad23107-18fc-44ff-880d-d10be5dff023" />



## Result:
Thus, the images were read, displayed, brightness and contrast adjustments were made, and bitwise operations were performed successfully using the Python program.

