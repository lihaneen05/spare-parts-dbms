# spare parts dbms

-- Table structure for table category
--
IF OBJECT_ID('category', 'U') IS NOT NULL
DROP TABLE category;
CREATE TABLE category (
  CATEGORY_ID INT NOT NULL IDENTITY(1,1) PRIMARY KEY,
  CNAME VARCHAR(50) DEFAULT NULL
);

--
-- Dumping data for table category
--
SET IDENTITY_INSERT category ON;
INSERT INTO category (CATEGORY_ID, CNAME) VALUES
(1, 'Electric_parts'),
(2, 'Metal_parts'),
(3, 'Rubber_parts'),
(4, 'Tranmission_parts');
SET IDENTITY_INSERT category OFF;

-- --------------------------------------------------------

--
-- Table structure for table customer
--
IF OBJECT_ID('customer', 'U') IS NOT NULL
DROP TABLE customer;
CREATE TABLE customer (
  CUST_ID INT NOT NULL IDENTITY(1,1) PRIMARY KEY,
  FIRST_NAME VARCHAR(50) DEFAULT NULL,
  LAST_NAME VARCHAR(50) DEFAULT NULL,
  PHONE_NUMBER VARCHAR(11) DEFAULT NULL
);

--
-- Dumping data for table customer
--
SET IDENTITY_INSERT customer ON;
INSERT INTO customer (CUST_ID, FIRST_NAME, LAST_NAME, PHONE_NUMBER) VALUES
(17, 'Ahmed', 'Khan', '03001234567'),
(18, 'Fatima', 'Ali', '03337654321'),
(19, 'Bilal', 'Hassan', '03219876543'),
(20, 'Ayesha', 'Malik', '03451122334'),
(21, 'Usman', 'Qureshi', '03125566778');
SET IDENTITY_INSERT customer OFF;

-- --------------------------------------------------------

--
-- Table structure for table job
--
IF OBJECT_ID('job', 'U') IS NOT NULL
DROP TABLE job;
CREATE TABLE job (
  JOB_ID INT NOT NULL PRIMARY KEY,
  JOB_TITLE VARCHAR(50) DEFAULT NULL
);

--
-- Dumping data for table job
--
INSERT INTO job (JOB_ID, JOB_TITLE) VALUES
(1, 'Manager'),
(2, 'Cashier');

-- --------------------------------------------------------

--
-- Table structure for table location
--
IF OBJECT_ID('location', 'U') IS NOT NULL
DROP TABLE location;
CREATE TABLE location (
  LOCATION_ID INT NOT NULL IDENTITY(1,1) PRIMARY KEY,
  PROVINCE VARCHAR(100) DEFAULT NULL,
  CITY VARCHAR(100) DEFAULT NULL
);

--
-- Dumping data for table location
--
SET IDENTITY_INSERT location ON;
INSERT INTO location (LOCATION_ID, PROVINCE, CITY) VALUES
(113, 'Punjab', 'Lahore'),
(159, 'Sindh', 'Karachi'),
(160, 'Punjab', 'Rawalpindi'),
(161, 'Khyber Pakhtunkhwa', 'Peshawar'),
(162, 'Balochistan', 'Quetta'),
(163, 'Punjab', 'Faisalabad'),
(164, 'Sindh', 'Hyderabad'),
(165, 'Punjab', 'Multan');
SET IDENTITY_INSERT location OFF;

-- --------------------------------------------------------

--
-- Table structure for table employee
--
IF OBJECT_ID('employee', 'U') IS NOT NULL
DROP TABLE employee;
CREATE TABLE employee (
  EMPLOYEE_ID INT NOT NULL IDENTITY(1,1) PRIMARY KEY,
  FIRST_NAME VARCHAR(50) DEFAULT NULL,
  LAST_NAME VARCHAR(50) DEFAULT NULL,
  GENDER VARCHAR(50) DEFAULT NULL,
  EMAIL VARCHAR(100) DEFAULT NULL,
  PHONE_NUMBER VARCHAR(11) DEFAULT NULL UNIQUE,
  JOB_ID INT DEFAULT NULL REFERENCES job(JOB_ID),
  LOCATION_ID INT DEFAULT NULL REFERENCES location(LOCATION_ID)
);

