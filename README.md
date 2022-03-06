# Insurance Fraud Classification :nepal:

## Table of Content
  * [Demo](#demo)
  * [Overview](#overview)
  * [Motivation](#motivation)
  * [Technical Aspect](#technical-aspect)
  * [Installation](#installation)
  * [Run](#run)
  * [Deployment on Heroku](#deployment-on-heroku)
  * [Directory Tree](#directory-tree)
  * [Technologies Used](#technologies-used)
  * [Credits](#credits)


## Demo 
Link: [https://insurancefraud12.herokuapp.com/](https://insurancefraud12.herokuapp.com/)

![dipu_github](https://user-images.githubusercontent.com/75604769/156878603-bcac8197-62dd-4b8e-b0f2-c147bc64a987.png)

## Overview
This is a Insurance Fraud Classification Flask app trained on **KMeans, KNN and Xgboost**. Here in this app data is given by the user in the form of **csv format.** 
- Code is organized in a **Modular Format.**
- First user input data is stored in a **Sqlite database** where given csv file is separeted as a good file or a bad file according to specified **Schema file** thus separeted good file is given for a model training. 
- After preprocessing the data we select sample by the means of **Clustering** technique and perform a  hyperparameter tuning then we train a model according to that cluster and select a best model according to their AUC score.


![dipu_github1](https://user-images.githubusercontent.com/75604769/156880590-5f640a12-a1df-4fc6-86bc-f844bdf66c00.png)


## Motivation
On this 2021 fraudulent activity is on the peak so, insurance company can not remain unaffected from fraudulent activity such as fake claim of accident in our case which lead to unnecessary expenditure for the company. So with this point on the mind I tried to implement this project to detect whether a filed claim is **Fraudulent Claim or a real claim** from the customer.

## Technical Aspect
This project is divided into two part
- **1 Data preprocessing, model building and selection.**
   - #### Data Validation
      - Name Validation- We validate the name of the files based on the given name in the schema file. We have created a regex pattern as per the name given in the schema file to use for validation. After validating the pattern in the name, we check for the length of date in the file name as well as the length of time in the file name. If all the values are as per requirement, we move such files to "Good_Data_Folder" else we move such files to "Bad_Data_Folder."
      - Number of Columns - We validate the number of columns present in the files, and if it doesn't match with the value given in the schema file, then the file is moved to "Bad_Data_Folder."
      - Name of Columns - The name of the columns is validated and should be the same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder".
      -  The datatype of columns - The datatype of columns is given in the schema file. It is validated when we insert the files into Database. If the datatype is wrong, then the file is moved to "Bad_Data_Folder".
      -  Null values in columns - If any of the columns in a file have all the values as NULL or missing, we discard such a file and move it to "Bad_Data_Folder".

   - #### Data Insertion in Database
       - Database Creation and connection.
       - Table creation in the database: Table with name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the table is already present, then the new table is not created, and new files are inserted in the already present table as we want training to be done on new as well as old training files.     
       - Insertion of files in the table.

   - #### Model Training
       - **Data Export from Db** - The data in a stored database is exported as a CSV file to be used for model training.
       - **Data Preprocessing**
          - Drop the columns not required for prediction.
          - For this dataset, the null values were replaced with ‘?’ in the client data. Those ‘?’ have been replaced with NaN values.
          - Check for null values in the columns. If present, impute the null values using the categorical imputer.
          - Replace and encode the categorical values with numeric values.
          - Scale the numeric values using the standard scaler.
       - **Clustering**
          - KMeans algorithm is used to create clusters in the preprocessed data.
          - The optimum number of clusters is selected by plotting the elbow plot, and for the dynamic selection of the number of clusters, we are using "KneeLocator" function. The idea behind clustering is to implement different algorithms.
       - **Model Selection**
          - We are using two algorithms, “SVM” and "XGBoost".
          - For each cluster, both the algorithms are passed with the best parameters derived from GridSearch.
          - We calculate the AUC scores for both models and select the model with the best score.
          - The model is selected for each cluster and all the models for every cluster are saved for use in prediction.
       - **Prediction**
          - Data Export from Db - The data in the stored database is exported as a CSV file to be used for prediction.
          - Data Preprocessing(**Same steps as above**)
          - Clustering
          - **Prediction - Based on the cluster number, the respective model is loaded and is used to predict the data for that cluster.**
          - Once the prediction is made for all the clusters, the predictions along with the Wafer names are saved in a CSV file at a given location, and the location is returned to the client.
- **2 Flask app creation and Deployment.**
  - **After flask app is created we deploy it on heroku.**

## Installation
The Code is written in Python 3.7. If you don't have Python installed you can find it [here](https://www.python.org/downloads/). If you are using a lower version of Python you can upgrade using the pip package, ensuring you have the latest version of pip. To install the required packages and libraries, run this command in the project directory after [cloning](https://www.howtogeek.com/451360/how-to-clone-a-github-repository/) the repository:
```bash
pip install -r requirements.txt
```
   
## Run
> **Step 1: Clone the repository.**
```bash
git clone https://github.com/dipesg/Insurance_Fraud.git
```
> **Step 2: Create virtual Environment, either by using conda, pyenv or venv.**
```bash
conda create -n fraud python=3.7 -y
```
> **Step 3: Install dependensies.**
```bash
pip install -r requirements.txt
```
>**Step 4: Run app from terminal.**
```bash
python main.py
```

## Deployment on Heroku
#### **Step 1: Create Required file**
- **Create requirements.txt**
```bash
pip freeze > requirements.txt
```
- **Create Procfile and include:**
```bash
web: gunicorn main:app
```
- **Create runtime .txt and include:**
```bash
python-3.7.12
```

#### **Step 2**
- **Go to Heroku.com and create an account and login.**
- **Click on new(inside the red box) to create a new app.**

![dipu_github2](https://user-images.githubusercontent.com/75604769/156884168-cebe9ce5-591c-4ea6-a9c6-e378f8c1f0c7.png)

- **Give the name of the app and click 'create app'.**
- **For pushing it on heroku follow [[Reference](https://devcenter.heroku.com/articles/config-vars)]**


## Directory Tree
![dir1](https://user-images.githubusercontent.com/75604769/156911395-9aacd999-9977-4e35-96a4-bd8781a64df8.png)


![dir2](https://user-images.githubusercontent.com/75604769/156911406-01bebc1d-de7e-4c96-91cf-2f4dd4fcedf1.png)
 
 
![dir3](https://user-images.githubusercontent.com/75604769/156911414-d365f646-f1f8-46bc-bcf3-8b4a622eaa59.png)


![dir4](https://user-images.githubusercontent.com/75604769/156911423-2ec61fe1-368e-4dfa-9b45-d0b4c73e0085.png)


## Bug / Feature Request
If you find a bug (the website couldn't handle the query and / or gave undesired results), kindly open an issue [here](https://github.com/dipesg/Insurance_Fraud/issues/new) by including your search query and the expected result.

If you'd like to request a new function, feel free to do so by opening an issue [here](https://github.com/dipesg/Insurance_Fraud/issues/new). Please include sample queries and their corresponding results.

## Technologies Used
![](https://forthebadge.com/images/badges/made-with-python.svg)

[<img target="_blank" src="https://scikit-learn.org/stable/_static/scikit-learn-logo-small.png" width=200>](https://scikit-learn.org/stable/) [<img target="_blank" src="https://flask.palletsprojects.com/en/1.1.x/_images/flask-logo.png" width=170>](https://flask.palletsprojects.com/en/1.1.x/) [<img target="_blank" src="https://number1.co.za/wp-content/uploads/2017/10/gunicorn_logo-300x85.png" width=280>](https://gunicorn.org) [<img target="_blank" src="https://i0.wp.com/learn.onemonth.com/wp-content/uploads/2019/07/image2-1.png?w=600&ssl=1" width=200>](https://learn.onemonth.com/the-best-way-to-learn-sql/) 

[<img target="_blank" src="https://numpy.org/images/logo.svg" width=270>](https://numpy.org/)  [<img target="_blank" src="https://pandas.pydata.org/static/img/pandas.svg" width=270>](https://pandas.pydata.org/about/citing.html)

## Credits
- [Ineuron](https://ineuron.ai/) - This project wouldn't have been possible without this website. It saved my enormous amount of time . A huge shout-out to its creator [krish Naik](https://github.com/krishnaik06) and [Sudhanshu Sir]().
