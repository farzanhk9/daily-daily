import os
import json
import datetime

DATA_FILE = "expenses.json"

def load_expenses():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r", encoding="utf-8") as f:
            return json.load(f)
    return []

def save_expenses(expenses):
    with open(DATA_FILE, "w", encoding="utf-8") as f:
        json.dump(expenses, f, indent=4)

def add_expense():
    category = input("Category (Food, Transport, Shopping, etc.): ")
    try:
        amount = float(input("Amount spent: "))
    except ValueError:
        print("⚠️ Invalid amount.")
        return
    date = str(datetime.date.today())
    
    expenses.append({"date": date, "category": category, "amount": amount})
    save_expenses(expenses)
    print("✅ Expense added successfully!")

def view_expenses():
    if not expenses:
        print("📭 No expenses recorded yet.")
        return
    print("\n📋 Your expenses:")
    total = 0
    for exp in expenses:
        print(f"{exp['date']} — {exp['category']}: ${exp['amount']}")
        total += exp["amount"]
    print(f"\n💵 Total spent: ${total}")

def category_summary():
    if not expenses:
        print("📭 No expenses recorded yet.")
        return
    summary = {}
    for exp in expenses:
        summary[exp["category"]] = summary.get(exp["category"], 0) + exp["amount"]
    print("\n📊 Spending by category:")
    for cat, amt in summary.items():
        print(f"{cat}: ${amt}")

def menu():
    while True:
        print("\n==== DAILY EXPENSE TRACKER ====")
        print("1. Add expense")
        print("2. View all expenses")
        print("3. View category summary")
        print("4. Exit")
        choice = input("Choose an option: ")
        
        if choice == "1":
            add_expense()
        elif choice == "2":
            view_expenses()
        elif choice == "3":
            category_summary()
        elif choice == "4":
            break
        else:
            print("⚠️ Invalid choice. Try again.")

expenses = load_expenses()
menu()

