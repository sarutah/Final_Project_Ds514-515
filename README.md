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

