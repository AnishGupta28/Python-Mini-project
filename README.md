# Python-Mini-project
FILE_NAME = "expenses.txt"


def add_expense(expenses):
    print("\n--- Add Expense ---")
    date = input("Enter date (DD-MM-YYYY): ")
    category = input("Enter category (Food/Travel/Books/Other): ")
    description = input("Enter description: ")

    try:
        amount = float(input("Enter amount (Rs.): "))
        if amount <= 0:
            print("Amount must be greater than zero.")
            return
    except ValueError:
        print("Please enter a valid amount.")
        return

    expense = {
        "date": date,
        "category": category,
        "description": description,
        "amount": amount
    }

    expenses.append(expense)
    save_expenses(expenses)
    print("Expense added successfully!")


def view_expenses(expenses):
    print("\n--- All Expenses ---")

    if len(expenses) == 0:
        print("No expenses recorded yet.")
        return

    for i, expense in enumerate(expenses, start=1):
        print(f"\nExpense {i}")
        print("Date:", expense["date"])
        print("Category:", expense["category"])
        print("Description:", expense["description"])
        print("Amount: Rs.", expense["amount"])


def show_total(expenses):
    total = 0

    for expense in expenses:
        total += expense["amount"]

    print("\nTotal expenses: Rs.", round(total, 2))


def save_expenses(expenses):
    file = open(FILE_NAME, "w")

    for expense in expenses:
        line = (
            expense["date"] + "|" +
            expense["category"] + "|" +
            expense["description"].replace("|", "/") + "|" +
            str(expense["amount"]) + "\n"
        )
        file.write(line)

    file.close()


def load_expenses():
    expenses = []

    try:
        file = open(FILE_NAME, "r")

        for line in file:
            parts = line.strip().split("|")

            if len(parts) == 4:
                expense = {
                    "date": parts[0],
                    "category": parts[1],
                    "description": parts[2],
                    "amount": float(parts[3])
                }
                expenses.append(expense)

        file.close()

    except FileNotFoundError:
        pass

    return expenses


def main():
    expenses = load_expenses()

    while True:
        print("\n========== STUDENT EXPENSE MANAGER ==========")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Calculate Total Expenses")
        print("4. Exit")

        choice = input("Enter your choice (1-4): ")

        if choice == "1":
            add_expense(expenses)
        elif choice == "2":
            view_expenses(expenses)
        elif choice == "3":
            show_total(expenses)
        elif choice == "4":
            print("Thank you for using Student Expense Manager!")
            break
        else:
            print("Invalid choice. Please try again.")


main()
