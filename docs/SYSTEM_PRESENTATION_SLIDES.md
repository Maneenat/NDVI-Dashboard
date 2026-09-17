# Mae Suai Spatial Decision-Support System
## Slide 1: Title

**Mae Suai Spatial Decision-Support System**

ระบบสนับสนุนการตัดสินใจเชิงพื้นที่สำหรับการวิเคราะห์การใช้ประโยชน์ที่ดิน

- Mae Suai District, Chiang Rai
- ML Land Cover Classification + GEE NDVI Analysis
- Presenter: ____________________
- Date: ____________________

**Speaker Notes**

สวัสดีครับ/ค่ะ วันนี้จะนำเสนอ Mae Suai Spatial Decision-Support System หรือระบบสนับสนุนการตัดสินใจเชิงพื้นที่สำหรับอำเภอแม่สรวย จังหวัดเชียงราย ระบบนี้รวมการจำแนกประเภทพื้นที่ด้วย Machine Learning และการวิเคราะห์สุขภาพพืชพรรณด้วย NDVI จาก Google Earth Engine ไว้ใน Dashboard เดียวกัน

---

## Slide 2: Project Motivation

### Why are we building this system?

**A Public Spatial Decision-Support Dashboard for Mae Suai**

The system is designed to help residents, farmers, and investors understand land conditions and make more informed decisions about land use.

- Land and vegetation data can be difficult to interpret separately.
- Manual inspection of every area takes time.
- Long-term vegetation changes may be missed from a single image.
- Different users need the same complex information in a simple, accessible format.
- Decision-makers need clear risk signals and recommendations before using an area.

### Who benefits?

- **Farmers:** Identify areas with declining vegetation health before investing in crops.
- **Residents and communities:** Understand local land conditions without needing GIS expertise.
- **Investors and area planners:** Screen land suitability for agriculture or development.
- **Local decision-makers:** Prioritize areas for field inspection and planning.

**Speaker Notes**

ก่อนอธิบายรายละเอียดของโมเดล ขอเริ่มจากเหตุผลที่เราสร้างระบบนี้ก่อน ระบบนี้ตั้งใจพัฒนาให้เป็น Public Spatial Decision-Support Dashboard เพื่อเปิดให้ประชาชน เกษตรกร และนักลงทุนเข้ามาใช้ข้อมูลประกอบการตัดสินใจเรื่องการใช้ประโยชน์ที่ดินในอำเภอแม่สรวยได้ง่ายขึ้น

ปัญหาหลักคือข้อมูลภาพถ่ายและข้อมูลพืชพรรณมีจำนวนมาก การดูภาพเพียงภาพเดียวไม่สามารถบอกแนวโน้มการเปลี่ยนแปลงในระยะยาวได้ และการตรวจสอบทุกพื้นที่ด้วยคนใช้เวลามาก ระบบนี้จึงช่วยรวมข้อมูลที่ซับซ้อนให้กลายเป็นคำอธิบายที่เข้าใจง่าย โดยแสดงทั้งประเภทพื้นที่ แนวโน้ม NDVI ระดับความเสี่ยง และคำแนะนำเบื้องต้น

ประโยชน์ของระบบแบ่งตามผู้ใช้งาน ได้แก่ เกษตรกรสามารถดูว่าพื้นที่มีแนวโน้มเสื่อมโทรมหรือไม่ก่อนลงทุนปลูกพืช ประชาชนสามารถเข้าใจสภาพพื้นที่โดยไม่จำเป็นต้องมีความรู้ด้าน GIS นักลงทุนและผู้วางแผนสามารถใช้คัดกรองความเหมาะสมเบื้องต้น และหน่วยงานสามารถจัดลำดับพื้นที่ที่ควรลงตรวจสอบภาคสนาม

---

## Slide 3: System Objective

### From complex spatial data to useful decisions

The system transforms technical data into a simple recommendation that people can understand and act on.

