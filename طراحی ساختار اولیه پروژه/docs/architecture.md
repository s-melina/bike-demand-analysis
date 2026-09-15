# Architecture Contract

## 1. Data Source
- The project uses `day.csv` only for analysis/modeling.
- `hour.csv` is retained in the repository as the original accompanying dataset but is not used in this project.
- `dteday` is converted to datetime and the data is sorted chronologically.

## 2. Target
`cnt`

## 3. Forbidden / Excluded Predictors
- `instant`: record ID
- `casual`: component of target
- `registered`: component of target

Because `cnt = casual + registered`, using `casual` or `registered` creates target leakage.

## 4. Multiple Regression Contract
Allowed:
- `temp`
- `hum`
- `windspeed`
- `yr`
- `workingday`
- `holiday`

Do not use `temp` and `atemp` simultaneously.
Do not directly use coded categorical columns `season`, `mnth`, `weekday`, `weathersit` in the required main multiple regression.

## 5. Temporal Split
After chronological sorting:
- first 80% → Train
- final 20% → Test
- no shuffle

## 6. Scaling
`StandardScaler.fit()` only on Train.
The fitted scaler is then used for Train and Test.

## 7. Required Models
1. Linear Regression — temp
2. Polynomial Regression — degree 2, temp
3. Polynomial Regression — degree 3, temp
4. Multiple Linear Regression — six specified features
5. Multiple Linear Regression + StandardScaler — same six features

## 8. Evaluation
For every model:
- Train R²
- Test R²

Do not hide negative Test R².
Polynomial predictions must not be evaluated outside the observed temperature range.

## 9. Integration Rule
The final notebook is the single source of truth for numerical results used in the report and presentation.
