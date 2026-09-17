# Mae Suai Spatial Decision-Support System
## Slide 1: Title

**Mae Suai Spatial Decision-Support System**

A spatial decision-support system for land-use analysis

- Mae Suai District, Chiang Rai
- ML Land-Cover Classification + GEE NDVI Analysis
- Presenter: ____________________
- Date: ____________________

**Speaker Notes**

Good morning/afternoon. Today I will present the Mae Suai Spatial Decision-Support System, a spatial decision-support system for Mae Suai District, Chiang Rai. The system combines Machine Learning for land-cover classification with NDVI-based vegetation-health analysis through Google Earth Engine in one dashboard.

---

## Slide 2: Project Motivation

### Why are we building this system?

**A Public Spatial Decision-Support Dashboard for Mae Suai**

The system is designed to help residents, farmers, and investors understand land conditions and make more informed land-use decisions.

- Land and vegetation data can be difficult to interpret separately.
- Manual inspection of every area takes time.
- Long-term vegetation changes may be missed from a single image.
- Different users need complex information in a simple, accessible format.
- Decision-makers need clear risk signals and recommendations before using an area.

### Who benefits?

- **Farmers:** Identify declining vegetation health before investing in crops.
- **Residents and communities:** Understand local land conditions without GIS expertise.
- **Investors and area planners:** Screen land suitability for agriculture or development.
- **Local decision-makers:** Prioritize areas for field inspection and planning.

**Speaker Notes**

Before explaining the technical model, I would like to begin with why we are building this system. It is designed as a Public Spatial Decision-Support Dashboard so that residents, farmers, and investors can access information to support land-use decisions in Mae Suai District.

The main problem is that there is a large amount of imagery and vegetation data. A single image cannot show long-term change, and manually inspecting every area takes time. This system combines complex information into an easy-to-understand result by showing land type, NDVI trends, risk levels, and initial recommendations.

The benefits vary by user group. Farmers can check whether an area shows signs of degradation before investing in crops. Residents can understand local land conditions without GIS expertise. Investors and planners can use the system for initial land-suitability screening. Local agencies can prioritize areas for field inspection.

---

## Slide 3: System Objective

### From complex spatial data to useful decisions

The system transforms technical data into simple recommendations that people can understand and act on.

1. Classify land-cover types from imagery.
2. Monitor vegetation health over time.
3. Detect unusual changes using NDVI anomalies.
4. Present risk levels and recommendations in a public web dashboard.

### What problem does it solve?

Instead of asking users to interpret raw satellite images, classification masks, and NDVI tables separately, the dashboard connects them into one workflow:

**Land type + vegetation trend + anomaly signal = decision support**

**Key idea:** The system supports decisions; it does not replace expert judgment or field surveys.

**Speaker Notes**

The objective is to transform technical data into information that people can actually use. Instead of asking users to view satellite images, classification masks, and NDVI tables separately, the system connects land type, vegetation trends, and anomaly signals in one workflow.

The system helps answer three initial questions: What type of land is this? Is vegetation health improving or declining? Should the area be used in a particular way or investigated further? The system supports decisions, but it does not replace experts or field surveys.

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

The workflow starts with imagery. The Machine Learning model classifies land-cover types. The system then retrieves Sentinel-2 data through Google Earth Engine and calculates annual NDVI values. These values are compared with a baseline to identify anomalies. Finally, the results are presented in the web dashboard.

---

## Slide 5: ML Land-Cover Classification

### Model method

**Satlas + UperNet + SwinV2B**

- Satlas is used as the pretrained foundation.
- UperNet is used as the segmentation architecture.
- SwinV2B is used as the backbone model.
- The model produces a land-cover classification mask for each image.

### Training and test setup

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

### Model performance on the CEI test set

| Training set | Overall Accuracy | mIoU | Mean F1-score |
|---|---:|---:|---:|
| Open Earth Map | 59.03% | 42.15% | 57.78% |
| **IRSA** | **78.98%** | **51.71%** | **63.10%** |

**Best result:** The model trained with IRSA achieved the highest performance on the CEI test set.

**Speaker Notes**

The first part of the system is land-cover classification from imagery. The model uses Satlas together with UperNet and SwinV2B. Satlas acts as the pretrained model, UperNet is the segmentation architecture, and SwinV2B is the model backbone.

We compared two training datasets: Open Earth Map and IRSA. CEI images 1 to 100 were used as the test set. The Open Earth Map experiment achieved 59.03 percent Overall Accuracy, 42.15 percent mIoU, and 57.78 percent Mean F1-score.