1. Classify land-cover types from imagery.
2. Monitor vegetation health over time.
3. Detect unusual changes using NDVI anomalies.
4. Present risk levels and recommendations in a public web dashboard.

### What problem does it solve?

Instead of asking users to interpret raw satellite images, classification masks, and NDVI tables separately, the dashboard connects them into one workflow:

**Land type + vegetation trend + anomaly signal = decision support**

**Key idea:** The system supports decisions; it does not replace expert judgment or field surveys.

**Speaker Notes**

วัตถุประสงค์ของระบบคือเปลี่ยนข้อมูลเชิงเทคนิคให้กลายเป็นข้อมูลที่คนทั่วไปนำไปใช้ได้จริง แทนที่จะให้ผู้ใช้งานเปิดดูภาพดาวเทียม Classification Mask และตาราง NDVI แยกกัน ระบบจะเชื่อมโยงประเภทพื้นที่ แนวโน้มสุขภาพพืชพรรณ และสัญญาณความผิดปกติเข้าด้วยกัน

ดังนั้น ระบบจะช่วยตอบคำถามเบื้องต้นได้ว่า พื้นที่นี้เป็นพื้นที่ประเภทใด พืชพรรณมีแนวโน้มดีขึ้นหรือลดลง และพื้นที่นี้ควรใช้ประโยชน์อย่างไรหรือควรตรวจสอบเพิ่มเติมหรือไม่ โดยระบบเป็นเครื่องมือช่วยตัดสินใจ ไม่ได้ตัดสินใจแทนผู้เชี่ยวชาญหรือแทนการสำรวจภาคสนาม

---

## Slide 4: System Workflow

```text
Aerial / Satellite Imagery
          |
          v
ML Land-Cover Classification
          |
          v
GEE Sentinel-2 NDVI Time Series
          |
          v
Anomaly Detection + Decision Logic
          |
          v
Interactive Web Dashboard
```

**Speaker Notes**

Workflow เริ่มจากข้อมูลภาพ จากนั้นโมเดล Machine Learning จำแนกประเภทพื้นที่ ระบบดึงข้อมูล Sentinel-2 ผ่าน Google Earth Engine เพื่อคำนวณ NDVI รายปี แล้วนำค่าเหล่านี้ไปเปรียบเทียบกับค่าเฉลี่ยเพื่อหาความผิดปกติ สุดท้ายแสดงผลใน Dashboard ให้ผู้ใช้ตรวจสอบได้ง่าย

---

## Slide 5: ML Land-Cover Classification

### Model method

**Satlas + UperNet + SwinV2B**

- Satlas เป็น pretrained foundation model
- UperNet เป็น segmentation architecture
- SwinV2B เป็น backbone ของโมเดล
- โมเดลสร้าง land-cover classification mask ให้กับภาพแต่ละภาพ

### Training และ test setup

- **Training set 1:** Open Earth Map
- **Training set 2:** IRSA
- **Test set:** CEI images 1–100

### Land-cover classes

- Rangeland
- Agriculture
- Tree
- Water
- Building
- Road
- Non-Vegetated

### Model performance on CEI test set

| Training set | Overall Accuracy | mIoU | Mean F1-score |
|---|---:|---:|---:|
| Open Earth Map | 59.03% | 42.15% | 57.78% |
| **IRSA** | **78.98%** | **51.71%** | **63.10%** |

**Best result:** โมเดลที่ train ด้วย IRSA ให้ผลดีที่สุดบน CEI test set

**Speaker Notes**

ส่วนแรกของระบบคือการจำแนกประเภทพื้นที่จากภาพ โดยใช้โมเดล Satlas ร่วมกับ UperNet และ SwinV2B โดย Satlas ทำหน้าที่เป็น pretrained model, UperNet เป็นโครงสร้างสำหรับ segmentation และ SwinV2B เป็น backbone ของโมเดล