--
-- Dumping data for table employee
--
SET IDENTITY_INSERT employee ON;
INSERT INTO employee (EMPLOYEE_ID, FIRST_NAME, LAST_NAME, GENDER, EMAIL, PHONE_NUMBER, JOB_ID, LOCATION_ID) VALUES
(1, 'Ali', 'Raza', 'Male', 'ali.raza@example.com', '03017654321', 1, 113),
(5, 'Sana', 'Javed', 'Female', 'sana.javed@example.com', '03331234567', 1, 163),
(6, 'Imran', 'Butt', 'Male', 'imran.butt@example.com', '03229876543', 2, 164),
(7, 'Hina', 'Abbasi', 'Female', 'hina.abbasi@example.com', '03461122334', 2, 165);
SET IDENTITY_INSERT employee OFF;

-- --------------------------------------------------------

--
-- Table structure for table manager
--
IF OBJECT_ID('manager', 'U') IS NOT NULL
DROP TABLE manager;
CREATE TABLE manager (
  FIRST_NAME VARCHAR(50) DEFAULT NULL,
  LAST_NAME VARCHAR(50) DEFAULT NULL,
  LOCATION_ID INT NOT NULL REFERENCES location(LOCATION_ID),
  EMAIL VARCHAR(50) DEFAULT NULL,
  PHONE_NUMBER VARCHAR(11) DEFAULT NULL UNIQUE
);
-- No PK defined as per original schema.

--
-- Dumping data for table manager
--
-- (No data was present in the original dump for this table)

-- --------------------------------------------------------
--
-- Table structure for table supplier
--
IF OBJECT_ID('supplier', 'U') IS NOT NULL
DROP TABLE supplier;
CREATE TABLE supplier (
  SUPPLIER_ID INT NOT NULL IDENTITY(1,1) PRIMARY KEY,
  COMPANY_NAME VARCHAR(50) DEFAULT NULL,
  LOCATION_ID INT NOT NULL REFERENCES location(LOCATION_ID),
  PHONE_NUMBER VARCHAR(11) DEFAULT NULL
);

--
-- Dumping data for table supplier
--
SET IDENTITY_INSERT supplier ON;
INSERT INTO supplier (SUPPLIER_ID, COMPANY_NAME, LOCATION_ID, PHONE_NUMBER) VALUES
(17, 'Honda Atlas Parts PK', 159, '03001112233'),
(18, 'Toyota Indus Motors Spares', 160, '03334455667'),
(19, 'Pak Suzuki Motor Parts', 161, '03217788990'),
(20, 'Al-Haj FAW Parts Center', 162, '03450011223');
SET IDENTITY_INSERT supplier OFF;

-- --------------------------------------------------------

--
-- Table structure for table product
--
IF OBJECT_ID('product', 'U') IS NOT NULL
DROP TABLE product;
CREATE TABLE product (
  PRODUCT_ID INT NOT NULL IDENTITY(1,1) PRIMARY KEY,
  PRODUCT_CODE VARCHAR(20) NOT NULL,
  NAME VARCHAR(100) DEFAULT NULL,
  DESCRIPTION VARCHAR(250) NOT NULL,
  QTY_STOCK INT DEFAULT NULL,
  ON_HAND INT NOT NULL,
  PRICE INT DEFAULT NULL,
  CATEGORY_ID INT DEFAULT NULL REFERENCES category(CATEGORY_ID),
  SUPPLIER_ID INT DEFAULT NULL REFERENCES supplier(SUPPLIER_ID),
  DATE_STOCK_IN VARCHAR(50) NOT NULL
);

