# Data science
## Adidas US Sales Datasets

โครงการนี้เป็นส่วนหนึ่งของรายวิชา DS512/DS513 Data Analytics และ DS514/DS515 Data Science 

โดยมีวัตถุประสงค์หลัก คือต้องการรู้ว่า "ปัจจัยใดมีผลต่อยอดขายรวมมากที่สุด"

Dataset: Adidas US Sales Datasets

link Source: https://www.kaggle.com/datasets/heemalichaudhari/adidas-sales-dataset

---
### จุดประสงค์จากการวิเคราะห์

* เพื่อทำความเข้าใจปัจจัยที่ส่งผลต่อยอดขายรวม (Total Sales Drivers)
* เพื่อประเมินประสิทธิภาพของโมเดลที่สร้างขึ้น (Model Performance Evaluation)
* เพื่อระบุแนวโน้มเชิงธุรกิจและ Insight สำคัญ
### Modelที่ใช้การทำนายในครั้งนี้
---
ใช้ Ridge Regression ในการทำนาย เนื่องจาก
* ให้การทำนายที่เสถียรกว่า Linear Regression เพราะไม่ให้ Coefficient ใด่โดดเด่นจนเกินไป
* ป้องกันการ overfitting
* เหมาะเมื่อมีฟีเจอร์ One-Hot
* ตีความ Coefficients ได้ง่ายกว่า Lasso
---
### Hypothesis

H0 (Null Hypothesis): แต่ละช่องทางขายไม่มีผลแตกต่างกันต่อ Total Sales
* H0: การขาย In-store, Online, Outlet ให้ยอดขายรวมเท่ากันโดยไม่มีความแตกต่างอย่างมีนัยสำคัญ
H1 (Alternative Hypothesis): ช่องทางขายมีผลแตกต่างกันต่อ Total Sales
* H1: อย่างน้อยหนึ่งช่องทางขายมีผลต่อ Total Sales แตกต่างจากช่องทางอื่น

