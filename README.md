# Project


import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Book class represents a book in the library
class Book {
    private String title;
    private String author;    
    private String isbn;      //International standard bood number .
    private boolean isIssued;

    public Book(String title, String author, String isbn) {
        this.title = title;
        this.author = author;
        this.isbn = isbn;
        this.isIssued = false;
    }

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public String getIsbn() {
        return isbn;
    }   

    public boolean isIssued() {
        return isIssued;
    }

    public void setIssued(boolean issued) {
        isIssued = issued;
    }

    @Override
    public String toString() {
        return "Book{" +
                "title='" + title + '\'' +
                ", author='" + author + '\'' +
                ", isbn='" + isbn + '\'' +
                ", isIssued=" + isIssued +
                '}';
    }
}

// Abstract User class represents a user of the library
abstract class User {
    protected String name;
    protected String userId;

    public User(String name, String userId) {
        this.name = name;
        this.userId = userId;
    }

    public String getName() {
        return name;
    }

    public String getUserId() {
        return userId;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", userId='" + userId + '\'' +
                '}';
    }
}

// Member class represents a library member
class Member extends User {
    private List<Book> borrowedBooks;

    public Member(String name, String userId) {
        super(name, userId);
        this.borrowedBooks = new ArrayList<>();
    }

    public List<Book> getBorrowedBooks() {
        return borrowedBooks;
    }

    public void borrowBook(Book book) {
        if (book.isIssued()) {
            System.out.println("The book is already issued.");
        } else {
            borrowedBooks.add(book);
            book.setIssued(true);
            System.out.println("Book borrowed: " + book.getTitle());
        }
    }

    public void returnBook(Book book) {
        if (borrowedBooks.remove(book)) {
            book.setIssued(false);
            System.out.println("Book returned: " + book.getTitle());
        } else {
            System.out.println("This book was not borrowed by this member.");
        }
    }

    @Override
    public String toString() {
        return "Member{" +
                "name='" + name + '\'' +
                ", userId='" + userId + '\'' +
                ", borrowedBooks=" + borrowedBooks +
                '}';
    }
}

// Librarian class represents a librarian who manages books
class Librarian extends User {

    public Librarian(String name, String userId) {
        super(name, userId);
    }

    public void addBook(Library library, Book book) {
        library.addBook(book);
        System.out.println("Book added: " + book.getTitle());
    }

    public void removeBook(Library library, Book book) {
        if (library.removeBook(book)) {
            System.out.println("Book removed: " + book.getTitle());
        } else {
            System.out.println("Book not found in the library.");
        }
    }

    public void listAllBooks(Library library) {
        List<Book> books = library.getBooks();
        System.out.println("Listing all books:");
        for (Book book : books) {
            System.out.println(book);
        }
    }
}

// Library class manages collections of books and users
class Library {
    private List<Book> books;
    private List<User> users;

    public Library() {
        this.books = new ArrayList<>();
        this.users = new ArrayList<>();
    }

    public List<Book> getBooks() {
        return books;
    }

    public void addBook(Book book) {
        books.add(book);
    }

    public boolean removeBook(Book book) {
        return books.remove(book);
    }

    public void addUser(User user) {
        users.add(user);
    }

    public List<User> getUsers() {
        return users;
    }

    public Book findBookByTitle(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                return book;
            }
        }
        return null;
    }

    public User findUserById(String userId) {
        for (User user : users) {
            if (user.getUserId().equalsIgnoreCase(userId)) {
                return user;
            }
        }
        return null;
    }
}

// Main class for the Library Management System
public class LibraryManagementSystem {

    public static void main(String[] args) {
        Library library = new Library();
        Librarian librarian = new Librarian("Alice", "lib123");
        Member member = new Member("Bob", "mem456");

        library.addUser(librarian);
        library.addUser(member);

        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("\nLibrary Management System");
            System.out.println("1. Add Book");
            System.out.println("2. Remove Book");
            System.out.println("3. List All Books");
            System.out.println("4. Borrow Book");
            System.out.println("5. Return Book");
            System.out.println("6. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter book title: ");
                    String title = scanner.nextLine();
                    System.out.print("Enter book author: ");
                    String author = scanner.nextLine();
                    System.out.print("Enter book ISBN: ");
                    String isbn = scanner.nextLine();
                    Book book = new Book(title, author, isbn);
                    librarian.addBook(library, book);
                    break;

                case 2:
                    System.out.print("Enter book title to remove: ");
                    String removeTitle = scanner.nextLine();
                    Book removeBook = library.findBookByTitle(removeTitle);
                    if (removeBook != null) {
                        librarian.removeBook(library, removeBook);
                    } else {
                        System.out.println("Book not found.");
                    }
                    break;

                case 3:
                    librarian.listAllBooks(library);
                    break;

                case 4:
                    System.out.print("Enter book title to borrow: ");
                    String borrowTitle = scanner.nextLine();
                    Book borrowBook = library.findBookByTitle(borrowTitle);
                    if (borrowBook != null) {
                        member.borrowBook(borrowBook);
                    } else {
                        System.out.println("Book not found.");
                    }
                    break;

                case 5:
                    System.out.print("Enter book title to return: ");
                    String returnTitle = scanner.nextLine();
                    Book returnBook = library.findBookByTitle(returnTitle);
                    if (returnBook != null) {
                        member.returnBook(returnBook);
                    } else {
                        System.out.println("Book not found.");
                    }
                    break;

                case 6:
                    System.out.println("Exiting...");
                    scanner.close();
                    System.exit(0);

                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
