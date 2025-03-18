# NumPy, Pandas, and SQLAlchemy: A Detailed Guide

## Overview

This guide provides a comprehensive understanding of three essential Python libraries:

- **NumPy** for numerical computing.
- **Pandas** for data manipulation and analysis.
- **SQLAlchemy** for handling database interactions.

---

## 1. NumPy Operations

### 1.1 Introduction to NumPy

[NumPy](https://numpy.org/) is a powerful library for numerical processing in Python. It supports high-performance multi-dimensional arrays and a variety of mathematical functions.

#### Installing NumPy

```sh
pip install numpy
```

#### Importing NumPy

```python
import numpy as np
```

---

### 1.2 Creating Arrays

NumPy arrays offer a performance advantage over Python lists.

#### One-Dimensional Array

```python
arr = np.array([1, 2, 3, 4, 5])
print(arr)
```

#### Two-Dimensional Array

```python
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
print(arr_2d)
```

---

### 1.3 Special Arrays

NumPy provides functions to generate arrays with predefined values.

```python
arr_random = np.random.rand(3, 4)  # Random values between 0 and 1
zeros_array = np.zeros((3, 4))  # Matrix filled with zeros
ones_array = np.ones((3, 4))  # Matrix filled with ones
full_array = np.full((3, 4), 10)  # Matrix filled with 10
```

---

### 1.4 Generating Sequences

```python
arr_range = np.arange(10, 20, 0.5)
print(arr_range)
```

`np.arange(start, stop, step)`: Creates a sequence with a defined step size.

---

### 1.5 Matrix Operations

```python
matrix_1 = np.array([[1, 2, 3], [4, 5, 6]])
matrix_2 = np.array([[1, 2, 3], [4, 5, 6]])

matrix_addition = matrix_1 + matrix_2
matrix_subtraction = matrix_1 - matrix_2
matrix_multiplication = matrix_1 * matrix_2
matrix_division = matrix_1 / matrix_2
matrix_power = matrix_1 ** matrix_2
matrix_transpose = matrix_1.T
```

---

### 1.6 Statistical Functions

```python
arr_mean = np.mean(arr)
arr_median = np.median(arr)
arr_std = np.std(arr)
arr_var = np.var(arr)
```

---

## 2. Pandas Operations

### 2.1 Introduction to Pandas

[Pandas](https://pandas.pydata.org/) is a powerful library for handling structured data efficiently.

#### Installing Pandas

```sh
pip install pandas
```

#### Importing Pandas

```python
import pandas as pd
```

---

### 2.2 Creating a Pandas Series

```python
series = pd.Series([1, 2, 3, 4, 5])
series_custom_index = pd.Series([1, 2, 3, 4, 5], index=['a', 'b', 'c', 'd', 'e'])
```

---

### 2.3 Creating a DataFrame

```python
data = {
    'Name': ['John', 'Jane', 'Jim', 'Jill'],
    'Age': [20, 21, 22, 23],
    'City': ['New York', 'Los Angeles', 'Chicago', 'Houston']
}
df = pd.DataFrame(data)
print(df)
```

---

### 2.4 Reading a CSV File

```python
df_csv = pd.read_csv('data.csv')
print(df_csv)
```

---

## 3. SQLAlchemy Operations

### 3.1 Introduction to SQLAlchemy

[SQLAlchemy](https://www.sqlalchemy.org/) is a robust toolkit for managing databases in Python.

#### Installing SQLAlchemy

```sh
pip install sqlalchemy
```

#### Importing SQLAlchemy

```python
from sqlalchemy import create_engine, text, MetaData, Table, Column, Integer, String, ForeignKey, insert, select
```

---

### 3.2 Creating a SQLite Database

```python
import os
print(os.getcwd())  # Get current working directory

engine = create_engine('sqlite:///basic_module/src/Datastructures/data.db', echo=True)
```

---

### 3.3 Handling Database Permission Issues

```sh
chmod 755 /basic_module/src/Datastructures/data.db
```

---

### 3.4 Executing SQLite Commands

To open the database in the terminal:

```sh
sqlite3 /basic_module/src/Datastructures/data.db
```

List tables:

```sh
.tables
```

---

### 3.5 Defining a Database Schema

```python
metadata = MetaData()

users = Table('users', metadata,
    Column('id', Integer, primary_key=True),
    Column('name', String),
    Column('age', Integer),
    Column('city', String),
)

posts = Table('posts', metadata,
    Column('id', Integer, primary_key=True),
    Column('title', String),
    Column('content', String),
    Column('user_id', Integer, ForeignKey('users.id')),
)

metadata.create_all(engine)
```

---

## Conclusion

This guide has covered essential operations in **NumPy**, **Pandas**, and **SQLAlchemy**:

- **NumPy**: Optimized numerical computations.
- **Pandas**: Flexible and powerful data analysis tools.
- **SQLAlchemy**: Efficient database management in Python.

These libraries are crucial for data science, analytics, and database handling, making Python a versatile language for handling large datasets and databases.

