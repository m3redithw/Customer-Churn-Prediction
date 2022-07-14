# Telco Classification Project by Meredith Wang
**Customer churn** is one of the most important metrics for a growing business to evaluate. It's easier to save an existing customer before they leave than to convice them to come back. Understanding and preventing customer churn is critial to company's **long-term success**.

In this project, we will use statistical testing to analyze the key factors of customers who are more likely to churn, develop a classification model to predict churn based on those factors, and provide recommendations for retaining customers as well as predictions of churn for a list of customers (delivered via csv).
## :chart:   Business Goals
- Find drivers for customer churn at Telco. Why are customers churning?

- Construct a ML classification model that accurately predicts customer churn.

- Deliver a report that a non-data scientist can read through and understand what steps were taken, why and what was the outcome?

## :memo:   Initial Hypothesis

## :open_file_folder:   Data Dictionary
**Variable** |    **Value**    | **Meaning**
---|---|---
*Contract Type*</span> | 1) Month-to-month 2) One year 3) Two year| This indicates what type of contract the customer has
*Internet Service Type*</span> | 1) DSL 2) Fiber Optic 3) None | This indicates what type of internet service the customer has, if any
*Payment Type*</span> | 1) Bank transfer 2) Credit card 3) Electronic check 4) Mailed check | This tells us how is the customer paying for the service
*Monthly Charges*</span>| Float number | This tells us how much is the customer paying each month
*Teunure*</span> | Integer ranging from 0-72 | This shows how long (months) does the customer stay with the company

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
- Ask questions to find what are driving the churn

- Create visualizations for each question

#### :four:    Statistical Testing & Modeling
- 
#### :five:    Modeling Evaluation

## :repeat:   Steps to Reproduce
- [x] You will need an **env.py** file that contains the hostname, username and password of the mySQL database that contains the telco table. Store that env file locally in the repository.
- [x] Clone my repo (including the **acquire.py** and **prepare.py**) 
- [x] Confirm **.gitignore** is hiding your env.py file
- [x] Libraries used are pandas, matplotlib, seaborn, sklearn, scipy
- [ ] Good to run telco_report :smile_cat:

## :key:    Key Findings

## :high_brightness:    Recommendations
