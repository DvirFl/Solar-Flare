# Solo Project - Solar Flare Classification

## Summary
1. Problem Description
2. EDA
3. Model Training
4. Exporting notebook to script
5. Model deployment
6. Dependency and enviroment management
7. Containerization
8. Refrences

## Package Files description
This package contain the following files:
1. notebook.ipynb: The jupiter notebook with the Analysis, EDA, best model determination etc.
2. Pipfile and Pipfile.lock: Pipenv files
3. Dockerfile: Docker configuration file
4. train.py: the notebook exported file in python and transformed in order to generate ai model
5. model.bin: model and DictVectorize
6. predict.py: the service which receive the request of price prediction
7. request: the request of price determination. Invoke the service and sent the POST request.
8. water_potabilty.csv: the dataset from Kaggle
9. README.md: this file with instruction.
10. Screenshots Folder: contain the screenshot included in README.md file


## 1. Problem Description
Solar Flare classification has been 


## 2. EDA
I did the following steps:
1. Check Columns types
2. Check null and duplicated values
3. Check and clean format
4. Convert Potability to boolean
5. Numerical feature correlation (Heatmap)
6. Locate and Trim outliers where needed

## 3. Model Training
1. Subdivide the dataset to 60-20-20 (train, val, test) using scikit-learn libraries
2. Prepare target value arrays
3. Remove target value 'Potability' from the datasets
4. Train the following models on train and finally full_train dataset (train + val datasets):
	* Linear Regression
	* Ridge Regression (for some alpha values)
	* SGD Regressor
	* SVR Regressor
	* Random Forest Regressor
	* Decision Tree Regressor
	* Xgboost Tree Regressor (Tuning eta, max_depth and max_child_weight parameters)
5. For each models, I check the best RMSE values:

## 4. Exporting notebook to script
The Jupiter Notebook was exported in Python.
Prepare the train.py script.
Train.py script read the dataset csv file, train the selected model with full_train dataset and export model and dv in .bin file.

## 5. Model deployment
Once the model.bin is exported and ready to be used, I prepare two scripts and tested it with flask.
The scripts are:
* request.py
* predict.py

#### Request.py

<!-- url = 'http://localhost:9090/predict'


response = requests.post(url, json=flare).json()
print('This image has a solar flare of type : %f' % response['flare_type'])
 -->
Data are encapsulated with json and sent to the service (predict.py)

#### Predict.py 
Manage the service (/predict) and use the model to predict the pizza (sent by POST method and encapsulate with json) price.

#### Flask test
1. Run predict.py script

![Flask_Predict_py](Screenshots/flask_predict_py.png)

2. Run request.py script

![Flask_Request_py](Screenshots/flask_request_py.png)

## 6. Dependency and enviroment management
#### Prepare the Virtual Environment with PIPENV

1. Command: pipenv install numpy pandas scikit-learn==0.24.1 xgboost flask gunicorn requests

#### Access or execute scripts from the Virtual Env

1. Command to access: pipenv shell

![PipEnv_Access](Screenshots/pipenv_access.png) 

2. Command to execute a command: pipenv cmd

Example launch predict.py from pipenv

pipenv run python predict.py

![PipEnv_Predict_py](Screenshots/pipenv_predict_py.png) 

Example launch request.py from pipenv

pipenv run python request.py

![PipEnv_Predict_Request_py](Screenshots/pipenv_predict_request_py.png) 

## 7. Containerization
Create a Docker container with the following steps:

1. sudo docker pull python:3.8.12-slim -> This command download a Docker container with python version 3.8 slim

2. sudo docker run -it --rm --entrypoint=bash python:3.8.12-slim -> This command allow to access to the docker container vm

3. mkdir app -> create the app directory in the docker vm. This folder will contain the model.bin

4. sudo docker build -t midtermproj . -> This command build the docker vm (called midtermproj) with the docker file

	The dockerfile copy the pipfiles, predict.py script and the model.bin into the app folder, install the necessary package using pipfiles, starts the service with gunicorn on port 9090

![Dockerfile](Screenshots/Dockerfile.png)

5. sudo docker run -it --rm -p 9090:9090 solarflareproj -> This command starts the service

6. python request.py -> You can test the services sending a pizza price evaluation

![DockerService](Screenshots/Dockerservice.png)
 

 
