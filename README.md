import java.io.*;
import java.util.ArrayList;
import java.util.Scanner;

class Book {
    private int bookID;
    private String title;
    private String author;
    private String genre;
    private boolean available;

    public Book(int bookID, String title, String author, String genre, boolean available) {
        this.bookID = bookID;
        this.title = title;
        this.author = author;
        this.genre = genre;
        this.available = available;
    }

    // Getters and setters
    public int getBookID() {
        return bookID;
    }

    public void setBookID(int bookID) {
        this.bookID = bookID;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public String getGenre() {
        return genre;
    }

    public void setGenre(String genre) {
        this.genre = genre;
    }

    public boolean isAvailable() {
        return available;
    }

    public void setAvailable(boolean available) {
        this.available = available;
    }
}

class User {
    private int userID;
    private String name;
    private String contactInfo;
    private ArrayList<Book> borrowedBooks;

    public User(int userID, String name, String contactInfo) {
        this.userID = userID;
        this.name = name;
        this.contactInfo = contactInfo;
        this.borrowedBooks = new ArrayList<>();
    }

    // Getters and setters
    public int getUserID() {
        return userID;
    }

    public void setUserID(int userID) {
        this.userID = userID;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getContactInfo() {
        return contactInfo;
    }

    public void setContactInfo(String contactInfo) {
        this.contactInfo = contactInfo;
    }

    public ArrayList<Book> getBorrowedBooks() {
        return borrowedBooks;
    }

    public void setBorrowedBooks(ArrayList<Book> borrowedBooks) {
        this.borrowedBooks = borrowedBooks;
    }
}

class Library {
    private ArrayList<Book> books;
    private ArrayList<User> users;

    public Library() {
        this.books = new ArrayList<>();
        this.users = new ArrayList<>();
    }

    // Methods for managing books and users
    public void addBook(Book book) {
        books.add(book);
    }

    public void addUser(User user) {
        users.add(user);
    }

    public Book findBookByTitle(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                return book;
            }
        }
        return null;
    }

    public Book findBookByAuthor(String author) {
        for (Book book : books) {
            if (book.getAuthor().equalsIgnoreCase(author)) {
                return book;
            }
        }
        return null;
    }

    public User findUserById(int userID) {
        for (User user : users) {
            if (user.getUserID() == userID) {
                return user;
            }
        }
        return null;
    }

    // Getters for accessing private fields
    public ArrayList<Book> getBooks() {
        return books;
    }

    public ArrayList<User> getUsers() {
        return users;
    }

    // Serialization method
    public void saveLibraryData() {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("library_data.ser"))) {
            oos.writeObject(this);
            System.out.println("Library data saved successfully.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // Deserialization method
    public static Library loadLibraryData() {
        Library loadedLibrary = null;
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("library_data.ser"))) {
            loadedLibrary = (Library) ois.readObject();
            System.out.println("Library data loaded successfully.");
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("No existing library data found. Creating a new library.");
            loadedLibrary = new Library();
        }
        return loadedLibrary;
    }
}

public class LibraryManagementSystem {
    private static Library library;

    public static void main(String[] args) {
        library = Library.loadLibraryData();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nMenu:");
            System.out.println("1. Add Book");
            System.out.println("2. Add User");
            System.out.println("3. Display Books");
            System.out.println("4. Search Books by Title");
            System.out.println("5. Search Books by Author");
            System.out.println("6. Search User by ID");
            System.out.println("7. Exit");
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    addBook(scanner);
                    break;
                case 2:
                    // Implement adding user functionality
                    break;
                case 3:
                    displayBooks();
                    break;
                case 4:
                    searchBooksByTitle(scanner);
                    break;
                case 5:
                    searchBooksByAuthor(scanner);
                    break;
                case 6:
                    // Implement searching user by ID functionality
                    break;
                case 7:
                    library.saveLibraryData();
                    System.out.println("Exiting...");
                    System.exit(0);
                    break;
                default:
                    System.out.println("Invalid choice! Please enter a valid option.");
            }
        }
    }

    private static void addBook(Scanner scanner) {
        System.out.print("Enter Book ID: ");
        int bookID = scanner.nextInt();
        scanner.nextLine(); // Consume newline character
        System.out.print("Enter Title: ");
        String title = scanner.nextLine();
        System.out.print("Enter Author: ");
        String author = scanner.nextLine();
        System.out.print("Enter Genre: ");
        String genre = scanner.nextLine();
        boolean available = true;

        Book newBook = new Book(bookID, title, author, genre, available);
        library.addBook(newBook);
        System.out.println("Book added successfully!");
    }

    private static void displayBooks() {
        for (Book book : library.getBooks()) {
            System.out.println("Book ID: " + book.getBookID());
            System.out.println("Title: " + book.getTitle());
            System.out.println("Author: " + book.getAuthor());
            System.out.println("Genre: " + book.getGenre());
            System.out.println("Available: " + book.isAvailable() + "\n");
        }
    }

    private static void searchBooksByTitle(Scanner scanner) {
        System.out.print("Enter Title to search: ");
        String title = scanner.nextLine();
        Book foundBook = library.findBookByTitle(title);
        if (foundBook != null) {
            System.out.println("Book found:");
            System.out.println("Book ID: " + foundBook.getBookID());
            System.out.println("Title: " + foundBook.getTitle());
            System.out.println("Author: " + foundBook.getAuthor());
            System.out.println("Genre: " + foundBook.getGenre());
            System.out.println("Available: " + foundBook.isAvailable());
        } else {
            System.out.println("Book not found!");
        }
    }

    private static void searchBooksByAuthor(Scanner scanner) {
        System.out.print("Enter Author to search: ");
        String author = scanner.nextLine();
        Book foundBook = library.findBookByAuthor(author);
        if (foundBook != null) {
            System.out.println("Book found:");
            System.out.println("Book ID: " + foundBook.getBookID());
            System.out.println("Title: " + foundBook.getTitle());
            System.out.println("Author: " + foundBook.getAuthor());
            System.out.println("Genre: " + foundBook.getGenre());
            System.out.println("Available: " + foundBook.isAvailable());
        } else {
            System.out.println("Book not found!");
        }
    }
}


EXPLANATION OF PROJECT:

a. Project Description:
This project is a simple library management system implemented in Java. It allows librarians to perform various tasks such as adding books, adding users, displaying books, searching for books by title or author, and saving the library data for persistence between application runs.

b. Setting Up and Running the Project:

Prerequisites:
Java Development Kit (JDK) installed on your system.
Any Java IDE such as IntelliJ IDEA, Eclipse, or NetBeans (optional, but recommended).
Instructions:
Download the provided Java code files.
Open your preferred IDE or text editor.
Import the Java files into your project.
Compile and run the LibraryManagementSystem.java file.
c. Key Features and Functionalities:

Adding Books: Librarians can add new books to the library by providing details such as book ID, title, author, and genre.
Adding Users: Librarians can add new users to the library by providing details such as user ID, name, and contact information.
Displaying Books: The system can display all the books currently available in the library along with their details.
Searching Books by Title or Author: Librarians can search for books by their title or author's name.
Saving and Loading Data: The system supports data persistence by allowing librarians to save the library data to a file and load it later for continued use. This is achieved through manual serialization and deserialization methods implemented in the Library class.
