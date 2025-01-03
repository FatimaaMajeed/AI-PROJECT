# AI-PROJECT
import mysql.connector
import tkinter as tk
from tkinter import messagebox

# Establish the connection to the MySQL server
connection = mysql.connector.connect(
    host="localhost",         # The host where your MySQL server is running (localhost for local machine)
    user="root",              # Your MySQL username (usually 'root' for local installations)
    password="",  
    database="medicine recommendation system"  # The name of your database
)

# Check if the connection is successful
if connection.is_connected():
    print("Connected to MySQL server successfully.")
    
    # Create a cursor object to interact with the database
    cursor = connection.cursor()
    
    # Example: Query the structure of a table (e.g., 'users' table)
    cursor.execute("SHOW TABLES;")  # Show all tables in the database
    tables = cursor.fetchall()
    
    print("Tables in the database:")
    for table in tables:
        print(table[0])  # Table names are in the first column of the result
    
    # Example: Query the 'users' table (replace with actual table name)
    cursor.execute("SELECT * FROM users_table LIMIT 5;")  # Adjust the table name as necessary
    rows = cursor.fetchall()