--
-- Dumping data for table product
--
SET IDENTITY_INSERT product ON;
INSERT INTO product (PRODUCT_ID, PRODUCT_CODE, NAME, DESCRIPTION, QTY_STOCK, ON_HAND, PRICE, CATEGORY_ID, SUPPLIER_ID, DATE_STOCK_IN) VALUES
(28, 'HCR-EML-01', 'Honda Civic Reborn Engine Mount Left', 'Engine Mount, Left Side for Honda Civic Reborn 2006-2012', 15, 15, 4500, 3, 17, '2023-10-01'),
(29, 'HCRB-HG-01', 'Head Gasket for Civic Rebirth', 'Cylinder Head Gasket for Honda Civic Rebirth 2012-2016 R18A', 25, 25, 2800, 3, 17, '2023-10-01'),
(30, 'TCG-CVTF-01', 'Torque Converter CVT Filter Corolla Altis', 'CVT Transmission Fluid Filter for Toyota Corolla Altis Grande 1.8', 20, 20, 3500, 4, 18, '2023-10-02'),
(31, 'HCX-BPF-01', 'Honda Civic X Brake Pads Front', 'Front Brake Pads Set for Honda Civic X / RS Turbo 2016-2021', 30, 30, 6500, 2, 17, '2023-10-03'),
(32, 'TCGLI-AF-01', 'Toyota Corolla GLI Air Filter', 'Engine Air Filter for Toyota Corolla GLI/XLI 1.3L', 50, 50, 1200, 1, 18, '2023-10-04'),
(33, 'HSN-OF-01', 'Hyundai Sonata Oil Filter', 'Engine Oil Filter for Hyundai Sonata 2.5L', 40, 40, 900, 2, 19, '2023-10-05'),
(34, 'HEL-SPK-01', 'Hyundai Elantra Spark Plugs (Set of 4)', 'Spark Plugs, Set of 4 for Hyundai Elantra 1.6L/2.0L', 30, 30, 2200, 1, 19, '2023-10-05'),
(35, 'TFRT-SAR-01', 'Toyota Fortuner Shock Absorber Rear', 'Rear Shock Absorber for Toyota Fortuner (Diesel/Petrol)', 10, 10, 18000, 2, 18, '2023-10-06'),
(36, 'TRV-FF-01', 'Toyota Revo Fuel Filter', 'Diesel Fuel Filter for Toyota Hilux Revo 2.8D', 20, 20, 2500, 2, 18, '2023-10-06'),
(37, 'LCPR-BP-01', 'Land Cruiser Prado Brake Pads Front', 'Front Brake Pads for Toyota Land Cruiser Prado J150', 15, 15, 12000, 2, 18, '2023-10-07'),
(38, 'LCZX-O2S-01', 'Land Cruiser ZX V8 Oxygen Sensor Bank 1', 'Oxygen Sensor (Bank 1, Sensor 1) for Land Cruiser ZX V8', 5, 5, 15000, 1, 18, '2023-10-07'),
(39, 'HCR-EMR-01', 'Honda Civic Reborn Engine Mount Right', 'Engine Mount, Right Side for Honda Civic Reborn 2006-2012', 12, 12, 4800, 3, 17, '2023-10-08'),
(40, 'HCRB-TC-01', 'Torque Converter for Civic Rebirth AT', 'Automatic Transmission Torque Converter for Honda Civic Rebirth', 3, 3, 25000, 4, 17, '2023-10-08'),
(41, 'TCG-CVTOIL-01', 'CVT Fluid for Corolla Altis (4L)', 'Toyota Genuine CVT Fluid FE (4 Liters) for Corolla Altis', 20, 20, 7500, 4, 18, '2023-10-09'),
(42, 'HCX-TL-L-01', 'Honda Civic X Tail Light Left', 'Left Tail Light Assembly for Honda Civic X (Sedan)', 8, 8, 11000, 1, 17, '2023-10-09'),
(43, 'TCGLI-CP-01', 'Toyota Corolla GLI Clutch Plate', 'Clutch Plate for Toyota Corolla GLI/XLI 1.3L Manual', 18, 18, 5500, 4, 18, '2023-10-10'),
(44, 'HSN-HL-R-01', 'Hyundai Sonata Headlight Right LED', 'Right LED Headlight Assembly for Hyundai Sonata', 4, 4, 45000, 1, 19, '2023-10-10'),
(45, 'HEL-BP-R-01', 'Hyundai Elantra Brake Pads Rear', 'Rear Brake Pads Set for Hyundai Elantra', 25, 25, 4000, 2, 19, '2023-10-11'),
(46, 'TFRT-OFD-01', 'Toyota Fortuner Oil Filter Diesel', 'Engine Oil Filter for Toyota Fortuner 2.8L Diesel', 30, 30, 1500, 2, 18, '2023-10-11'),
(47, 'TRV-LBJ-01', 'Toyota Revo Lower Ball Joint', 'Lower Ball Joint for Toyota Hilux Revo (Left/Right)', 16, 16, 3800, 2, 18, '2023-10-12'),
(48, 'LCPR-ASF-01', 'Land Cruiser Prado Air Suspension Filter', 'Air Suspension Compressor Filter for LC Prado J150', 10, 10, 6000, 1, 18, '2023-10-12'),
(49, 'LCZX-AFM-01', 'Land Cruiser ZX Air Flow Meter', 'Mass Air Flow Meter/Sensor for Land Cruiser ZX V8', 6, 6, 13500, 1, 18, '2023-10-13'),
(50, 'HCR-RADCAP-01', 'Honda Civic Reborn Radiator Cap', 'Radiator Cap (1.1 bar) for Honda Civic Reborn', 50, 50, 700, 2, 17, '2023-10-13'),
(51, 'HCRB-BELT-01', 'Serpentine Belt for Civic Rebirth', 'Serpentine/Drive Belt for Honda Civic Rebirth R18A', 30, 30, 1800, 3, 17, '2023-10-14'),
(52, 'TCG-THERM-01', 'Thermostat for Corolla Altis', 'Engine Coolant Thermostat for Toyota Corolla Altis 1.8L', 22, 22, 2200, 2, 18, '2023-10-14'),
(53, 'GEN-NUT-M8', 'Generic Nut M8', 'Standard M8 Nut, Zinc Plated', 200, 200, 10, 2, 20, '2023-10-15'),
(54, 'GEN-BOLT-M8-20', 'Generic Bolt M8x20mm', 'Standard M8x20mm Bolt, Steel', 150, 150, 20, 2, 20, '2023-10-15'),
(664, 'SZSW-WPR-01', 'Suzuki Swift Wiper Blade Set', 'Front Wiper Blade Set for Suzuki Swift (2010-2017)', 20, 20, 1500, 3, 19, '2023-11-01');
SET IDENTITY_INSERT product OFF;

