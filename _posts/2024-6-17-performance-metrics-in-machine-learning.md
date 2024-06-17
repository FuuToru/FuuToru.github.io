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
    path: /imgs/performance-metric-in-ML/performance-metrics-in-machine-learning.png
    alt: Performance Metrics in Machine Learning
---

Performance metrics are crucial in evaluating machine learning models as they quantify how well models perform on given tasks. Every machine learning task can be categorized as either regression or classification, and the performance metrics for each category are designed to handle the specific nature of the problem. This post will serve as a handbook to help you remember these metrics easily. Let's delve into popular metrics for both regression and classification, their key characteristics, and how they are implemented.

### Regression Metrics

Regression models predict continuous output, requiring metrics that evaluate the distance between predicted and actual values.

1. **Mean Squared Error (MSE)**
   - **Formula**:   $$ \text{MSE} = \frac{1}{N} \sum_{j=1}^{N} (y_j - \hat{y}_j)^2 $$
        - **$ y_j $**: Actual value (ground truth) of the j-th data point.
        - **$ \hat{y}_j $**: Predicted value from the regression model for the j-th data point.
        - **$ N $**: Total number of data points.
   - **Key Characteristics**:  
        - Penalizes larger errors more due to the squaring of differences.
        - Sensitive to outliers.
   - **Implementation**:
     ```python
     mse = (y - y_hat) ** 2
     print(f"MSE: {mse.mean():0.2f} (+/- {mse.std():0.2f})")
     ```

2. **Mean Absolute Error (MAE)**
   - **Formula**: $$  \text{MAE} = \frac{1}{N} \sum_{j=1}^{N} \| y_j - \hat{y}_j \|  $$
        - **$ y_j $**: Actual value (ground truth) of the j-th data point.
        - **$ \hat{y}_j $**: Predicted value from the regression model for the j-th data point.
        - **$ N $**: Total number of data points.
   - **Key Characteristics**:  
        - Measures average magnitude of errors.
        - Less sensitive to outliers compared to MSE.
   - **Implementation**:
     ```python
     mae = np.abs(y - y_hat)
     print(f"MAE: {mae.mean():0.2f} (+/- {mae.std():0.2f})")
     ```

3. **Root Mean Squared Error (RMSE)**
   - **Formula**: $$ \text{RMSE} = \sqrt{\frac{1}{N} \sum_{j=1}^{N} (y_j - \hat{y}_j)^2} $$
        - **$ y_j $**: Actual value (ground truth) of the j-th data point.
        - **$ \hat{y}_j $**: Predicted value from the regression model for the j-th data point.
        - **$ N $**: Total number of data points.
   - **Key Characteristics**:  
        - Retains the same units as the original values.
        - Provides a general idea of the error magnitude.
   - **Implementation**:
     ```python
     mse = (y - y_hat) ** 2
     rmse = np.sqrt(mse.mean())
     print(f"RMSE: {rmse:0.2f}")
     ```

4. **R² (R-Squared)**
   - **Formula**: $$ R^2 = 1 - \frac{\sum (y_j - \hat{y}_j)^2}{\sum (y_j - \bar{y})^2} $$  
        - **$ y_j $**: Actual value (ground truth) of the j-th data point.
        - **$ \hat{y}_j $**: Predicted value from the regression model for the j-th data point.
        - **$ \bar{y} $**: Mean of the actual values.
        - **$ N $**: Total number of data points.
   - **Key Characteristics**:  
        - Represents the proportion of variance explained by the model.
        - Values range from 0 to 1, with higher values indicating better fit.
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
   - **Formula**: $$ \text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}  $$
        - **TP**: True Positives (correctly predicted positive instances).
        - **TN**: True Negatives (correctly predicted negative instances).
        - **FP**: False Positives (incorrectly predicted positive instances).
        - **FN**: False Negatives (incorrectly predicted negative instances).
   - **Key Characteristics**:  
        - Simple and intuitive measure.
        - Can be misleading with imbalanced datasets.
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
        - **TP**: True Positives.
        - **FP**: False Positives.
   - **Key Characteristics**:  
        - Focuses on the accuracy of positive predictions.
        - Important when the cost of false positives is high.
   - **Implementation**:
     ```python
     from sklearn.metrics import precision_score
     print(f'Precision: {precision_score(y_test, y_hat)}')
     ```

4. **Recall (Sensitivity/Hit-Rate)**
   - **Formula**: $$ \text{Recall} = \frac{TP}{TP + FN} $$
        - **TP**: True Positives.
        - **FN**: False Negatives.
   - **Key Characteristics**:  
        - Measures the ability to identify positive instances.
        - Important when the cost of false negatives is high.
   - **Implementation**:
     ```python
     from sklearn.metrics import recall_score
     print(f'Recall: {recall_score(y_test, y_hat)}')
     ```

5. **F1-Score**
   - **Formula**: $$ \text{F1-Score} = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}} $$
        - **Precision**: As defined above.
        - **Recall**: As defined above.
   - **Key Characteristics**:  
        - Harmonic mean of precision and recall.
        - Provides a balance between precision and recall, useful for imbalanced datasets.
   - **Implementation**:
     ```python
     from sklearn.metrics import f1_score
     print(f'F1 Score: {f1_score(y_test, y_hat)}')
     ```

6. **AUROC (Area Under the ROC Curve)**
   - **Formula**: $ \text{AUROC} = \int_0^1 TPR(FPR) \, d(FPR) $
        - **TPR (True Positive Rate)**: Proportion of actual positives correctly identified.
        - **FPR (False Positive Rate)**: Proportion of actual negatives incorrectly identified as positive.
   - **Key Characteristics**:  
        - Measures the ability to distinguish between classes.
        - Higher values indicate better performance, with 1 being perfect and 0.5 being random guessing.
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

### References

<a name="performance-metrics"></a>
[1] Aayush Bajaj, "Performance Metrics in Machine Learning [Complete Guide]". 
<a href="https://neptune.ai/blog/performance-metrics-in-machine-learning-complete-guide">https://neptune.ai/blog/performance-metrics-in-machine-learning-complete-guide</a> (2023).


---
I hope this posts helps you start your learning journey effectively. Good luck!

### Comments and discussions 

📍Thanks for spending your time on me.

<iframe src="https://forms.gle/DdmAidKFda4MUDfP6" width="640" height="686" frameborder="0" marginheight="0" marginwidth="0">🔃Đang tải…</iframe>

