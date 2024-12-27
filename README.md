# CIFAR10 Image Classification

## Model Architecture (C1C2C3C40)

The model follows a C1C2C3C40 architecture with the following components:

1. **C1 (Initial Block)**
   - Conv2d(3, 24, k=3, p=1)
   - BatchNorm2d + ReLU + Dropout(0.03)
   - RF: 3x3

2. **C2 (Depthwise Separable Block)**
   - DepthwiseSeparableConv(24, 48, k=3, s=2)
   - BatchNorm2d + ReLU + Dropout(0.03)
   - RF: 7x7

3. **C3 (Dilated Conv Block)**
   - Conv2d(48, 84, k=3, d=2, p=2)
   - BatchNorm2d + ReLU + Dropout(0.03)
   - Conv2d(84, 84, k=3, s=2, p=1)
   - BatchNorm2d + ReLU + Dropout(0.1)
   - RF: 15x15

4. **C4 (Strided Conv Block)**
   - Conv2d(84, 128, k=3, s=2, p=1)
   - BatchNorm2d + ReLU + Dropout(0.1)
   - RF: 45x45

5. **Output (0)**
   - Global Average Pooling
   - Conv2d(128, 10, k=1)
   - Log Softmax

### Key Features
- No MaxPooling (replaced with strided convolutions)
- Uses Depthwise Separable Convolution in C2
- Uses Dilated Convolution in C3
- Total Parameters: ~200k
- Final Receptive Field: 45x45

## Image Augmentation Techniques

Using Albumentations library with the following transforms:

1. **HorizontalFlip**
   - Probability: 0.5

2. **ShiftScaleRotate**
   - Shift Limit: 0.1
   - Scale Limit: 0.1
   - Rotate Limit: 15°
   - Probability: 0.5

3. **CoarseDropout**
   - Max Holes: 1
   - Max Height: 16px
   - Max Width: 16px
   - Min Holes: 1
   - Min Height: 16px
   - Min Width: 16px
   - Fill Value: Dataset Mean
   - Probability: 0.5

### Sample Images

#### Before Augmentation
![Before Augmentation](cifar10_grid_before_augmentation.png)

#### After Augmentation
![After Augmentation](cifar10_grid_after_augmentation.png)


## Training Summary

| Metric               | Value   |
|-----------------------|---------|
| Total Parameters     | 200.9k  |
| Best Test Accuracy   | 84.56%  |
| Final Train Accuracy | 82.34%  |
| Final Test Accuracy  | 84.41%  |
| Learning Rate        | 0.01    |
| Momentum             | 0.9     |
| Batch Size           | 128     |
| Total Epochs         | 100     |