-- --------------------------------------------------------
--
-- Table structure for table transaction and transaction_details
--
IF OBJECT_ID('transaction_details', 'U') IS NOT NULL
DROP TABLE transaction_details;
IF OBJECT_ID('transaction', 'U') IS NOT NULL
DROP TABLE transaction;

CREATE TABLE transaction (
  TRANS_ID INT NOT NULL IDENTITY(1,1) PRIMARY KEY,
  CUST_ID INT DEFAULT NULL REFERENCES customer(CUST_ID),
  NUMOFITEMS VARCHAR(250) NOT NULL,
  SUBTOTAL VARCHAR(50) NOT NULL,
  LESSVAT VARCHAR(50) NOT NULL,
  NETVAT VARCHAR(50) NOT NULL,
  ADDVAT VARCHAR(50) NOT NULL,
  GRANDTOTAL VARCHAR(250) NOT NULL,
  CASH VARCHAR(250) NOT NULL,
  DATE VARCHAR(50) NOT NULL,
  TRANS_D_ID VARCHAR(250) NOT NULL UNIQUE
);

CREATE TABLE transaction_details (
  ID INT NOT NULL IDENTITY(1,1) PRIMARY KEY,
  TRANS_D_ID VARCHAR(250) NOT NULL REFERENCES transaction(TRANS_D_ID),
  PRODUCTS VARCHAR(250) NOT NULL,
  QTY VARCHAR(250) NOT NULL,
  PRICE VARCHAR(250) NOT NULL,
  EMPLOYEE VARCHAR(250) NOT NULL,
  ROLE VARCHAR(250) NOT NULL
);

