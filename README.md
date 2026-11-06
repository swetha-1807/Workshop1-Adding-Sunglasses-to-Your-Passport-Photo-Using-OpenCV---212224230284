# Workshop1-Adding-Sunglasses-to-Your-Passport-Photo-Using-OpenCV---212224230284

## NAME : SWETHA K
## REGISTER NUMBER : 212224230284

## Aim

To write a Python program using OpenCV and NumPy to add sunglasses to a passport-size photo using image processing techniques. The program resizes and overlays sunglasses onto the face at the appropriate position to create a fun photo transformation.

## The program performs the following operations:

Read and display the input passport photo.
Read the sunglasses image in PNG format.
Resize the sunglasses according to the required dimensions.
Extract the sunglasses color and alpha channels.
Create a mask for the sunglasses image.
Position the sunglasses over the eye region.
Overlay the sunglasses onto the original photo.
Display the final image for comparison.

## Software Used

Anaconda – Python 3.7

Jupyter Notebook / VS Code

OpenCV (cv2)

NumPy

Matplotlib

## Algorithm

Step 1:
Import the required libraries: OpenCV, NumPy, and Matplotlib.

Step 2:
Read the input passport photo using cv2.imread().

Step 3:
Read the sunglasses image in PNG format with the alpha channel.

Step 4:
Convert the original image from BGR to RGB format for display.

Step 5:
Resize the sunglasses image according to the required face dimensions using cv2.resize().

Step 6:
Separate the color channels and alpha channel from the sunglasses image.

Step 7:
Create a mask for the sunglasses using the alpha channel.

Step 8:
Calculate the correct position to place the sunglasses over the eyes.

Step 9:
Overlay the sunglasses onto the original image using masking techniques.

Step 10:
Display the original image and the final image with sunglasses for comparison.

## Program

## Developed By

## Name: SWETHA K
## Register No: 212224230284


```
import cv2
import numpy as np
import matplotlib.pyplot as plt

faceImage = cv2.imread(r'C:\Users\admin\Downloads\my photo.jpeg')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")

faceImage.shape

glassPNG = cv2.imread(r'C:\Users\admin\Downloads\Sunglasses.jpeg',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")

glassPNG = cv2.resize(glassPNG,(190,50))
print("image Dimension ={}".format(glassPNG.shape))

# Separate the color channels
glassBGR = glassPNG[:, :, 0:3]

# Convert BGR to grayscale
gray = cv2.cvtColor(glassPNG, cv2.COLOR_BGR2GRAY)

# Create alpha mask
ret, glassMask1 = cv2.threshold(
    gray, 200, 255, cv2.THRESH_BINARY_INV
)

plt.figure(figsize=[15, 5])

plt.subplot(121)
plt.imshow(glassBGR[:, :, ::-1])
plt.title('Sunglass Color channels')

plt.subplot(122)
plt.imshow(glassMask1, cmap='gray')
plt.title('Sunglass Alpha channel')

plt.show()

faceWithGlassesNaive = faceImage.copy()

# Resize sunglasses
glassBGR_resized = cv2.resize(glassBGR, (300, 80))

# Place sunglasses slightly below
faceWithGlassesNaive[275:355, 395:695] = glassBGR_resized

plt.figure(figsize=(6, 8))
plt.imshow(faceWithGlassesNaive[..., ::-1])
plt.axis('off')
plt.show()

print("Face image shape:", faceImage.shape)
print("Glasses image shape:", glassBGR.shape)
print("Mask shape:", glassMask1.shape)
# Find the actual sunglasses area using the mask
coords = cv2.findNonZero(glassMask1)

x, y, w, h = cv2.boundingRect(coords)

# Crop both glasses and mask
glassBGR = glassBGR[y:y+h, x:x+w]
glassMask1 = glassMask1[y:y+h, x:x+w]
# Create a fresh copy
faceWithGlassesArithmetic = faceImage.copy()

H, W = faceImage.shape[:2]

# Set sunglasses position proportionally
x1 = int(W * 0.425)
x2 = int(W * 0.645)

y1 = int(H * 0.24)
y2 = int(H * 0.32)

eyeROI = faceWithGlassesArithmetic[y1:y2, x1:x2]

# Resize glasses and mask
glassBGR = cv2.resize(glassBGR, (eyeROI.shape[1], eyeROI.shape[0]))
glassMask1 = cv2.resize(glassMask1, (eyeROI.shape[1], eyeROI.shape[0]))

# Convert mask into 3 channels
glassMask = cv2.cvtColor(glassMask1, cv2.COLOR_GRAY2BGR)

# Normalize mask
glassMask = glassMask.astype(float) / 255.0

# Arithmetic masking
maskedEye = cv2.multiply(eyeROI.astype(float), 1 - glassMask)
maskedGlass = cv2.multiply(glassBGR.astype(float), glassMask)

# Combine
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

# Replace eye region
faceWithGlassesArithmetic[y1:y2, x1:x2] = eyeRoiFinal.astype('uint8')

# Display
plt.figure(figsize=(8, 10))
plt.imshow(cv2.cvtColor(faceWithGlassesArithmetic, cv2.COLOR_BGR2RGB))
plt.axis('off')
plt.show()

# Create a fresh copy of the original face image
faceWithGlassesArithmetic = faceImage.copy()

# Select the eye region
eyeROI = faceWithGlassesArithmetic[310:410, 430:660]
# Resize sunglasses and mask to match the eye region
glassBGR = cv2.resize(glassBGR, (230, 100))
glassMask1 = cv2.resize(glassMask1, (230, 100))

# Normalize the mask
glassMask = glassMask1.astype(np.float32) / 255.0

# Convert mask into 3 channels
glassMask = cv2.cvtColor(glassMask, cv2.COLOR_GRAY2BGR)

# Apply mask to the face
maskedEye = cv2.multiply(eyeROI.astype(np.float32), 1 - glassMask)

# Apply mask to sunglasses
maskedGlass = cv2.multiply(glassBGR.astype(np.float32), glassMask)

# Combine both images
eyeRoiFinal = cv2.add(maskedEye, maskedGlass).astype(np.uint8)

# Place sunglasses on the face
faceWithGlassesArithmetic[310:410, 430:660] = eyeRoiFinal

# Display the final result
plt.figure(figsize=[20,20])

plt.subplot(121)
plt.imshow(faceImage[:,:,::-1])
plt.title("Original Image")

plt.subplot(122)
plt.imshow(faceWithGlassesArithmetic[:,:,::-1])
plt.title("With Sunglasses")

plt.show()

```

## Output

<img width="550" height="442" alt="image" src="https://github.com/user-attachments/assets/518ea0ce-02af-4ea2-9c1a-be0cf7be4acd" />

<img width="548" height="301" alt="image" src="https://github.com/user-attachments/assets/c8f42f5c-2f4f-4f72-8567-0348a67b9ce0" />

<img width="662" height="118" alt="image" src="https://github.com/user-attachments/assets/c02ba947-13df-4272-a888-de2e44ec2d12" />

<img width="484" height="593" alt="download" src="https://github.com/user-attachments/assets/076634aa-561b-4b56-aeaa-96a35531de3d" />

<img width="640" height="785" alt="download" src="https://github.com/user-attachments/assets/091ab053-475f-42f6-81c1-3742642b88e2" />

<img width="1615" height="934" alt="download" src="https://github.com/user-attachments/assets/f9067210-7781-442d-a5af-4054a47955e1" />

## Result

Thus, sunglasses are successfully added to the passport-size photo using OpenCV and NumPy. Image resizing, masking, and overlay techniques are used to position the sunglasses correctly over the face and generate the final output.