## Epoch-wise Performance
| Epoch | Train Accuracy | Test Accuracy |
|-------|-----------------|---------------|
| 1     | 37.24%          | 48.51%        |
| 2     | 47.97%          | 54.87%        |
| 3     | 53.03%          | 59.64%        |
| 4     | 56.39%          | 62.94%        |
| 5     | 58.72%          | 64.96%        |
| 6     | 60.68%          | 66.15%        |
| 7     | 61.98%          | 66.97%        |
| 8     | 63.63%          | 68.99%        |
| 9     | 64.63%          | 71.28%        |
| 10    | 65.80%          | 69.46%        |
| 11    | 66.55%          | 71.85%        |
| 12    | 67.16%          | 72.21%        |
| 13    | 68.22%          | 71.58%        |
| 14    | 68.76%          | 74.54%        |
| 15    | 69.27%          | 75.61%        |
| 16    | 69.60%          | 75.25%        |
| 17    | 70.26%          | 76.40%        |
| 18    | 70.99%          | 76.31%        |
| 19    | 71.51%          | 76.33%        |
| 20    | 71.93%          | 75.58%        |
| 21    | 72.33%          | 77.36%        |
| 22    | 72.62%          | 78.13%        |
| 23    | 73.06%          | 77.21%        |
| 24    | 73.41%          | 77.32%        |
| 25    | 73.39%          | 77.77%        |
| 26    | 73.83%          | 78.93%        |
| 27    | 74.34%          | 77.87%        |
| 28    | 74.45%          | 78.92%        |
| 29    | 74.65%          | 78.27%        |
| 30    | 75.00%          | 80.03%        |
| 31    | 75.48%          | 79.42%        |
| 32    | 75.35%          | 79.38%        |
| 33    | 75.74%          | 79.15%        |
| 34    | 76.02%          | 80.09%        |
| 35    | 76.07%          | 78.87%        |
| 36    | 76.09%          | 80.12%        |
| 37    | 76.61%          | 79.11%        |
| 38    | 76.56%          | 80.60%        |
| 39    | 76.88%          | 80.46%        |
| 40    | 77.12%          | 80.21%        |
| 41    | 77.40%          | 80.60%        |
| 42    | 77.14%          | 80.50%        |
| 43    | 77.48%          | 81.20%        |
| 44    | 77.67%          | 81.74%        |
| 45    | 77.57%          | 81.35%        |
| 46    | 77.64%          | 81.50%        |
| 47    | 77.98%          | 81.95%        |
| 48    | 78.04%          | 81.40%        |
| 49    | 78.29%          | 82.29%        |
| 50    | 78.04%          | 82.04%        |
| 51    | 78.71%          | 82.51%        |
| 52    | 78.68%          | 81.33%        |
| 53    | 78.56%          | 82.63%        |
| 54    | 78.86%          | 81.92%        |
| 55    | 78.81%          | 82.95%        |
| 56    | 78.89%          | 82.57%        |
| 57    | 79.38%          | 82.68%        |
| 58    | 79.48%          | 81.89%        |
| 59    | 79.31%          | 82.35%        |
| 60    | 79.41%          | 83.04%        |
| 61    | 79.63%          | 82.70%        |
| 62    | 79.77%          | 82.29%        |
| 63    | 80.00%          | 82.56%        |
| 64    | 80.03%          | 82.51%        |
| 65    | 80.21%          | 83.03%        |
| 66    | 79.95%          | 82.37%        |
| 67    | 80.05%          | 83.36%        |
| 68    | 80.32%          | 83.08%        |
| 69    | 80.39%          | 84.15%        |
| 70    | 80.34%          | 83.67%        |
| 71    | 80.51%          | 83.41%        |
| 72    | 80.57%          | 83.57%        |
| 73    | 80.59%          | 83.44%        |
| 74    | 80.51%          | 83.36%        |
| 75    | 80.65%          | 83.73%        |
| 76    | 81.00%          | 83.60%        |
| 77    | 80.97%          | 83.89%        |
| 78    | 80.94%          | 83.66%        |
| 79    | 80.77%          | 84.07%        |
| 80    | 80.81%          | 83.77%        |
| 81    | 81.15%          | 83.01%        |
| 82    | 81.29%          | 83.94%        |
| 83    | 81.34%          | 83.40%        |
| 84    | 81.28%          | 83.48%        |
| 85    | 81.49%          | 83.78%        |
| 86    | 81.30%          | 84.14%        |
| 87    | 81.53%          | 84.02%        |
| 88    | 81.64%          | 83.60%        |
| 89    | 81.54%          | 84.12%        |
| 90    | 81.39%          | 83.79%        |
| 91    | 81.55%          | 83.90%        |
| 92    | 81.53%          | 84.50%        |
| 93    | 81.88%          | 83.90%        |
| 94    | 81.58%          | 83.92%        |
| 95    | 81.84%          | 83.77%        |
| 96    | 82.09%          | 84.56%        |
| 97    | 82.18%          | 84.07%        |
| 98    | 81.92%          | 84.44%        |
| 99    | 82.00%          | 83.62%        |
| 100   | 82.34%          | 84.41%        |
