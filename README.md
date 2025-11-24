# CIFAR-10-Image-Classification-with-ResNet18

Transfer learning implementation using pre-trained ResNet18 to classify CIFAR-10 images with 81.92% validation accuracy.

## Project Overview

This project demonstrates transfer learning by fine-tuning a ResNet18 model (pre-trained on ImageNet) for CIFAR-10 classification. Includes comprehensive analysis of overfitting detection and per-class performance evaluation.

### Key Results
- **Validation Accuracy**: 81.92%
- **Dataset**: CIFAR-10 (60,000 32x32 color images, 10 classes)
- **Architecture**: ResNet18 with custom 10-class classifier
- **Optimal Stopping**: Epoch 23 (prevented overfitting)

## Technical Implementation

### Model Architecture
- Base: ResNet18 pre-trained on ImageNet
- Modified: Final layer replaced with 10-class output
- Parameters: ~11.7 million

### Training Configuration
- **Loss Function**: Cross-Entropy Loss
- **Optimizer**: SGD (lr=0.001, momentum=0.9)
- **Batch Size**: 128
- **Epochs**: 50 (optimal: 23)
- **Data Split**: 80% train (40K), 20% validation (10K)

### Data Augmentation
- Random horizontal flips
- Random crops (32x32 with padding=4)
- Normalization: mean=(0.5, 0.5, 0.5), std=(0.5, 0.5, 0.5)

## Performance Metrics

### Overall Performance
| Metric | Epoch 23 (Optimal) | Epoch 50 (Overfit) |
|--------|-------------------|-------------------|
| Training Loss | 0.3032 | 0.1100 |
| Validation Loss | 0.5733 | 0.7110 |
| Training Accuracy | 89.21% | 96.08% |
| Validation Accuracy | 81.21% | 81.92% |

### Overfitting Analysis
- **Optimal stopping point**: Epoch 23
- **Validation loss increase after epoch 23**: +24.02%
- **Train-validation gap at epoch 50**: 14.16%

### Per-Class Performance (Test Set)

| Class | Accuracy |
|-------|----------|
| Truck | 91.20% ✅ |
| Ship | 90.70% ✅ |
| Car | 89.10% ✅ |
| Frog | 86.90% |
| Deer | 86.50% |
| Plane | 86.00% |
| Horse | 81.50% |
| Bird | 77.60% |
| Dog | 76.50% ⚠️ |
| Cat | 63.60% ⚠️ |

**Key Finding**: Vehicle classes (avg: 90.33%) significantly outperformed animals (avg: 72.57%) - a 17.76% gap.

## Key Insights

### Overfitting Detection
- Clear divergence between training and validation loss after epoch 23
- Training accuracy continued improving (96.08%) while validation plateaued (81.92%)
- Loss ratio worsened from 1.9x to 6.5x

### Class-Specific Challenges
**Why animals performed poorly:**
- Similar textures (fur) between cats/dogs at 32x32 resolution
- Pose variations harder to distinguish at low resolution
- Potentially similar in ImageNet features

**Why vehicles performed well:**
- Distinct geometric shapes
- High contrast with backgrounds
- Better representation in ImageNet pre-training

## Recommended Improvements

1. **Early Stopping**: Implement automatic stopping at validation loss minimum
2. **Data Augmentation**: Add targeted augmentations for animal classes
   - Rotation (±30°)
   - Color jittering
   - Random occlusion
3. **Class Weighting**: Penalize misclassifications of difficult classes more heavily
4. **Two-Stage Classification**: 
   - Stage 1: Animals vs Vehicles
   - Stage 2: Specific class within category

## Training Curves

<img width="562" height="356" alt="Screenshot 2025-11-22 at 3 12 56 PM" src="https://github.com/user-attachments/assets/a23b2981-2673-4954-aafc-a8e2097e9d65" />

## Author
Lisa Tran
- [GitHub](https://github.com/lisatran183)
- [LinkedIn](https://www.linkedin.com/in/lisa-tran-analytics/)
