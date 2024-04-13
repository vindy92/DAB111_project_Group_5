# DAB111_project_Group_5

## Customer Data Project

### Overview
The Customer Data Project involves importing customer data from a CSV file into a Pandas DataFrame. 
This data is then sent to a SQLite database and subsequently read back from the database into a DataFrame. 
Additionally, the project includes setting up a Flask web application to display information about the dataset.
Data set collected legally from Kaggle.com . The dataset has 5 variables with numerical and categorical variables.

### Project Structure
##### The project is structured as follows:

- Data Collection: Simulated data is collected and stored in a CSV file named customer_data.csv.
- Database: The data is then stored in a SQLite database named customer_data.db.
- Website: A Flask-based website is developed to serve the data from the database.
- README.md: This file provides an overview of the project and instructions on how to set it up.
- requirements.txt: Lists all the Python packages necessary to run the project.

### Code Snippets
The project includes code snippets demonstrating key functionalities:

- Establishing a connection to the SQLite database
```
import sqlite3
conn = sqlite3.connect("customer_data.db")
cur = conn.cursor()
```
- Reading and displaying data from a CSV file into a Pandas DataFrame
```
import pandas as pd
df = pd.read_csv("customer_data.csv")
print(df.head())
```
- Sending data to the database
```
df.to_sql("customer_data", conn, index=False, if_exists="append")
```
- Reading data from the database into a DataFrame
```
cur.execute("SELECT * FROM customer_data")
rows = cur.fetchall()
print(rows)
```
- Read from Database to DataFrame (Limiting Rows)
```
result = pd.read_sql_query("SELECT * FROM customer_data LIMIT 15", conn)
print(result)
```
- Implementing a Flask application for web presentation
```
from flask import Flask, render_template
import sqlite3
import pathlib

base_path = pathlib.Path(r'C:\Users\vindy\OneDrive\Documents\Python\PROJECT')
db_name = "customer_data.db"

db_path = base_path / db_name
print(db_path)

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/about')
def about():
    return render_template('about.html')

@app.route('/data')
def data():
    con = sqlite3.connect(db_path)
    cursor = con.cursor()
    customer_data = cursor.execute("SELECT * FROM customer_data LIMIT 15").fetchall()
    con.close()

    return render_template('data_table.html', customer_data=customer_data)

if __name__ == '__main__':
    app.run(debug=True)
```
