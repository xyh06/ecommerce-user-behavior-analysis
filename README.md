# E-commerce Customer Churn Prediction

## 📌 Project Overview

This project aims to predict customer churn in an e-commerce platform using machine learning techniques.  
The goal is to identify high-risk customers and support targeted retention strategies.

---

## 🎯 Business Objective

- Predict whether a customer will churn
- Segment customers into risk levels
- Improve retention campaign efficiency

---

## 📊 Model Performance

- ROC-AUC: 0.79
- Optimal Threshold: 0.35
- High Risk Churn Rate: 55.9%
- Overall Churn Rate: 28.9%
- Lift: 1.94

The model nearly doubles churn detection precision in the high-risk group.

---

## 🛠 Workflow

1. Data Cleaning
2. Feature Engineering
3. Logistic Regression Baseline
4. Threshold Optimization
5. Risk Segmentation
6. Business Impact Evaluation

---

## 📂 Project Structure

```
ecommerce-user-churn/
│
├── data/
├── notebooks/
├── README.md
└── requirements.txt
```

---

## 🚀 Future Improvements

- XGBoost / LightGBM
- SHAP Model Interpretation
- Cross-validation
- RFM Feature Engineering