# Expense-Tracker
import csv
import os
import pandas as pd
from datetime import datetime

FILE_NAME = "expenses.csv"

if not os.path.exists(FILE_NAME):
    with open(FILE_NAME, "w", newline="") as file:
        writer = csv.writer(file)
        writer.writerow(["Date", "Amount", "Category", "Description"])

def add_expense():
    date = datetime.today().strftime('%Y-%m-%d')
    try:
        amount = float(input("Enter amount spent: "))
    except ValueError:
        print("Invalid amount!")
        return
    category = input("Enter category: ").strip()
    description = input("Enter description: ").strip()

    with open(FILE_NAME, "a", newline="") as file:
        writer = csv.writer(file)
        writer.writerow([date, amount, category, description])

    print("Expense added successfully!")

def view_expenses():
    try:
        df = pd.read_csv(FILE_NAME)
        print(df.to_string(index=False) if not df.empty else "No expenses recorded.")
    except Exception as e:
        print("Error:", e)

def monthly_summary():
    try:
        df = pd.read_csv(FILE_NAME)
        df["Date"] = pd.to_datetime(df["Date"])
        df["Month"] = df["Date"].dt.strftime("%Y-%m")
        month = input("Enter month (YYYY-MM): ")
        total = df[df["Month"] == month]["Amount"].sum()
        print(f"Total expenses for {month}: ₹{total}" if total > 0 else "No expenses found.")
    except Exception as e:
        print("Error:", e)

def category_summary():
    try:
        df = pd.read_csv(FILE_NAME)
        category_totals = df.groupby("Category")["Amount"].sum()
        print(category_totals.to_string() if not category_totals.empty else "No expense data available.")
    except Exception as e:
        print("Error:", e)

while True:
    print("\n1. Add Expense\n2. View Expenses\n3. Monthly Summary\n4. Category-wise Summary\n5. Exit")
    choice = input("Enter your choice: ").strip()

    if choice == "1":
        add_expense()
    elif choice == "2":
        view_expenses()
    elif choice == "3":
        monthly_summary()
    elif choice == "4":
        category_summary()
    elif choice == "5":
        break
    else:
        print("Invalid choice.")
