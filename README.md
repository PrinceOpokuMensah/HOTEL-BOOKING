#include <iostream>
#include <fstream>
#include <vector>
#include <string>
using namespace std;

// Book class to store book details
class Book {
public:
    string title;
    string author;
    int id;
    bool isIssued;

    Book(string t, string a, int i) : title(t), author(a), id(i), isIssued(false) {}
};

// Library Management System class
class Library {
private:
    vector<Book> books;

public:
    void addBook(string title, string author, int id) {
        books.push_back(Book(title, author, id));
        cout << "Book added successfully!\n";
    }

    void displayBooks() {
        for (const Book& book : books) {
            cout << "ID: " << book.id << ", Title: " << book.title 
                 << ", Author: " << book.author << ", Issued: " << (book.isIssued ? "Yes" : "No") << endl;
        }
    }

    void searchBook(int id) {
        for (const Book& book : books) {
            if (book.id == id) {
                cout << "Found - Title: " << book.title << ", Author: " << book.author 
                     << ", Issued: " << (book.isIssued ? "Yes" : "No") << endl;
                return;
            }
        }
        cout << "Book not found!\n";
    }

    void issueBook(int id) {
        for (Book& book : books) {
            if (book.id == id) {
                if (!book.isIssued) {
                    book.isIssued = true;
                    cout << "Book issued successfully!\n";
                } else {
                    cout << "Book is already issued!\n";
                }
                return;
            }
        }
        cout << "Book not found!\n";
    }

    void returnBook(int id) {
        for (Book& book : books) {
            if (book.id == id) {
                if (book.isIssued) {
                    book.isIssued = false;
                    cout << "Book returned successfully!\n";
                } else {
                    cout << "Book was not issued!\n";
                }
                return;
            }
        }
        cout << "Book not found!\n";
    }

    void saveToFile() {
        ofstream file("library_records.txt");
        if (file.is_open()) {
            for (const Book& book : books) {
                file << book.id << "," << book.title << "," << book.author << "," 
                     << book.isIssued << endl;
            }
            file.close();
            cout << "Records saved to file!\n";
        } else {
            cout << "Unable to open file!\n";
        }
    }
};

int main() {
    Library library;
    int choice, id;
    string title, author;

    while (true) {
        cout << "\n1. Add Book\n2. Display Books\n3. Search Book\n4. Issue Book\n5. Return Book\n6. Save to File\n7. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter Title: ";
                cin.ignore();
                getline(cin, title);
                cout << "Enter Author: ";
                getline(cin, author);
                cout << "Enter ID: ";
                cin >> id;
                library.addBook(title, author, id);
                break;
            case 2:
                library.displayBooks();
                break;
            case 3:
                cout << "Enter ID to search: ";
                cin >> id;
                library.searchBook(id);
                break;
            case 4:
                cout << "Enter ID to issue: ";
                cin >> id;
                library.issueBook(id);
                break;
            case 5:
                cout << "Enter ID to return: ";
                cin >> id;
                library.returnBook(id);
                break;
            case 6:
                library.saveToFile();
                break;
            case 7:
                return 0;
            default:
                cout << "Invalid choice!\n";
        }
    }

return 0;
}
