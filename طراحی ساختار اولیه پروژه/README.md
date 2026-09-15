# Bike Sharing Demand Prediction

پروژه تحلیل و پیش‌بینی تقاضای روزانه دوچرخه‌های اشتراکی.

## Important Data Rule
طبق شرح پروژه، برای تحلیل و مدل‌سازی فقط `data/raw/day.csv` استفاده می‌شود.
`hour.csv` در Repository نگهداری می‌شود، اما نباید وارد تحلیل این پروژه شود.

## Dataset
- Daily records: 731
- Hourly records: 17,379
- Target: `cnt`
- Date: `dteday`

## Architecture / Data Contract
1. `dteday` به Date تبدیل و داده‌ها بر اساس تاریخ مرتب شوند.
2. `cnt` متغیر هدف است.
3. `instant` فقط ID است و از مدل حذف می‌شود.
4. `casual` و `registered` از همه مدل‌ها حذف می‌شوند؛ چون:
   `cnt = casual + registered`
   و استفاده از آن‌ها باعث Data Leakage می‌شود.
5. Split زمانی: 80% ابتدایی Train و 20% انتهایی Test؛ بدون shuffle.
6. Linear Regression: `temp -> cnt`
7. Polynomial Regression: درجه 2 و 3 با `temp`
8. Multiple Regression:
   `temp, hum, windspeed, yr, workingday, holiday`
9. `temp` و `atemp` همزمان استفاده نمی‌شوند.
10. `season`, `mnth`, `weekday`, `weathersit` مستقیماً در مدل اصلی چندمتغیره وارد نمی‌شوند.
11. StandardScaler فقط روی Train fit و سپس روی Train/Test transform می‌شود.
12. R² برای Train و Test همه مدل‌ها گزارش می‌شود و R² منفی Test پنهان نمی‌شود.

## Team Roles
- Analyst: Data loading, cleaning, EDA, statistics, charts and interpretations.
- Architect: Repository, contracts, integration, leakage/temporal-split/quality control, final merge and documentation.
- Manager: Coordination, schedule, deliverables, report/presentation coordination.

## Repository
```text
bike_sharing_project/
├── data/
│   ├── raw/
│   │   ├── day.csv
│   │   ├── hour.csv
│   │   └── Readme.txt
│   └── processed/
├── notebooks/
│   └── bike_demand_analysis.ipynb
├── src/
├── reports/
├── docs/
│   └── architecture.md
├── README.md
└── requirements.txt
```

## Run
```bash
pip install -r requirements.txt
jupyter notebook
```

سپس `notebooks/bike_demand_analysis.ipynb` را از ابتدا تا انتها اجرا کنید.