The IRSA experiment achieved 78.98 percent Overall Accuracy, 51.71 percent mIoU, and 63.10 percent Mean F1-score. This was the best result in the experiment and is the most suitable model for use as the land-cover filter before connecting the results to GEE and the dashboard.

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

NDVI is an index used to estimate vegetation greenness and health. It uses near-infrared and red-band reflectance. Higher NDVI values generally indicate healthier vegetation. This system analyzes the period from 2015 to 2026 to show long-term trends.

---

## Slide 7: Anomaly Detection and Risk Logic

### How the system interprets a parcel

- Calculate the latest NDVI value.
- Calculate the baseline average for the selected period.
- Compare the latest NDVI with the baseline.
- Generate an anomaly value and risk category.

| Example condition | System output |
|---|---|
| Latest NDVI >= 0.55 | Low Risk |
| 0.35 <= Latest NDVI < 0.55 | Medium Risk |
| Latest NDVI < 0.35 | High Risk |

**Important:** Risk categories are indicators for prioritization, not final field confirmation.

**Speaker Notes**

The system compares the latest NDVI value with the average for the selected period and calculates an anomaly value. It then assigns a Low Risk, Medium Risk, or High Risk category. In the current backend logic, an NDVI value below 0.35 is classified as High Risk. This result is used to prioritize further investigation, not to confirm damage by itself.

---

## Slide 8: Dashboard Mockup

### Main dashboard functions

- **Class Filter:** Filter Agriculture, Rangeland, Tree Canopy, and Water.
- **Parcel Selector:** Select A-12, B-04, or C-19.
- **Decision Analysis:** View NDVI, risk level, and recommendation.
- **Interactive response:** Results update when a parcel is selected.

**Demonstration file:** `data/web/dashboard.html`

**Speaker Notes**

This is the dashboard mockup. Users can select a land-cover category with the Class Filter and select a sample parcel with the Parcel Selector. When a parcel is clicked, the NDVI value, risk level, and recommendation on the right side update automatically. The mockup is available in `data/web/dashboard.html`.

---

## Slide 9: Demonstration Scenario

### Compare three sample parcels

| Parcel | NDVI example | Risk | Interpretation |
|---|---:|---|---|
| A-12 | 0.62 | Low Risk | Relatively healthy vegetation |
| B-04 | 0.55 | Medium Risk | Monitor future changes |
| C-19 | 0.21 | High Risk | Possible degraded condition |

### Demo sequence

1. Select Agriculture in the Class Filter.
2. Click Parcel A-12 and explain Low Risk.
3. Click Parcel B-04 and explain the monitoring recommendation.
4. Click Parcel C-19 and explain the high-risk recommendation.

**Speaker Notes**

For the demonstration, I will begin with A-12, which has an NDVI of 0.62 and is classified as Low Risk. Next, I will select B-04, which has an NDVI of 0.55 and should be monitored. Finally, I will select C-19, which has an NDVI of 0.21 and is classified as High Risk. The system recommends field verification before making a land-use decision.

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

Technically, the frontend uses HTML, Tailwind CSS, and JavaScript. The backend uses a Python HTTP server. The API receives parcel geometry as GeoJSON and returns the analysis result. When GEE is configured, the system attempts to use Sentinel-2 data. When credentials are unavailable, the Demo fallback allows the workflow to be demonstrated without interruption.

---

## Slide 11: Limitations and Future Improvements

### Current limitations

- Demo parcels and fallback values are sample data.
- High-risk results should be verified in the field.
- Cloud cover, seasons, and image quality can affect NDVI.
- Model accuracy depends on training data and class balance.

### Future improvements

- Connect the trained inference model directly to the backend.
- Add real parcel and map layers.
- Add export to CSV or KML.
- Add more years, indicators, and validation metrics.
- Improve mobile and accessibility support.

**Speaker Notes**

The current limitations are that the demo parcels and fallback values are sample data. High-risk results should therefore be verified in the field. NDVI can also be affected by seasons, clouds, and image quality. Future improvements include connecting the trained model directly to the backend, adding real parcel layers, supporting CSV or KML export, and adding more indicators and validation metrics.

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

In summary, this system connects land-cover classification, long-term NDVI monitoring, anomaly detection, risk prioritization, and recommendations in one dashboard. Its main benefit is making complex spatial information easier to understand and use for land-use decisions. Thank you.
