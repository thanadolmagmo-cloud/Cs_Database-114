--เริ่มจาก Master สร้างฐานข้อมูลชื่อ CSMinimartCREATE DATABASE CSMinimart;
--ปรับให้ฐานข้อมูลสามารถเพิ่มข้อมูลที่เป็นภาษาไทยได้
ALTER DATABASE CSMinimart COLLATE Thai_CI_AS;USE CSMinimart;
--สร้างตารางเก็บข้อมูลพนักงาน ชื่อ EmployeesCREATE TABLE Employees(
    EmployeeID int identity(1,1) Primary Key,
    title varchar(20) null,
    firstname varchar(50) not null,
    lastname varchar(50) null,
    position varchar(50) null,
    username varchar(50) Unique,
    passwordhash varchar(255) not null,
    IsActive bit Not null default 1);INSERT INTO Employees (Title, FirstName, LastName, Position, UserName, PasswordHash) VALUES ('นางสาว', 'กาญจนา', 'พวงแก้ว', 'Sale Manager', 'user1', 'hashed1');INSERT INTO Employees (Title, FirstName, LastName, Position, UserName, PasswordHash) VALUES ('นาย', 'ธนดล', 'ใจใหญ่', 'Web Development', 'user2', 'hashed2');



 -- ตารางหมวดสินค้าCREATE
 TABLE Categories (
    CategoryID INT IDENTITY(1,1) PRIMARY KEY,
    CategoryName VARCHAR(50) NOT NULL UNIQUE,
    Description VARCHAR(200));Insert into Categories(CategoryName, Description)VALUES('เครื่องดื่ม', 'น้ำดื่ม น้ำผลไม้ ชาและกาแฟ');Insert into Categories(CategoryName, Description)VALUES('เครื่องปรุง', 'เกลือ น้ำตาล พริกไทยและผงมาซาล่า');Insert into Categories(CategoryName, Description)VALUES('อาหารสำเร็จรูป', 'มาม่า ปลากระป๋อง ผลไม้กระป๋องและโจ๊กสำเร็จรูป');Insert into Categories(CategoryName, Description)VALUES('เครื่องสำอาง', 'ลิปสติก แป้งรองพื้น มาสคาร่า อายแชโดว์');Insert into Categories(CategoryName, Description)VALUES('เวชภัณฑ์', 'ยาพารา ถุงมือ ไม้เท้า สำลี');
    -- ตารางสินค้า
    CREATE TABLE Products (
    ProductID VARCHAR(13) PRIMARY KEY,
    ProductName VARCHAR(100) NOT NULL,
    UnitPrice DECIMAL(10,2) NOT NULL DEFAULT 0,
    UnitsInStock INT NOT NULL DEFAULT 0,
    CategoryID INT NOT NULL,
    Discontinued BIT NOT NULL DEFAULT 0,

    CONSTRAINT CK_Products_UnitPrice
        CHECK (UnitPrice >= 0),

    CONSTRAINT CK_Products_UnitsInStock
        CHECK (UnitsInStock >= 0),

    CONSTRAINT FK_Products_Categories
        FOREIGN KEY (CategoryID)
        REFERENCES Categories(CategoryID));INSERT INTO Products    (ProductID, ProductName, UnitPrice,
    UnitsInStock, CategoryID)VALUES    ('8858757001948', 'โค้ก',
    15.00, 290, 1);

   

    INSERT INTO Products    (ProductID, ProductName, UnitPrice,
    UnitsInStock, CategoryID)VALUES    ('64598135498', 'Speakers ',
   600, 5411, 1);

   -- สร้างตารางใบเสร็จ
      CREATE TABLE Receipts (
    ReceiptID INT IDENTITY(1,1) PRIMARY KEY,
    ReceiptDate DATETIME NOT NULL
        DEFAULT GETDATE(),
    EmployeeID INT NOT NULL,
    TotalCash DECIMAL(10,2) NOT NULL DEFAULT 0,

    CONSTRAINT CK_Receipts_TotalCash
        CHECK (TotalCash >= 0),

    CONSTRAINT FK_Receipts_Employees
        FOREIGN KEY (EmployeeID)
        REFERENCES Employees(EmployeeID));INSERT INTO Receipts    (EmployeeID, TotalCash)VALUES    (1, 115.00);
        
        --เพิ่มข้อมูลในตารางใบเสร็จ
       INSERT INTO Receipts    (EmployeeID, TotalCash)VALUES    (1, 115.00);

    --ดูข้อมูลในตารางใบเสร็จ
    SELECT * FROM Receipts;

    --สร้างตาราง Details
    CREATE TABLE Details (
    ReceiptID INT NOT NULL,
    ProductID VARCHAR(13) NOT NULL,
    UnitPrice DECIMAL(10,2) NOT NULL,
    Quantity INT NOT NULL,

    CONSTRAINT PK_Details
        PRIMARY KEY (ReceiptID, ProductID),

    CONSTRAINT CK_Details_UnitPrice
        CHECK (UnitPrice >= 0),

    CONSTRAINT CK_Details_Quantity
        CHECK (Quantity > 0),

    CONSTRAINT FK_Details_Receipts
        FOREIGN KEY (ReceiptID)
        REFERENCES Receipts(ReceiptID),

    CONSTRAINT FK_Details_Products
        FOREIGN KEY (ProductID)
        REFERENCES Products(ProductID));INSERT INTO Details    (ReceiptID, ProductID, UnitPrice, Quantity)VALUES    (1, '88587571548628', 15.00, 3);

        SELECT * FROM Details;




 
