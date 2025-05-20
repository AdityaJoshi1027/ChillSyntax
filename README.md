## Features

### Customer Features
- User registration and authentication
- Product browsing and search functionality 
- Category-based filtering
- Shopping cart management
  - Add/remove products
  - Update quantities
  - Cart count tracking
- Order placement and history
- Email notifications for:
  - Registration confirmation
  - Order placement
  - Order shipping updates
  - Back-in-stock alerts

### Admin Features
- Product management
  - Add/edit/remove products
  - Update prices and inventory
  - Upload product images
- Order management
  - View all orders
  - Update shipping status
  - Track sold items

## Technical Architecture

### Models
1. User Model (`com.chillsyntax.beans.UserBean`)
   - Attributes: email, name, mobile, address, pincode, password

2. Product Model (`com.chillsyntax.beans.ProductBean`)
   - Attributes: prodId, prodName, prodType, prodInfo, prodPrice, prodQuantity, prodImage

3. Cart Model (`com.chillsyntax.beans.CartBean`)
   - Attributes: userId, prodId, quantity

4. Order Model (`com.chillsyntax.beans.OrderBean`)
   - Attributes: transactionId, productId, quantity, amount, shipped

### Data Access Layer
The project uses JDBC for database operations with the following key services:

1. Product Service (`ProductServiceImpl`)
   ```java
   - addProduct(ProductBean product)
   - updateProduct(ProductBean prevProduct, ProductBean updatedProduct)
   - removeProduct(String prodId)
   - getAllProducts()
   - getProductDetails(String prodId)
   - searchAllProducts(String search)
   ```

2. Cart Service (`CartServiceImpl`)
   ```java
   - addProductToCart(String userId, String prodId, int prodQty)
   - updateProductToCart(String userId, String prodId, int prodQty)
   - removeProductFromCart(String userId, String prodId)
   - getAllCartItems(String userId)
   - getCartCount(String userId)
   ```

3. Order Service (`OrderServiceImpl`)
   ```java
   - paymentSuccess(String userName, double paidAmount)
   - addOrder(OrderBean order)
   - getAllOrders()
   - getOrdersByUserId(String emailId)
   - shipNow(String orderId, String prodId)
   ```

### Database Design
The database schema includes the following tables:

1. `user` 
   ```sql
   CREATE TABLE user (
     email VARCHAR(60) PRIMARY KEY,
     name VARCHAR(30),
     mobile BIGINT,
     address VARCHAR(250),
     pincode INT,
     password VARCHAR(20)
   )
   ```

2. `product`
   ```sql
   CREATE TABLE product (
     pid VARCHAR PRIMARY KEY,
     pname VARCHAR,
     ptype VARCHAR,
     pinfo TEXT,
     pprice DOUBLE,
     pquantity INT,
     image BLOB
   )
   ```

3. `usercart`
   ```sql
   CREATE TABLE usercart (
     username VARCHAR REFERENCES user(email),
     prodid VARCHAR REFERENCES product(pid),
     quantity INT
   )
   ```

4. `orders`
   ```sql
   CREATE TABLE orders (
     orderid VARCHAR,
     prodid VARCHAR REFERENCES product(pid),
     quantity INT,
     amount DOUBLE,
     shipped INT
   )
   ```

## UI Design & Components

The application uses JSP for server-side rendering with Bootstrap for responsive design:

1. Customer Interface
   - Header with navigation and cart count
   - Product grid with filtering options
   - Cart management interface
   - Order history view
   - Responsive mobile design

2. Admin Interface 
   - Product management dashboard
   - Order tracking system
   - Inventory management
   
## Component Layout

The project follows a layered architecture:

```
shopping-cart/
├── src/
│   └── com/chillsyntax/
│       ├── beans/           # Model classes
│       ├── service/         # Service interfaces
│       ├── service/impl/    # Service implementations
│       ├── utility/         # Helper classes
│       └── srv/            # Servlet controllers
├── WebContent/
│   ├── admin/              # Admin interface JSPs
│   ├── customer/           # Customer interface JSPs
│   ├── css/               # Stylesheets
│   ├── js/                # JavaScript files
│   └── images/            # Static resources
└── databases/             # Database scripts
`````

## Setup Instructions

### Prerequisites
1. Java JDK 8+ 
2. Eclipse EE (Enterprise Edition)
3. Apache Maven
4. MySQL Server & Workbench
5. Apache Tomcat 8.0+

### Database Setup
1. Open MySQL Workbench and connect to your local MySQL server
2. Execute the database creation script from `databases/mysql_query.sql`
3. Verify the tables are created: user, product, usercart, orders, transactions

### Email Configuration
1. Create/use a Gmail account for sending notifications
2. Enable 2-step verification at https://myaccount.google.com/security
3. Generate an App Password:
   - Go to https://myaccount.google.com/apppasswords
   - Select "Other (Custom name)" and enter "ChillSyntax Shop"
   - Copy the generated 16-digit password

### Project Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/shashirajraja/shopping-cart.git
   ```

2. Import into Eclipse:
   - File > Import > Git > Projects From Git > Clone URI
   - Enter repo URL and select master branch
   - Import as Maven project

3. Configure application properties:
   - Open `src/application.properties`
   - Update database credentials:
     ```properties
     db.username=your_mysql_username
     db.password=your_mysql_password
     ```
   - Update email settings:
     ```properties
     mailer.email=your_gmail
     mailer.password=your_app_password
     ```

4. Build the project:
   - Right-click project > Run as > Maven Build
   - Goals: `clean install`
   - Apply and run

5. Server configuration:
   - Right-click project > Run As > Run on Server
   - Choose Tomcat v8.0+
   - Start the server

6. Access the application:
   - Open http://localhost:8080/shopping-cart/
   - Default admin credentials are in the database script

### Troubleshooting
- Port conflicts: Change Tomcat port in server.xml
- Database connection: Verify MySQL is running and credentials are correct
- Build errors: Update Maven dependencies and project facets
- Classpath issues: Verify all required JARs are in WEB-INF/lib

## Contributing
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request


