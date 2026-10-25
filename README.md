# Car Price Prediction with FastAPI

A machine-learning project for predicting a car's price from its specifications. It includes exploratory and training notebooks, a preprocessing script, a FastAPI prediction endpoint, and a simple browser-based form.

## Project structure

```text
API/
  index.html                  Browser-based prediction form
  main.py                     FastAPI application
data/
  car_price_prediction.csv    Dataset
models/
  label_encoders.pkl          Fitted categorical label encoders
  one_hot_encoders.pkl        Fitted one-hot encoder
  scaler.pkl                  Fitted numerical scaler
  model.pkl                   Trained estimator (not included in this repository)
notebooks/                    Data exploration and model training notebooks
scripts/
  preprocessing.py            Data cleaning and feature engineering helpers
```

## Requirements

- Python 3.9 or newer
- FastAPI
- Uvicorn
- pandas
- scikit-learn

Install the API dependencies in a virtual environment:

```powershell
cd API
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install fastapi uvicorn pandas scikit-learn
```

If PowerShell prevents activation, run the commands below using the virtual environment's Python directly:

```powershell
.\.venv\Scripts\python.exe -m pip install fastapi uvicorn pandas scikit-learn
```

## Model file

`models/model.pkl` is excluded from Git because it exceeds GitHub's regular file size limit. The API requires this trained model, as well as the encoder and scaler files in `models/`, to start. Obtain or train the model and place it at `models/model.pkl` before launching the API. The training notebooks are in `notebooks/`.

Only load pickle files from trusted sources. Pickle files can execute code when loaded.

## Run the API

From the repository root, start the application from the `API` directory so its relative model paths resolve correctly:

```powershell
cd API
.\.venv\Scripts\Activate.ps1
uvicorn main:app --reload
```

The API runs at `http://127.0.0.1:8000`. Interactive endpoint documentation is available at `http://127.0.0.1:8000/docs`.

## Use the web form

With the API running, open `API/index.html` in a browser, fill in the car details, and select **Predict**. The page sends a request to `http://127.0.0.1:8000/predict/`.

## Prediction endpoint

`POST /predict/` accepts a JSON object with these fields:

| Field | Type |
| --- | --- |
| `Levy` | integer |
| `Manufacturer` | string |
| `Model` | string |
| `Prod_year` | integer |
| `Category` | string |
| `Leather_interior` | string |
| `Fuel_type` | string |
| `Engine_volume` | number |
| `Mileage` | integer |
| `Cylinders` | number |
| `Gear_box_type` | string |
| `Drive_wheels` | string |
| `Wheel` | string |
| `Color` | string |
| `Airbags` | integer |

Example request:

```json
{
  "Levy": 1399,
  "Manufacturer": "LEXUS",
  "Model": "RX 450",
  "Prod_year": 2020,
  "Category": "Jeep",
  "Leather_interior": "Yes",
  "Fuel_type": "Hybrid",
  "Engine_volume": 3.5,
  "Mileage": 156423,
  "Cylinders": 6,
  "Gear_box_type": "Automatic",
  "Drive_wheels": "4x4",
  "Wheel": "Left wheel",
  "Color": "Silver",
  "Airbags": 12
}
```

Successful responses contain the model output:

```json
{
  "prediction": 12345.67
}
```

The prediction value and its units depend on the model and training data.