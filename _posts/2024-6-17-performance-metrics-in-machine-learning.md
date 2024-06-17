---
title: "Performance Metrics in Machine Learning"
date: 2024-6-17
categories: [Machine Learning]
tags: [overview, basis, machine learning, beginner, metrics]
toc: true
math: true
comments: true
published: true
image: 
    path: /imgs/masteringEnglish/bground.jpeg
    alt: Mastering English
---

Performance metrics are crucial in evaluating machine learning models as they quantify how well models perform on given tasks. Every machine learning task can be categorized as either regression or classification, and the performance metrics for each category are designed to handle the specific nature of the problem. This post will serve as a handbook to help you remember these metrics easily. Let's delve into popular metrics for both regression and classification, their key characteristics, and how they are implemented.

### Regression Metrics

Regression models predict continuous output, requiring metrics that evaluate the distance between predicted and actual values.

1. **Mean Squared Error (MSE)**
   - **Formula**:   $$ \text{MSE} = \frac{1}{N} \sum_{j=1}^{N} (y_j - \hat{y}_j)^2 $$
   - **Key Points**:
     - Differentiable, thus suitable for optimization.
     - Penalizes larger errors more heavily due to squaring.
     - Sensitive to outliers.
   - **Implementation**:
     ```python
     mse = (y - y_hat) ** 2
     print(f"MSE: {mse.mean():0.2f} (+/- {mse.std():0.2f})")
     ```

2. **Mean Absolute Error (MAE)**
   - **Formula**: $$  \text{MAE} = \frac{1}{N} \sum_{j=1}^{N} \| y_j - \hat{y}_j \|  $$
   - **Key Points**:
     - More robust to outliers compared to MSE.
     - Does not exaggerate errors.
     - Non-differentiable.
   - **Implementation**:
     ```python
     mae = np.abs(y - y_hat)
     print(f"MAE: {mae.mean():0.2f} (+/- {mae.std():0.2f})")
     ```

3. **Root Mean Squared Error (RMSE)**
   - **Formula**: $$ \text{RMSE} = \sqrt{\frac{1}{N} \sum_{j=1}^{N} (y_j - \hat{y}_j)^2} $$
   - **Key Points**:
     - Retains differentiability.
     - Scales errors back to the same units as the original data.
     - Less sensitive to outliers than MSE.
   - **Implementation**:
     ```python
     mse = (y - y_hat) ** 2
     rmse = np.sqrt(mse.mean())
     print(f"RMSE: {rmse:0.2f}")
     ```

4. **R² (R-Squared)**
   - **Formula**: $$ R^2 = 1 - \frac{\sum (y_j - \hat{y}_j)^2}{\sum (y_j - \bar{y})^2} $$
   - **Key Points**:
     - Measures the proportion of variance explained by the model.
     - Ranges from -∞ to 1, with 1 indicating a perfect fit.
   - **Implementation**:
     ```python
     SE_line = sum((y - y_hat) ** 2)
     SE_mean = sum((y - y.mean()) ** 2)
     r2 = 1 - (SE_line / SE_mean)
     print(f"R^2: {r2 * 100:0.2f}%")
     ```

### Classification Metrics

Classification models predict discrete output, necessitating metrics that evaluate how well predicted classes match actual classes.

1. **Accuracy**
   - **Formula**: $$ \text{Accuracy} = \frac{\text{Number of Correct Predictions}}{\text{Total Predictions}} \times 100 $$
   - **Implementation**:
     ```python
     from sklearn.metrics import accuracy_score
     print(f'Accuracy Score: {accuracy_score(y_test, y_hat)}')
     ```

2. **Confusion Matrix**
   - **Components**: True Positive (TP), True Negative (TN), False Positive (FP), False Negative (FN)
   - **Implementation**:
     ```python
     from sklearn.metrics import confusion_matrix
     print(confusion_matrix(y_test, y_hat))
     ```

3. **Precision**
   - **Formula**: $$ \text{Precision} = \frac{TP}{TP + FP} $$
   - **Key Points**:
     - Focuses on Type I errors (FP).
   - **Implementation**:
     ```python
     from sklearn.metrics import precision_score
     print(f'Precision: {precision_score(y_test, y_hat)}')
     ```

4. **Recall (Sensitivity/Hit-Rate)**
   - **Formula**: $$ \text{Recall} = \frac{TP}{TP + FN} $$
   - **Key Points**:
     - Focuses on Type II errors (FN).
   - **Implementation**:
     ```python
     from sklearn.metrics import recall_score
     print(f'Recall: {recall_score(y_test, y_hat)}')
     ```

5. **F1-Score**
   - **Formula**: $$ \text{F1-Score} = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}} $$
   - **Key Points**:
     - Harmonic mean of precision and recall.
     - Useful for imbalanced datasets.
   - **Implementation**:
     ```python
     from sklearn.metrics import f1_score
     print(f'F1 Score: {f1_score(y_test, y_hat)}')
     ```

6. **AUROC (Area Under the ROC Curve)**
   - **Formula**: Plots TPR against FPR at various threshold settings.
   - **Key Points**:
     - Represents the probability that a positive example is ranked above a negative example.
   - **Implementation**:
     ```python
     from sklearn.metrics import roc_auc_score, roc_curve
     import matplotlib.pyplot as plt

     lr_probs = clf.predict_proba(X_test)[:, 1]
     ns_probs = [0 for _ in range(len(y_test))]
     lr_auc = roc_auc_score(y_test, lr_probs)
     ns_auc = roc_auc_score(y_test, ns_probs)
     lr_fpr, lr_tpr, _ = roc_curve(y_test, lr_probs)
     ns_fpr, ns_tpr, _ = roc_curve(y_test, ns_probs)

     plt.plot(lr_fpr, lr_tpr, marker='.', label='Logistic')
     plt.plot(ns_fpr, ns_tpr, linestyle='--', label='No Skill')
     plt.xlabel('False Positive Rate')
     plt.ylabel('True Positive Rate')
     plt.legend()
     plt.show()
     ```

Understanding these metrics helps in selecting the right evaluation technique based on the specific requirements of the machine learning task at hand.