เราเปรียบเทียบการ train ด้วยข้อมูล 2 ชุด คือ Open Earth Map และ IRSA โดยใช้ภาพ CEI หมายเลข 1 ถึง 100 เป็น test set ผลจาก Open Earth Map ได้ Overall Accuracy 59.03 เปอร์เซ็นต์, mIoU 42.15 เปอร์เซ็นต์ และ Mean F1-score 57.78 เปอร์เซ็นต์

ส่วนผลจาก IRSA ได้ Overall Accuracy 78.98 เปอร์เซ็นต์, mIoU 51.71 เปอร์เซ็นต์ และ Mean F1-score 63.10 เปอร์เซ็นต์ จึงเป็นผลที่ดีที่สุดในชุดการทดลองนี้ และเป็นโมเดลที่เหมาะจะนำไปใช้เป็นตัวกรองประเภทพื้นที่ก่อนเชื่อมต่อกับ GEE และ Dashboard

---

## Slide 6: NDVI Analysis with GEE

### Normalized Difference Vegetation Index

```text
NDVI = (NIR - Red) / (NIR + Red)
```

- Uses near-infrared and red-band reflectance.
- Higher values generally indicate healthier vegetation.
- The system analyzes a time range from **2015 to 2026**.
- Sentinel-2 data is processed through Google Earth Engine.

**Speaker Notes**

NDVI เป็นดัชนีที่ใช้ประเมินความเขียวและสุขภาพของพืช โดยใช้ข้อมูลการสะท้อนแสงย่านใกล้อินฟราเรดและย่านสีแดง ค่า NDVI ที่สูงมักแสดงถึงพืชพรรณที่สมบูรณ์กว่า ระบบนี้วิเคราะห์ข้อมูลในช่วงปี 2015 ถึง 2026 เพื่อให้เห็นแนวโน้มระยะยาว

---

## Slide 7: Anomaly Detection and Risk Logic

### How the system interprets a parcel

- Calculate the latest NDVI value.
- Calculate the baseline average for the selected period.
- Compare latest NDVI with the baseline.
- Generate an anomaly value and risk category.

| Example condition | System output |
|---|---|
| Latest NDVI >= 0.55 | Low Risk |
| 0.35 <= Latest NDVI < 0.55 | Medium Risk |
| Latest NDVI < 0.35 | High Risk |

**Important:** Risk categories are indicators for prioritization, not final field confirmation.

**Speaker Notes**

ระบบจะนำค่า NDVI ล่าสุดมาเปรียบเทียบกับค่าเฉลี่ยของช่วงเวลาที่เลือก แล้วคำนวณค่า anomaly จากนั้นจัดกลุ่มเป็น Low Risk, Medium Risk หรือ High Risk ใน Backend ปัจจุบันใช้ค่า NDVI ต่ำกว่า 0.35 เป็น High Risk และควรใช้ผลนี้เป็นสัญญาณสำหรับจัดลำดับการตรวจสอบ ไม่ใช่การยืนยันความเสียหายโดยสมบูรณ์

---

## Slide 8: Dashboard Mockup

### Main dashboard functions

- **Class Filter:** Filter Agriculture, Rangeland, Tree Canopy, and Water.
- **Parcel Selector:** Select A-12, B-04, or C-19.
- **Decision Analysis:** View NDVI, risk level, and recommendation.
- **Interactive response:** Results update when a parcel is selected.

**Demonstration file:** `data/web/dashboard.html`

**Speaker Notes**

หน้านี้คือ Mockup ของ Dashboard ผู้ใช้สามารถเลือกประเภทพื้นที่จาก Class Filter และเลือกแปลงตัวอย่างจาก Parcel Selector เมื่อคลิกแปลง ระบบจะเปลี่ยนค่า NDVI ระดับความเสี่ยง และคำแนะนำทางด้านขวา โดยไฟล์สำหรับเปิดดู Mockup คือ `data/web/dashboard.html`

---

## Slide 9: Demonstration Scenario

### Compare three sample parcels

