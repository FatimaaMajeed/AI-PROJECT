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

# Print the first 5 rows of the 'users' table (or any table you query)
    print("First 5 rows from 'users_table' table:")
    for row in rows:
        print(row)
    
    # Close the cursor and connection
    cursor.close()
    connection.close()
else:
    print("Failed to connect to MySQL server.")

# Step 1: Define a simple medicine database
medicine_database = {
    "paracetamol": ["fever", "headache", "body pain"],
    "ibuprofen": ["inflammation", "pain", "fever"],
    "amoxicillin": ["infection", "bacterial infection"],
    "cetirizine": ["allergy", "runny nose", "sneezing","cough"],
    "aspirin": ["headache", "fever", "pain"],
    "loratadine": ["allergy", "itching", "rash"],
    "metformin": ["diabetes"],
    "omeprazole": ["acid reflux", "stomach ulcer"],
}

# Step 2: Define a function to recommend medicine based on user symptoms
def recommend_medicine(symptoms):
    recommended_medicines = []
    for medicine, conditions in medicine_database.items():
        if any(symptom in conditions for symptom in symptoms):
            recommended_medicines.append(medicine)

    if recommended_medicines:
        return recommended_medicines


        else:
        return ["No medicine found for the given symptoms. Please consult a doctor."]

# Step 3: Main function to run the recommendation system
# GUI function to collect user symptoms and display recommendations
def create_gui():
    def get_recommendations():
        symptoms_input = symptoms_entry.get()
        if not symptoms_input.strip():
            messagebox.showwarning("Input Error", "Please enter symptoms.")
            return

        symptoms = [symptom.strip().lower() for symptom in symptoms_input.split(",")]
        recommendations = recommend_medicine(symptoms)

        # Display recommendations in the output area
        output_text.delete("1.0", tk.END)
        if recommendations:
            output_text.insert(tk.END, "Recommended Medicines:\n")
            for medicine in recommendations:
                output_text.insert(tk.END, f"- {medicine}\n")
        else:
            output_text.insert(tk.END, "No recommendations available.")

    # Create the main application window
    root = tk.Tk()
    root.title("Medicine Recommendation System")
    root.geometry("500x400")

    # Title label
    title_label = tk.Label(root, text="Medicine Recommendation System", font=("Arial", 16, "bold"))
    title_label.pack(pady=10)


    # Symptoms input
    symptoms_label = tk.Label(root, text="Enter your symptoms (comma-separated):", font=("Arial", 12))
    symptoms_label.pack(pady=5)

    symptoms_entry = tk.Entry(root, width=50, font=("Arial", 12))
    symptoms_entry.pack(pady=5)

    # Recommend button
    recommend_button = tk.Button(root, text="Get Recommendations", font=("Arial", 12), command=get_recommendations)
    recommend_button.pack(pady=10)

    # Output area
    output_label = tk.Label(root, text="Recommendations:", font=("Arial", 12))
    output_label.pack(pady=5)

    output_text = tk.Text(root, width=60, height=10, font=("Arial", 12), wrap=tk.WORD)
    output_text.pack(pady=5)

    # Start the GUI event loop
    root.mainloop()

# Main function to run the GUI
if _name_ == "_main_":
    create_gui()
