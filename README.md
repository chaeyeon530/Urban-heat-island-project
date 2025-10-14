# Urban-heat-island-project
# 🌆 Urban Heat Island Analysis: Seoul vs Melbourne

> **Team Project | La Trobe Exchange 2025**  
> **Team Members:** 권하림 · 이채연(Hani) · 임연지 · 김현지  
> **Supervisor:** David (La Trobe University)

---

## 🧭 Project Overview

This project investigates **urban heat island (UHI)** effects by comparing **Seoul (South Korea)** and **Melbourne (Australia)** using geospatial and environmental datasets.

We analyzed how factors such as **building density**, **green space**, **road length**, and **air pollution** correlate with **average temperature changes (2016 → 2023)**.

**Main Tools:**  
PostgreSQL + PostGIS · QGIS · R (`ggplot2`, `gstat`, `raster`)  
**Data Source:**  
KMA (Korea Meteorological Administration), OpenStreetMap, Statistics Korea, Australian Bureau of Meteorology

---

## 📂 Data Pipeline

```
요약:
	1.	기상청 & OSM 데이터 수집
	2.	PostGIS로 공간결합 및 테이블 구축
	3.	IDW 보간법으로 기온 분포 지도 생성
	4.	행정동 단위로 환경요소 집계
	5.	R에서 상관분석 및 시각화 수행
	6.	서울–멜버른 간 열섬 양상 비교
```
# 🌡 Urban Heat Island Analysis: Seoul vs Melbourne

📍 *This project was conducted as part of the La Trobe AI & Data Science Exchange Program (2025).*  
📍 *본 프로젝트는 2025년 라트로브 AI & 데이터사이언스 교환 프로그램의 일환으로 수행되었습니다.*

---

## 🧮 Analysis Process | 분석 과정

### 1️⃣ Data Collection & Preprocessing
- **AWS Temperature Data (2016, 2023, August)**
  - 일별 평균기온을 지점별로 평균화 후 공간좌표(`geom`) 결합
- **Spatial Layers**
  - 서울 행정경계, 건물, 도로, 녹지, 수역, 인구, 오염물질 레이어 수집

---

### 2️⃣ Spatial Interpolation (IDW 보간법)
- **Method:** Inverse Distance Weighting (IDW)
- **Tools:** R (`gstat`, `raster`)
- **근거:** 기상청의 기온분포도 작성 방식과 동일한 IDW 근거 적용
- **결과:** Raster → Polygon 변환 후 DB(PostGIS)에 저장

---

### 3️⃣ Spatial Aggregation (PostGIS)
행정동(`adm_nm`) 단위로 환경요소를 결합하는 SQL 수행  
온도, 빌딩, 녹지, 도로, 수역, 인구, 오염도 등을 하나의 데이터셋으로 통합

| Feature | 설명 | SQL 연산 |
|:--|:--|:--|
| 🏢 **Buildings** | 건물 수 및 총 면적 | `ST_Intersects`, `ST_Union`, `ST_Area` |
| 🌳 **Green Area** | 공원·숲 면적 | `ST_Intersection` + `landuse` 필터링 |
| 🚗 **Roads** | 도로 총 길이 | `ST_Length` |
| 💧 **Water Area** | 강·저수지 등 수역 면적 | `ST_Area` |
| 👥 **Population** | 인구수 | 행정동 테이블 `JOIN` |
| 🌫 **Air Pollution** | PM10, PM2.5, NO₂, CO, O₃ | `JOIN` 으로 결합 |

---

### 4️⃣ Correlation Analysis (R)
PostGIS에서 불러온 데이터를 R에서 상관분석으로 시각화  
`ggplot2`를 사용해 회귀선 및 산점도 그래프를 함께 표현

| 변수 | 기온과의 상관계수 |
|:--|:--|
| 인구수 | +0.16 |
| 빌딩면적 | +0.08 |
| 녹지면적 | −0.26 |
| 물 면적 | +0.23 |
| 도로 길이 | −0.01 |
| 미세먼지 (PM10) | +0.37 |
| 이산화질소 (NO₂) | +0.39 |
| 초미세먼지 (PM2.5) | +0.35 |

**해석**
- 인구밀도·건물면적·오염물질 ↑ → 기온 상승  
- 녹지율 ↑ → 기온 하락

---

### 5️⃣ Results Summary

#### 🏙 Seoul (2016 → 2023)
- 평균기온 상승 뚜렷  
- 녹지 감소 지역 중심으로 열섬 강화  
- 도로·건물 밀집 지역과 기온 간 양의 상관 확인  

#### 🇦🇺 Melbourne
- 넓은 녹지 비율과 분산된 도시 구조로 열섬 완화  
- 빌딩 밀도와 기온의 상관관계가 서울보다 낮음  

---

### 6️⃣ Key Findings
- IDW 보간법이 도시 기온 시각화에 적합함을 검증  
- 녹지면적과 기온 간 음의 상관관계 명확히 확인  
- 서울의 열섬 완화 대책으로 **녹지 확충의 중요성 제시**

---

## 🧩 Tools & Technologies | 사용 도구

| Category | Tool |
|:--|:--|
| Database | PostgreSQL + PostGIS |
| Spatial Processing | QGIS |
| Statistical Analysis | R (`ggplot2`, `gstat`, `raster`) |
| Visualization | QGIS · ggplot2 · Tableau |
| Version Control | GitHub |
| Documentation | Notion |

---

## 🧑‍🤝‍🧑 Team Roles | 팀 역할

| Member | Role | Contribution |
|:--|:--|:--|
| 임연지 | R Analyst | 상관분석 및 시각화(`ggplot2`) |
| 권하림 | GIS Specialist | SQL 쿼리, 공간데이터 처리 |
| **이채연 (Hani)** | Data Engineer | 데이터 파이프라인 설계, 발표자료 구성 |
| 김현지 | Research Lead | 참고 논문 조사 및 멜버른 비교 분석 |

---

## 🧠 Key Takeaways | 핵심 인사이트
- IDW 보간법이 도시 기온 시각화에 적합함을 검증  
- 녹지면적과 기온 간 음의 상관관계 명확히 확인  
- 서울의 열섬 완화 대책으로 **녹지 확충의 중요성 제시**

---

## 🪄 Next Steps | 후속 연구 제안
- 2000–2025년 장기 시계열 데이터 확장  
- 머신러닝 기반 열섬 예측 모델 구축 (Python + TensorFlow)

---

## 📎 References | 참고문헌
- KMA Data Portal (기상청 데이터포털)  
- OpenStreetMap  
- GIS Developer: IDW 보간법 설명  
- La Trobe University GIS Course Resources
