SENG8071 - Midterm Assignment

Meetkumar Hareshbhai Prajapati - 8952762
Sameer Pratik Rao - 8888610
Gurkirat Singh - 8882095

# Group Members and Assigned Duties
------------------------------------------------------
| Member Name    | Student ID | Assigned Duty        |
|----------------|------------|----------------------|
| Gurkirat S     | 8882095    | Database Design      |
| Meetkumar HP   | 8952762    | SQL Code             |
| Sameer P Rao   | 8888610    | Typescript Interface |
------------------------------------------------------

## Tables and Attributes
### Books
-------------------------------
| Attribute Name  | Data Type |
|-----------------|-----------|
| BookId          | 123       |
| Title           | Hopeless  |
| Genre           | Thriller  |
| AuthorId        | HP12      |
| PublisherId     | PB1       |
| PublishDate     | 2008-05-12|
| Price           | 450       |
-------------------------------
### Authors
-------------------------------
| Attribute Name  | Data Type |
|-----------------|-----------|
| AuthorId        | 452       |
| Name            | J M SOD   |
| Biography       | Time lie  |
| Birthdate       | 1928-09-21|
-------------------------------
### BookAuthor
-------------------------------
| Attribute Name  | Data Type |
------------------|-----------|
| BookId          | 309       |
| AuthorId        | 452       |
| AuthorName      | J M SOD   |
-------------------------------
### Publishers
-------------------------------
| Attribute Name  | Data Type |
|-----------------|-----------|
| PublisherId     | HJ9       |
| Name            | S D JON   |
| ContactInfo     | 9876789   |
-------------------------------
### Customers
-------------------------------
| Attribute Name  | Data Type |
|-----------------|-----------|
| CustomerId      | CU2       |
| Name            | BEN WUK   |
| Email           | FA@HAM.   |
| TotalSpent      | $3444     |
| RegistrationDate| 2011-09-11|
-------------------------------
### Orders
------------------------------
| Attribute Name | Data Type |
|----------------|-----------|
| OrderId        | OD3       |
| CustomerId     | CU2       |
| OrderDate      | 2012-08-14|
| TotalAmount    | $150      |
------------------------------
### OrderDetails
-------------------------------
| Attribute Name  | Data Type |
|-----------------|-----------|
| OrderDetailId   | OI9       |
| OrderId         | OD3       |
| BookId          | 378       |
| Quantity        | 1         |
| Price           | $150      |
-------------------------------
### Reviews
------------------------------
| Attribute Name | Data Type |
|----------------|-----------|
| ReviewId       | RE1       |
| CustomerId     | CU2       |
| BookId         | 378       |
| Rating         | 5         |
| ReviewDate     | 2012-08-15|
------------------------------

#### SQL

CREATE TABLE `Book`(
    BookID INT PRIMARY KEY,
    Title VARCHAR(20) NOT NULL,
    Genre VARCHAR(10) NOT NULL,
    PublicationDate DATE NOT NULL,
    Price DECIMAL(10, 2) NOT NULL,
    AverageRating DECIMAL(3, 2),
    PublisherID INT,
    FOREIGN KEY (PublisherID) REFERENCES Publisher(PublisherID)
);

-- Create the Author table

CREATE TABLE `Author` (
    AuthorID INT PRIMARY KEY,
    Name VARCHAR(20) NOT NULL,
    Biography TEXT
);

-- Create the BookAuthor table 

CREATE TABLE `BookAuthor` 
(
    BookID INT,
    AuthorID INT,
   AuthorName VARCHAR(20) NOT NULL
    PRIMARY KEY (BookID, AuthorID),
    FOREIGN KEY (BookID) REFERENCES Book(BookID),
    FOREIGN KEY (AuthorID) REFERENCES Author(AuthorID)
);

-- Create the Publisher table

CREATE TABLE `Publisher` (
    PublisherID INT PRIMARY KEY,
    Name VARCHAR(20) NOT NULL,
    ContactInfo VARCHAR(20)
);

-- Create the Customer table

CREATE TABLE `Customer` 
(
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(20) NOT NULL,
    Email VARCHAR(20) NOT NULL UNIQUE,
    RegistrationDate DATE NOT NULL,
    TotalSpentLastYear DECIMAL(10, 2) DEFAULT 0
);

-- Create the Order table

CREATE TABLE `Order` 
(
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATETIME NOT NULL,
    TotalAmount DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
);

-- Create the OrderItem table

CREATE TABLE `OrderItem` (
    OrderItemID INT PRIMARY KEY,
    OrderID INT,
    BookID INT,
    Quantity INT NOT NULL,
    Price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (OrderID) REFERENCES `Order`(OrderID),
    FOREIGN KEY (BookID) REFERENCES Book(BookID)
);

//Create the Review table//

CREATE TABLE `Review` 
(
    ReviewID INT PRIMARY KEY,
    BookID INT,
    CustomerID INT,
    Rating INT NOT NULL CHECK (*rating >= 1 AND rating <= 5*),
    ReviewText TEXT,
    ReviewDate DATETIME NOT NULL,
    FOREIGN KEY (`BookID`) REFERENCES Book(`BookID`),
    FOREIGN KEY (`CustomerID`) REFERENCES Customer(`CustomerID`)
);

##### DDL and DML (CRUD operations) on Author table

DDL: Create the Author table

CREATE TABLE `Author` (
    AuthorID INT PRIMARY KEY,
    Name VARCHAR(20) NOT NULL,
    Biography TEXT,
    BirthDate DATE
);

*DML: CRUD Operations for Author table*

\\Create (Insert) a new author\\

INSERT INTO `Author` (*Name, Biography, BirthDate*)
VALUES (‘J K Rowel’, 'The tsunami.', '1999-10-15');

\\Read (Select) all authors\\

SELECT * FROM `Author`;

\\Read (Select) a specific author by Name\\

SELECT * FROM `Author` 
WHERE Name = ‘Jon Snow’;

\\Update an author's information\\

UPDATE `Author`
SET Name = ‘Damon Salvator’
WHERE Name = ‘Klaus Michelson’;

\\Delete an author\\

DELETE FROM `Author` WHERE `AuthorID` = 1;

###### Table testing

10 most Recent posted reviews by Customers:

SELECT *r.ReviewID, r.BookID, b.Title as BookTitle, r.CustomerID, c.Name as CustomerName, r.Rating, r.ReviewText, r.ReviewDate*
FROM Review r
JOIN Book b ON r.BookID = b.BookID
JOIN Customer c ON r.CustomerID = c.CustomerID
ORDER BY r.ReviewDate DESC
LIMIT 5;

###### Typescript Interface allowing modification to table (Author Table)
    
interface `Author Table` {
    AuthorID: number;
    Name: string;
    Biography: string;  
    BirthDate: Date;   
}
    
--Modification
    
function `modifyAuthorTable`(modification: `AuthorTableModification`): 
void {
    console.log(Modifying author with ID {modification.AuthorID}:);
    if (modification.Name) {
        console.log(- Updating Name to: {'J K Simmons});
    }
    if (modification.Biography) {
        console.log(- Updating Biography to: {'What is it that you need'});
    }
    if (modification.BirthDate) {
        console.log(- Updating BirthDate to: {1923-09-21});
    }
}

---------------------------------------------
| Attribute name | Iteam                    |
-----------------|---------------------------
| Name           | J K Simmons              |
| Biography      | What is it that you need |
| Birthdate      | 1923-09-21               |
---------------------------------------------





    
    
    
    
    
    
    
    
    
    
    
    
    
