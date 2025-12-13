# Data science
## Adidas US Sales Datasets

โครงการนี้เป็นส่วนหนึ่งของรายวิชา DS512/DS513 Data Analytics และ DS514/DS515 Data Science 

โดยมีวัตถุประสงค์หลัก คือต้องการรู้ว่า "ปัจจัยใดมีผลต่อยอดขายรวมมากที่สุด"

Dataset: Adidas US Sales Datasets

link Source: https://www.kaggle.com/datasets/heemalichaudhari/adidas-sales-dataset

---
## จุดประสงค์จากการวิเคราะห์

* เพื่อทำความเข้าใจปัจจัยที่ส่งผลต่อยอดขายรวม (Total Sales Drivers)
* เพื่อประเมินประสิทธิภาพของโมเดลที่สร้างขึ้น (Model Performance Evaluation)
* เพื่อระบุแนวโน้มเชิงธุรกิจและ Insight สำคัญ

## Modelที่ใช้การทำนายในครั้งนี้

ใช้ Ridge Regression ในการทำนาย เนื่องจาก
* ให้การทำนายที่เสถียรกว่า Linear Regression เพราะไม่ให้ Coefficient ใด่โดดเด่นจนเกินไป
* ป้องกันการ overfitting
* เหมาะเมื่อมีฟีเจอร์ One-Hot
* ตีความ Coefficients ได้ง่ายกว่า Lasso

## Preprocessing Data

### กำหนด Target & Features

* Target: Total Sales
* Features: 'Price per Unit', 'Units Sold', 'Sales Method'

### import OneHotEncoder

เพื่อลดความเสี่ยงที่ Model จะเข้าใจผิดระหว่างข้อมูลประเภท Category และข้อมูลประเภท numeric

โดยกำหนดว่า

* Column ไหนเป็นตัวเลข = ไม่ต้องเข้ารหัส One-Hot
* Column ไหนเป็นตัวหนังสือ = ต้องเข้ารหัส One-Hot

### Train-Test Split

* 80% สำหรับ train
* 20% สำหรับ test
* random_state=42 ทำให้ผลลัพธ์เหมือนเดิมทุกครั้ง

## Build Model 
### Ridge with Polynomial (numeric-only) + OneHot (categorical)
* สร้าง sub-pipeline สำหรับคอลัมน์ประเภท categorical
* สร้าง ColumnTransformer
* กำหนด Parameter / ปรับ hyperparameter

### GridSearchCV
* แบ่ง train data ออกเป็น 5 folds
* ลองรัน Ridge แต่ละ alpha
* เลือก alpha ที่ดีที่สุด
* fit data

## Evaluate Best Model
เปรียบเทียบ Train Test เพื่อตรวจจับ Overfit

```python
from sklearn.metrics import r2_score, mean_squared_error
import numpy as np

y_train_pred = best_model.predict(X_train)
y_test_pred  = best_model.predict(X_test)

train_r2 = r2_score(y_train, y_train_pred)
test_r2  = r2_score(y_test, y_test_pred)
train_rmse = np.sqrt(mean_squared_error(y_train, y_train_pred))
test_rmse  = np.sqrt(mean_squared_error(y_test, y_test_pred))

print(f"Train R2: {train_r2:.4f} | Test R2: {test_r2:.4f}")
print(f"Train RMSE: {train_rmse:.4f} | Test RMSE: {test_rmse:.4f}")
```
## Coefficients
* ดึง preprocessor ออกมาจาก pipeline
* ดึงชื่อฟีเจอร์ฝั่ง numeric หลัง PolynomialFeatures
* ดึงชื่อฟีเจอร์ฝั่ง categorical หลัง OneHotEncoder
* รวมชื่อฟีเจอร์ทั้งหมดตามลำดับใน matrix ที่ส่งเข้า Ridge
* ดึง coefficients ของ Ridge
* สร้างตารางค่า coefficient
* เรียงลำดับจากผลกระทบมาก → น้อย (ใช้ absolute value)
* แสดงผล

### สรุป Insight จาก Coefficients

1. ปัจจัยด้าน “ราคา” และ “จำนวนหน่วยขาย” ส่งผลบวกต่อรายได้ตามคาด ราคาต่อหน่วยสูงมีแนวโน้มเชื่อมโยงกับสินค้า high-value ที่สร้างรายได้รวมสูง
2. กลยุทธ์ช่องทางขายมีผลต่อรายได้ In-store สร้างรายได้สูงที่สุด Online และ Outlet มียอดขายรวมลดลงอย่างมากเมื่อเทียบกับ baseline
3. In-store เป็นช่องทางที่มีศักยภาพสร้าง Total Sales สูงที่สุด (แต่ไม่แสดงในตารางเพราะตอนตั้ง OneHotEncoder(drop=first))

## EDA
