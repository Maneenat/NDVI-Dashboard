# User Guide and Presentation Script
## Mae Suai Spatial Decision-Support System

This guide is prepared for demonstrating the system and presenting the project to an instructor. The project includes a landing page, an interactive dashboard, a Python backend API, and NDVI analysis through Google Earth Engine (GEE).

## 1. Project Overview

The system analyzes land in Mae Suai District, Chiang Rai, by combining three main components:

- **ML Land Cover Classification:** Classifies land-use and land-cover types from imagery.
- **Google Earth Engine (GEE):** Retrieves Sentinel-2 imagery and calculates NDVI across multiple years.
- **Decision-Support Dashboard:** Presents parcels, NDVI values, risk levels, and automated recommendations.

The goal is to turn imagery and spatial data into information that is easier to use for decision-making, such as identifying healthy vegetation, areas that require monitoring, and high-risk areas.

## 2. Starting the System

### Recommended Presentation Setup

Open a terminal in the project folder and run:

```bash
cd /Users/maneenat/Desktop/CEI-Dataset
python3 backend.py
```

Then open the following URL in a browser:

```text
http://127.0.0.1:8000/
```

The landing page will appear. Click **Go to Dashboard** to open the analysis page.

### Connecting to Google Earth Engine

To use live GEE data, authenticate Earth Engine first:

```bash
pip install -r requirements.txt
earthengine authenticate
GEE_PROJECT=your-google-cloud-project python3 backend.py
```

If GEE credentials are not configured, the system uses a deterministic **Demo fallback**. This allows the interface and API flow to be demonstrated without a live Earth Engine connection.

## 3. Using the Landing Page

1. Open `http://127.0.0.1:8000/`.
2. Use the Hero section to introduce the project as an ML Land Cover and GEE 11-Year NDVI Analysis system.
3. Point out the **Go to Dashboard** button.
4. Explain the four workflow steps:
   - **Step 1:** ML Classification Model with 74.03% accuracy.
   - **Step 2:** GEE NDVI Extraction from 2015 to 2026.
   - **Step 3:** Automated Anomaly Detection and Decision Logic.
   - **Step 4:** Interactive Web Dashboard.
5. Click **Go to Dashboard** to begin the analysis demonstration.

## 4. Using the Dashboard

### 4.1 Class Filter

The **Class Filter** allows the user to select a land-cover category:

- **Agriculture:** Agricultural land.
- **Rangeland:** Grassland, open land, or areas covered by low vegetation.
- **Tree Canopy:** Tree-covered or forested areas.
- **Water:** Rivers, ponds, lakes, and other water bodies.

When a category is selected, related parcels remain emphasized while other parcels become less prominent, making comparison easier.

### 4.2 Parcel Selector

The center of the dashboard contains three sample parcels. Click a parcel to update its analysis:

- **Parcel A-12:** Target parcel with an example NDVI of 0.62 and low risk.
- **Parcel B-04:** Peer agricultural parcel with an example NDVI of 0.55 and medium risk.
- **Parcel C-19:** Degraded parcel with an example NDVI of 0.21 and high risk.

### 4.3 Decision Analysis

The right-hand panel shows the selected parcel's:

- Parcel name and ID.
- Latest NDVI value.
- Risk level, such as Low Risk, Medium Risk, or High Risk.
- Automated recommendation based on the parcel's condition.

These values update when a different parcel is selected.

## 5. Analysis Principles

NDVI is calculated from near-infrared and red-band reflectance. The basic formula is:

```text
NDVI = (NIR - Red) / (NIR + Red)
```

A simple interpretation is:

- **High NDVI:** Healthy or dense vegetation.
- **A continuously decreasing NDVI:** Possible degradation, drought, or land-use change.
- **A strongly negative anomaly:** An area that should receive further investigation.

In the backend, the latest NDVI value is compared with the average NDVI over the selected analysis period. The system then assigns a risk level based on the decision rules.

## 6. Presentation Script

> Good morning/afternoon. Today I will present the **Mae Suai Spatial Decision-Support System**, a spatial decision-support system for Mae Suai District, Chiang Rai.
>
> The problem this project addresses is that satellite imagery and vegetation-change data can be difficult to interpret when viewed separately. This system combines Machine Learning for land-cover classification with Google Earth Engine for NDVI analysis from 2015 to 2026.
>
> This landing page summarizes the system in four steps. The first step is the ML Classification Model, which classifies land-cover types with an accuracy of 74.03 percent. The second step is extracting NDVI data through Google Earth Engine. The third step is detecting anomalies and converting them into risk levels. The final step is presenting the results through an interactive web dashboard.
>
> I will now click **Go to Dashboard** to open the analysis page. On the left side, the Class Filter allows us to filter categories such as Agriculture, Rangeland, Tree Canopy, and Water.
>
> In the center is the Parcel Selector, which contains three sample parcels. I will start with Parcel A-12. Its NDVI value is 0.62 and it is classified as Low Risk, meaning that its vegetation condition is relatively healthy.
>
> Next, I will select Parcel B-04. Its NDVI value is 0.55 and the system classifies it as Medium Risk. The recommendation is to monitor moisture and changes during the next analysis period.
>
> Finally, I will select Parcel C-19. Its NDVI value is 0.21, so the system identifies it as High Risk because the vegetation signal is substantially lower than the normal condition. The system recommends field verification and avoiding a single-crop decision without further assessment.
>
> On the backend, the system attempts to retrieve Sentinel-2 data and calculate NDVI through Google Earth Engine. When GEE is configured, the system can return live analysis results. When credentials are not available, the Demo fallback allows the dashboard workflow to be tested without interrupting the presentation.
>
> In summary, this system does not only display a map or an NDVI value. It connects land-cover classification, time-series monitoring, anomaly detection, and recommendations in one decision-support workflow.

## 7. Possible Questions and Answers

### Why did you choose NDVI?

NDVI is a practical index for estimating vegetation health and greenness from satellite data. It is suitable for monitoring land changes over time without requiring a field survey at every location.

### Can the system work without GEE?

Yes. The interface and API can still run because the system includes a Demo fallback for testing. However, live analysis results require Earth Engine authentication and a configured Google Cloud project.

### Does High Risk mean that the area is definitely damaged?

No. It is an indicator for prioritizing an area for further investigation. NDVI can also be affected by clouds, seasons, image quality, and other environmental factors, so field verification is still important.

### Can the system be used in another area?

In principle, yes. The geographic boundary, imagery, training dataset, model, and decision thresholds would need to be adapted to the new area.

### Does the system make decisions for the user?

No. It is a Decision Support System. It summarizes data and prioritizes risk, but it does not replace expert judgment or field surveys.

## 8. Closing the Demonstration

After the demonstration, press `Ctrl+C` in the terminal to stop the backend. Conclude by explaining that the dashboard supports both live GEE analysis when credentials are configured and Demo fallback mode for interface and workflow testing.