--
-- Dumping data for table transaction
--
SET IDENTITY_INSERT transaction ON;
INSERT INTO transaction (TRANS_ID, CUST_ID, NUMOFITEMS, SUBTOTAL, LESSVAT, NETVAT, ADDVAT, GRANDTOTAL, CASH, DATE, TRANS_D_ID) VALUES
(13, 20, '1', '2800.00', '300.00', '2500.00', '300.00', '2800.00', '3000', '2023-11-10 10:15 am', 'TXN111141641PK'),
(14, 19, '2', '2100.00', '225.00', '1875.00', '225.00', '2100.00', '2100', '2023-11-10 11:30 am', 'TXN111151334PK');
SET IDENTITY_INSERT transaction OFF;

--
-- Dumping data for table transaction_details
--
SET IDENTITY_INSERT transaction_details ON;
INSERT INTO transaction_details (ID, TRANS_D_ID, PRODUCTS, QTY, PRICE, EMPLOYEE, ROLE) VALUES
(21, 'TXN111141641PK', 'Head Gasket for Civic Rebirth', '1', '2800', 'Ali Raza', 'Manager'),
(22, 'TXN111151334PK', 'Toyota Corolla GLI Air Filter', '1', '1200', 'Ali Raza', 'Manager'),
(23, 'TXN111151334PK', 'Hyundai Sonata Oil Filter', '1', '900', 'Ali Raza', 'Manager');
SET IDENTITY_INSERT transaction_details OFF;


-- --------------------------------------------------------

--
-- Table structure for table type
-- TYPE is a reserved keyword in SQL Server, so it MUST be quoted.
--
IF OBJECT_ID('[type]', 'U') IS NOT NULL -- Quoted here because TYPE is a keyword
DROP TABLE [type];
CREATE TABLE [type] ( -- Quoted here
  TYPE_ID INT NOT NULL PRIMARY KEY,
  [TYPE] VARCHAR(50) DEFAULT NULL -- Column name also quoted for consistency, though not strictly necessary if not a keyword itself
);

--
-- Dumping data for table type
--
INSERT INTO [type] (TYPE_ID, [TYPE]) VALUES -- Quoted table name
(1, 'Admin'),
(2, 'User');

-- --------------------------------------------------------

--
-- Table structure for table users
--
IF OBJECT_ID('users', 'U') IS NOT NULL
DROP TABLE users;
CREATE TABLE users (
  ID INT NOT NULL IDENTITY(1,1) PRIMARY KEY,
  EMPLOYEE_ID INT DEFAULT NULL REFERENCES employee(EMPLOYEE_ID),
  USERNAME VARCHAR(50) DEFAULT NULL,
  PASSWORD VARCHAR(50) DEFAULT NULL,
  TYPE_ID INT DEFAULT NULL REFERENCES [type](TYPE_ID) -- Referenced table name quoted
);

--
-- Dumping data for table users
--
SET IDENTITY_INSERT users ON;
INSERT INTO users (ID, EMPLOYEE_ID, USERNAME, PASSWORD, TYPE_ID) VALUES
(1, 1, 'admin_ali', '6C7CA345F63F835CB353FF15BD6C5E052EC08E7A', 1),
(2, 5, 'admin_sana', '315F166C5ACA63A157F7D41007675CB44A948B33', 1),
(3, 6, 'user_imran', '33AAB3C7F01620CADE108F488CFD285C0E62C1EC', 2),
(4, 7, 'user_hina', 'EA053D11A8AAD1CCF8C18F9241BAEB9EC47E5D64', 2);
SET IDENTITY_INSERT users OFF;


COMMIT TRANSACTION;
