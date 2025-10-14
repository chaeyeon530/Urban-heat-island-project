# Urban-heat-island-project
# 🌆 Urban Heat Island Analysis: Seoul vs Melbourne

📍 *This project was conducted as part of the La Trobe AI & Data Science Exchange Program (2025).*  
📍 *본 프로젝트는 2025년 라트로브 대학 해외 연수 프로그램의 일환으로 수행되었습니다.*

> **Team Project | La Trobe Exchange 2025**  
> **Team Members:** 권하림 · 이채연 · 김현지  

---

## 🧭 Project Overview | 프로젝트 개요  

이 프로젝트는 **서울과 멜버른의 도시 열섬(UHI) 현상**을 비교하기 위해  
지리공간(Spatial) 및 환경 데이터를 활용하여 진행되었습니다.  
**건물 밀도**, **녹지 면적**, **도로 길이**, **대기 오염도**가  
**평균기온 변화(2016년 → 2023년)** 에 어떤 영향을 미치는지를 분석했습니다.

**Main Tools | 주요 도구:**  
PostgreSQL + PostGIS · QGIS · R

**Data Sources | 데이터 출처:**  
- KMA (기상청, AWS 데이터)  
- OpenStreetMap (건물·도로·녹지 등 공간데이터)  
- 통계청 (인구 데이터)  
- Australian Bureau of Meteorology (멜버른 비교 데이터)
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
| Statistical Analysis | R |
| Visualization | QGIS · ggplot2 · Tableau |
| Version Control | GitHub |
| Documentation | Notion |

---

## 🧠 Key Takeaways | 핵심 인사이트
- IDW 보간법이 도시 기온 시각화에 적합함을 검증  
- 녹지면적과 기온 간 음의 상관관계 명확히 확인  
- 서울의 열섬 완화 대책으로 **녹지 확충의 중요성 제시**

---

## 🪄 Next Steps | 후속 연구 제안
- 2000–2025년 장기 시계열 데이터 확장  
- 머신러닝 기반 열섬 예측 모델 구축 (Python + TensorFlow)
- 맬버른의 도시 열섬 현상을 데이터로 파악해 결과의 신뢰도 높이기

---

## 📎 References | 참고문헌
- 기상청 데이터포털 (기상자료개방 포털, 방재기상관측 자료)
- OpenStreetMap (건물·도로·녹지 등 공간데이터)
- 통계청 (인구 데이터)  
- GIS Developer: IDW 보간법 설명  
- La Trobe University Data science(GIS) Course Resources
