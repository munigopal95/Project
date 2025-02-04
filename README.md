# Project
#Library Management Project
import mysql.connector

# Define the Book class
class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn

    def add_book(self):
        print(f"Book '{self.title}' by {self.author} (ISBN: {self.isbn}) added.")

    @staticmethod
    def search_book(title):
        print(f"Searching for book with title: {title}")
        return [f"Book Found: {title}"]

# Define the User class
class User:
    def __init__(self, name):
        self.name = name

    def add_user(self):
        print(f"User '{self.name}' added.")

# Define the Library class
class Library:
    @staticmethod
    def borrow_book(user_id, book_id, borrow_date, due_date):
        print(f"User {user_id} borrowed book {book_id} on {borrow_date}, due on {due_date}.")

    @staticmethod
    def return_book(borrow_id, return_date):
        print(f"Book with borrow ID {borrow_id} returned on {return_date}.")

# Define the Report class
class Report:
    @staticmethod
    def overdue_books():
        print("Generating report for overdue books.")

# Function to insert employee records into the database
def recordinsert():
    while True:
        try:
            con = mysql.connector.connect(host="127.0.0.1",
                                          user="root",
                                          passwd="muni",
                                          database="exam")
            cur = con.cursor()

            print("*" * 50)
            print("Enter employee details:")

            emp_id = input("Enter Employee ID: ")
            emp_name = input("Enter Employee Name: ")
            emp_age = input("Enter Employee Age: ")
            emp_department = input("Enter Employee Department: ")

            query = "INSERT INTO Emp (emp_id, emp_name, emp_age, emp_department) VALUES (%s, %s, %s, %s)"
            cur.execute(query, (emp_id, emp_name, emp_age, emp_department))

            con.commit()
            print("Employee record inserted successfully!")

            cont = input("Do you want to add another record? (yes/no): ").lower()
            if cont != 'yes':
                break

        except mysql.connector.Error as err:
            print(f"Error: {err}")
        finally:
            if con.is_connected():
                cur.close()
                con.close()

# Main function for the library system
def main():
    while True:
        print("\nLibrary Management System")
        print("1. Add Book")
        print("2. Search Book")
        print("3. Add User")
        print("4. Borrow Book")
        print("5. Return Book")
        print("6. Overdue Books Report")
        print("7. Add Employee Record")  # Option for employee record insertion
        print("8. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            title = input("Enter book title: ")
            author = input("Enter book author: ")
            isbn = input("Enter book ISBN: ")
            book = Book(title, author, isbn)
            book.add_book()
            print("Book added!")

        elif choice == '2':
            title = input("Enter book title to search: ")
            books = Book.search_book(title)
            if books:
                for book in books:
                    print(book)
            else:
                print("No books found.")

        elif choice == '3':
            name = input("Enter user name: ")
            user = User(name)
            user.add_user()
            print("User added!")

        elif choice == '4':
            user_id = int(input("Enter user ID: "))
            book_id = int(input("Enter book ID: "))
            borrow_date = input("Enter borrow date (YYYY-MM-DD): ")
            due_date = input("Enter due date (YYYY-MM-DD): ")
            Library.borrow_book(user_id, book_id, borrow_date, due_date)
            print("Book borrowed!")

        elif choice == '5':
            borrow_id = int(input("Enter borrow ID: "))
            return_date = input("Enter return date (YYYY-MM-DD): ")
            Library.return_book(borrow_id, return_date)
            print("Book returned!")

        elif choice == '6':
            Report.overdue_books()

        elif choice == '7':
            recordinsert()  # Call the employee record insertion function

        elif choice == '8':
            print("Exiting...")
            break


if __name__ == '__main__':
    main()
