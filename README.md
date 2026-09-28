# Library Management API

A backend API for managing a library's books, members, borrowing and returns. The project is built using **ASP.NET Core 8**, **Entity Framework Core**, and **SQL Server**.

## Features

* Add, update, view and delete books
* Search books by title or author
* Register and manage library members
* Borrow and return books
* Limit each member to 3 active borrowed books
* Set a 14-day borrowing period
* Automatically calculate late fees at ₹5 per day
* View overdue books and a member's borrowing history
* Validate API request data
* Handle errors through global exception middleware
* Swagger UI for testing the API

## Tech Stack

* **C# / .NET 8**
* **ASP.NET Core Web API**
* **Entity Framework Core**
* **SQL Server / LocalDB**
* **Swagger / Swashbuckle**

## Project Structure

```text
LibraryManagement/
├── Controllers/     # API endpoints
├── Services/        # Business logic
├── Models/          # Book, Member and BorrowRecord entities
├── DTOs/            # Request and response models
├── Data/            # DbContext, indexes and seed data
├── Middleware/      # Global error handling
└── SQL/             # SQL reporting queries
```

The project follows a simple layered structure:

```text
Controllers → Services → DbContext → SQL Server
```

Controllers handle HTTP requests, services contain the main business logic, and Entity Framework Core handles database operations.

## OOP Concepts Used

The project also uses common OOP concepts:

* **Inheritance:** Entities inherit common properties from `BaseEntity`
* **Encapsulation:** Book stock changes are handled through methods such as `CheckOut()` and `CheckIn()`
* **Interfaces:** Services use interfaces such as `IBookService` and `IBorrowService`
* **Polymorphism:** Service implementations follow their respective interfaces
* **Dependency Injection:** Services are registered and injected through `Program.cs`
* **Separation of Concerns:** Controllers, business logic and database operations are kept separate

## Getting Started

### Requirements

You will need:

* .NET 8 SDK
* SQL Server or SQL Server LocalDB
* Entity Framework Core CLI

Install the EF Core CLI if you don't already have it:

```bash
dotnet tool install --global dotnet-ef
```

### Run the Project

Clone the repository and open the project folder:

```bash
git clone https://github.com/<your-username>/library-management-api-dotnet.git
cd library-management-api-dotnet
```

Restore the required packages:

```bash
dotnet restore
```

Create the database using EF Core:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

Then start the application:

```bash
dotnet run
```

Once the application is running, open the Swagger URL shown in the terminal, usually:

```text
https://localhost:<port>/swagger
```

### Configuration

Loan duration and the late fee can be changed in `appsettings.json`:

```json
"Library": {
  "LoanDays": 14,
  "FinePerDay": 5
}
```

## API Endpoints

| Method | Endpoint                        | Purpose                          |
| ------ | ------------------------------- | -------------------------------- |
| GET    | `/api/books?search=`            | Get or search books              |
| GET    | `/api/books/{id}`               | Get a book by ID                 |
| POST   | `/api/books`                    | Add a new book                   |
| PUT    | `/api/books/{id}`               | Update a book                    |
| DELETE | `/api/books/{id}`               | Delete a book                    |
| GET    | `/api/members`                  | Get all members                  |
| POST   | `/api/members`                  | Register a member                |
| POST   | `/api/borrow`                   | Borrow a book                    |
| POST   | `/api/borrow/{recordId}/return` | Return a borrowed book           |
| GET    | `/api/borrow/overdue`           | Get overdue books                |
| GET    | `/api/borrow/member/{memberId}` | Get a member's borrowing history |

### Borrowing Example

```http
POST /api/borrow
Content-Type: application/json

{
  "bookId": 1,
  "memberId": 1
}
```

If the book has no available copies, the API returns an error such as:

```json
{
  "error": "No copies available."
}
```

## Validation and Error Handling

The API validates incoming data using data annotations in the DTOs.

A global exception middleware is also used to return consistent error responses for cases such as:

* Invalid requests (`400`)
* Resource not found (`404`)
* Unexpected server errors (`500`)

Some business rules handled by the API include:

* A member can have a maximum of 3 active loans
* A book cannot be borrowed when no copies are available
* A book cannot be deleted while it is currently borrowed
* The same book cannot be returned twice
* Duplicate ISBNs are not allowed
* Late returns automatically generate a fine

## Testing

The API can currently be tested using Swagger.

Some of the scenarios tested include:

* Creating a book with an existing ISBN
* Borrowing and returning books
* Checking available copies after borrowing or returning
* Trying to borrow more than 3 books
* Trying to borrow a book with no available copies
* Returning the same book twice
* Calculating fines for overdue books
* Requesting a book that doesn't exist
* Deleting a book that is currently borrowed

Automated tests using **xUnit and Moq** are planned for a future update.

## SQL Reports

The `SQL/reports.sql` file contains a few reporting queries, including:

* Currently active loans
* Overdue books
* Most borrowed books
* Fines collected per member

## Future Improvements

Some features planned for future versions:

* Unit testing with xUnit and Moq
* JWT authentication
* Admin and Member roles
* Pagination and sorting
* Structured logging with Serilog
* Docker support
* GitHub Actions CI pipeline

## Author

**Riya**

* LinkedIn: (https://www.linkedin.com/in/riya-sharma-48bb34253/)
  
