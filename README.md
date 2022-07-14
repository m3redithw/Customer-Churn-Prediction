# Telco Classification Project by Meredith Wang
**Customer churn** is one of the most important metrics for a growing business to evaluate. It's easier to save an existing customer before they leave than to convice them to come back. Understanding and preventing customer churn is critial to company's **long-term success**.

In this project, we will use statistical testing to analyze the key factors of customers who are more likely to churn, develop a classification model to predict churn based on those factors, and provide recommendations for retaining customers as well as predictions of churn for a list of customers (delivered via csv).
## :chart:   Business Goals
- Find drivers for customer churn at Telco. Why are customers churning?

- Construct a ML classification model that accurately predicts customer churn.

- Deliver a report that a non-data scientist can read through and understand what steps were taken, why and what was the outcome?

## :memo:   Initial Questions/Hypothesis
- Which variables are associated with churn?

- Are average monthly charges higher for customers who churn?

- Are tenure shorter for customer who churn?

- Are additional services independent with churn?
## :open_file_folder:   Data Dictionary
**Variable** |    **Value**    | **Meaning**
---|---|---
*Contract Type* | 1) Month-to-month 2) One year 3) Two year| This indicates what type of contract the customer has
*Internet Service Type* | 1) DSL 2) Fiber Optic 3) None | This indicates what type of internet service the customer has, if any
*Payment Type* | 1) Bank transfer 2) Credit card 3) Electronic check 4) Mailed check | This tells us how is the customer paying for the service
*Monthly Charges* | Float number | This tells us how much is the customer paying each month
*Teunure* | Integer ranging from 0-72 | This shows how long (months) does the customer stay with the company
*Online Bakcup* | 1) Yes 2) No 3) No internet service | This indicates if the customer has online backup service
*Online Security* | 1) Yes 2) No 3) No internet service | This indicates if the customer has online security service
*Tech Support*| 1) Yes 2) No 3) No internet service | This indicates if the customer has tech support service
*Device Protection* | 1) Yes 2) No 3) No internet service | This indicates if the customer has device protection service
*Streaming TV* | 1) Yes 2) No 3) No internet service | This indicates if the customer has streaming tv service
*Streaming Movies* | 1) Yes 2) No 3) No internet service | This indicates if the customer has streaming movies service

## :placard:    Process
#### :one:   Data Acquisition

<details>
<summary> Gather data from mySQL database</summary>

- Create env.py file to establish connection to mySQL server

- Use **telco_churn** database in the mySQL server

- Write query to join useful tables to gather all data about the customers:  <u>customers, contract_types, payment_types, internet_service_types </u>
     ```sh
     SELECT * FROM customers JOIN contract_types USING (contract_type_id) JOIN payment_types ON customers.payment_type_id = payment_types.payment_type_id JOIN internet_service_types ON customers.internet_service_type_id = internet_service_types.internet_service_type_id
     ```
</details>

<details>
<summary> acqure.py</summary>

- Create acquire.py and user-defined function `get_telco_data()` to gather data from mySQL
     ```sh
     def get_telco_data():
     
     if os.path.isfile('telco.csv'):
        df = pd.read_csv('telco.csv', index_col=0)
    else:
        df = new_telco_data()
        df.to_csv('telco.csv')
        
    return df
    ```
- Import [acquire.py](acquire.py)

- Test acquire function

- Calling the function, and store the table in the form of dataframe
    ```sh
    df = acquire.get_telco_data()
    ```
</details>

#### :two:   Data Preparation

<details>
<summary> Data Cleaning</summary>

- **Missing values: null values are dropped** (total_charges)
     ```sh
    df['total_charges'] = df['total_charges'].str.strip()
    df = df[df.total_charges != '']
    ```
- **Data types: object is converted to the numeric datatype** (total_charges)
     ```sh
     df['total_charges'] = df.total_charges.astype(float)
     ```
- **Dummy variables: created dummy variables for binary and non-binary categorical variables**

- **Duplicate columns: duplicated columns are dropped**

- Create function `prep_telco` to clean and prepare data with steps above

- Import [prepare.py](prepare.py)

- Test prepare function

- Call the function, and store the cleaned data in the form of dataframe
</details>

<details>
<summary> Data Splitting</summary>

- Create function `split_telco_data()` to split data into **train, validate, test**

- Test prepare function

- Check the size of each dataset
     ```sh
     train.shape, validate.shape, test.shape
     ```
- Call the function, and store the 3 data samples separately in the form of dataframe
     ```sh
     train, validate, test = prepare.split_telco_data(df)
     ```
</details>

#### :three:   Exploratory Analysis
- Ask questions to find what are the key variables that are driving the churn

- Gather and sort churn rate from each driver into .xlsx file

- Import [churn_rates.xlsx](churn_rates.xlsx) and store the data in the form of datafram

- Create visualizations for the churn rate for each variable

- Explore each feature's dependency with churn and create visualization for each

#### :four:    Statistical Testing & Modeling
- Conduct T-Test for categorical variable vs. numerical variable

- Conduct Chi^2 Test for categorical variable vs. categorical variable

- Conclude hypothesis and address the initial questions
#### :five:    Modeling Evaluation
- Create decision tree classifer and fit train dataset

- Find the max depth for the best performing decision tree classifer (evaluated using classification report, accuracy score)

- Create random forest classifier and fit train dataset

- Find the max depth for the best performing random forest classifier (evaluated using classification report, accuracy score)

- Create KNN classifier and fit train dataset

- Find the k for the best performing KNN classifier (evaluated using classification report, accuracy score)

- Create logistic regression model and fit train dataset

- Find the parameter C for the best performing logistic regression model (evaluated using classification report, accuracy score)

- Pick the top 3 models among all the models and evaluate performance on validate dataset

- Pick the model with highest accuracy and evaluate on test dataset

## :repeat:   Steps to Reproduce
- [x] You will need an **env.py** file that contains the hostname, username and password of the mySQL database that contains the telco table. Store that env file locally in the repository.
- [x] Clone my repo (including the **acquire.py**, **prepare.py** and **churn_rates.xlsx**) 
- [x] Confirm **.gitignore** is hiding your env.py file
- [x] Libraries used are pandas, matplotlib, seaborn, plotly, sklearn, scipy
- [x] Follow instructions in [telco_analysis](telco_analysis.ipynb) workbook and README file
- [x] Good to run telco_report :smile_cat:

## :key:    Key Findings
<img width="800" alt="churn_drivers" src="https://user-images.githubusercontent.com/105242871/179089814-967c69e9-7b54-433b-a310-aa11de46b94f.png">
▪️ The top 4 drivers of churn are:

 - electronic payment type
 
 - sernior citizens
 
 - month-to-month contract type
 
 - fiber optic internet service type
    
    
▪️ Average **monthly charges** is higher for customers who churn
 
▪️ Average **tenure** is shorter for customers who churn
 
▪️ Additional services (device protection, online security, online backup, tech support, streaming tv, streaming movies) are dependent on churn

▪️ The meachine learning model: logistic regression classifier is expected to predict churn with **81% accuracy** on future unseen data

## :high_brightness:    Recommendations
▪️ Raise price of month-to-month contract type and offer discounts for two-year contract to lead customers towards the other two contract types

▪️ Offer discount on device protection, streaming tv and streaming movies services

▪️ Offer online security, online backup, tech support services for free for one-year and two-year contracts customers