| Parcel | NDVI example | Risk | Interpretation |
|---|---:|---|---|
| A-12 | 0.62 | Low Risk | Relatively healthy vegetation |
| B-04 | 0.55 | Medium Risk | Monitor future changes |
| C-19 | 0.21 | High Risk | Possible degraded condition |

### Demo sequence

1. Select Agriculture in Class Filter.
2. Click Parcel A-12 and explain Low Risk.
3. Click Parcel B-04 and explain monitoring.
4. Click Parcel C-19 and explain the high-risk recommendation.

**Speaker Notes**

ในการสาธิต ผม/ดิฉันจะเริ่มจาก A-12 ซึ่งมีค่า NDVI 0.62 และความเสี่ยงต่ำ จากนั้นเลือก B-04 ซึ่งมีค่า NDVI 0.55 และควรติดตามต่อ สุดท้ายเลือก C-19 ที่มีค่า NDVI 0.21 ระบบจัดเป็น High Risk และแนะนำให้ตรวจสอบภาคสนามก่อนตัดสินใจใช้พื้นที่

---

## Slide 10: Technical Architecture

### Components

- **Frontend:** HTML, Tailwind CSS, JavaScript
- **Backend:** Python HTTP server
- **API:** `POST /api/analyze`
- **Remote sensing:** Google Earth Engine and Sentinel-2
- **Fallback mode:** Deterministic demo data when GEE is unavailable

### Local startup

```bash
python3 backend.py
```

Open: `http://127.0.0.1:8000/`

**Speaker Notes**

ในเชิงเทคนิค Frontend ใช้ HTML, Tailwind CSS และ JavaScript ส่วน Backend ใช้ Python HTTP server มี API สำหรับรับข้อมูลแปลงแบบ GeoJSON และส่งผลวิเคราะห์กลับมา หากตั้งค่า GEE ระบบจะพยายามใช้ข้อมูลจริงจาก Sentinel-2 แต่หากยังไม่มี credential ระบบจะใช้ Demo fallback เพื่อให้สามารถสาธิตการทำงานได้

---

## Slide 11: Limitations and Future Improvements

### Current limitations

- Demo parcels and fallback values are sample data.
- High-risk results should be verified in the field.
- Cloud cover, seasons, and image quality can affect NDVI.
- Model accuracy depends on training data and class balance.

### Future improvements

- Connect a trained inference model directly to the backend.
- Add real parcel and map layers.
- Add export to CSV or KML.
- Add more years, indicators, and validation metrics.
- Improve mobile and user-accessibility support.

**Speaker Notes**

ข้อจำกัดของระบบคือข้อมูล Demo และแปลงตัวอย่างยังเป็นข้อมูลสำหรับทดสอบการทำงาน ผล High Risk จึงควรตรวจสอบภาคสนาม นอกจากนี้ NDVI อาจได้รับผลจากฤดูกาล เมฆ และคุณภาพภาพถ่าย ในอนาคตสามารถเชื่อมโมเดลที่ผ่านการฝึกจริง เพิ่มชั้นข้อมูลแปลงจริง เพิ่มการ Export และเพิ่มตัวชี้วัดอื่นได้

---

## Slide 12: Conclusion

### Key takeaway

The Mae Suai Spatial DSS connects:

- Land-cover classification
- Long-term NDVI monitoring
- Anomaly detection
- Risk prioritization
- Interactive decision support

**Thank you**

### Questions?

**Speaker Notes**

สรุปคือระบบนี้เชื่อมโยงตั้งแต่การจำแนกประเภทพื้นที่ การติดตาม NDVI ระยะยาว การตรวจจับความผิดปกติ ไปจนถึงการจัดลำดับความเสี่ยงและคำแนะนำใน Dashboard จุดเด่นคือช่วยทำให้ข้อมูลเชิงพื้นที่ที่ซับซ้อนเข้าใจง่ายและนำไปใช้ประกอบการตัดสินใจได้มากขึ้น ขอบคุณครับ/ค่ะ
