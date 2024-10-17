
# Hometeq Web Application

## Project Overview

This project implements a dynamic web application for Hometeq, an online store specializing in smart home technology. It was developed using **PHP**, **MySQL**, **HTML/CSS**, and **JavaScript**. The web app includes a product listing page, a product details page, and a basket for users to purchase products.

### Project Structure

- **index.php**: The homepage displaying available products dynamically.
- **prodbuy.php**: The product page showing detailed information for selected products.
- **basket.php**: The shopping basket page where users can add products.
- **aboutus.php**: The "About Us" page with company information.
- **db.php**: The database connection file.
- **CSS and Images**: Custom stylesheets and images for product displays.

---

## Technical Implementation

### 1. MySQL Database Setup

- **Product Table Creation**: 
  The `Product` table was created in MySQL to store product information, including `prodId`, `prodName`, `prodPicNameSmall`, `prodPicNameLarge`, `prodDescripShort`, `prodDescripLong`, `prodPrice`, and `prodQuantity`. This table is used to dynamically retrieve product information and display it on the website.

  ```sql
  CREATE TABLE Product (
      prodId INT AUTO_INCREMENT,
      prodName VARCHAR(200) NOT NULL,
      prodPicNameSmall VARCHAR(200) NOT NULL,
      prodPicNameLarge VARCHAR(200) NOT NULL,
      prodDescripShort VARCHAR(1000),
      prodDescripLong VARCHAR(2000),
      prodPrice DECIMAL(8,2) NOT NULL DEFAULT '0.00',
      prodQuantity INT NOT NULL DEFAULT '100',
      PRIMARY KEY (prodId)
  );
  ```

- **Populating Product Table**: Inserted product records into the table using SQL `INSERT` statements.

  ```sql
  INSERT INTO Product (prodName, prodPicNameSmall, prodPicNameLarge, prodDescripShort, prodDescripLong, prodPrice, prodQuantity)
  VALUES ('HIVE Active Heating Thermostat', 'hivesmall.jpg', 'hivebig.jpg', 'Short description', 'Long description', 145.00, 35);
  ```

### 2. Database Connectivity

- The **db.php** file establishes the MySQL database connection for the app. This file is included in each PHP page that requires database access.

  ```php
  $conn = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);
  if (!$conn) {
      die('Could not connect: ' . mysqli_error($conn));
  }
  mysqli_select_db($conn, $dbname);
  ```

### 3. Dynamic Product Display (index.php)

- The **index.php** page retrieves product names and small images from the database and displays them dynamically. It includes clickable anchors that pass the `prodId` to the product details page via a URL parameter using the `GET` method.

  ```php
  $SQL = "SELECT prodId, prodName, prodPicNameSmall FROM Product";
  $exeSQL = mysqli_query($conn, $SQL);
  
  while ($arrayp = mysqli_fetch_array($exeSQL)) {
      echo "<a href='prodbuy.php?u_prod_id=".$arrayp['prodId']."'>";
      echo "<img src='images/".$arrayp['prodPicNameSmall']."' />";
      echo "<h5>".$arrayp['prodName']."</h5>";
      echo "</a>";
  }
  ```

### 4. Product Details Page (prodbuy.php)

- The **prodbuy.php** page retrieves detailed information about a selected product, including a large image, description, price, and stock quantity. The product ID is passed via the URL and used in a `WHERE` clause to filter the specific product from the database.

  ```php
  $prodid = $_GET['u_prod_id'];
  $SQL = "SELECT prodName, prodPicNameLarge, prodDescripLong, prodPrice, prodQuantity FROM Product WHERE prodId=".$prodid;
  $exeSQL = mysqli_query($conn, $SQL);
  $arrayp = mysqli_fetch_array($exeSQL);
  
  echo "<img src='images/".$arrayp['prodPicNameLarge']."' />";
  echo "<p>".$arrayp['prodName']."</p>";
  echo "<p>".$arrayp['prodDescripLong']."</p>";
  echo "<p>£".$arrayp['prodPrice']."</p>";
  echo "<p>Stock: ".$arrayp['prodQuantity']."</p>";
  ```

### 5. Adding to Basket

- A form on the **prodbuy.php** page allows users to select a quantity of the product to purchase and add it to the basket. This form sends data via the `POST` method to the **basket.php** page.

  ```php
  echo "<form action='basket.php' method='post'>";
  echo "<input type='hidden' name='h_prodid' value='".$prodid."'>";
  echo "<select name='p_quantity'>";
  for ($i=1; $i<=$arrayp['prodQuantity']; $i++) {
      echo "<option value='".$i."'>".$i."</option>";
  }
  echo "</select>";
  echo "<input type='submit' value='Add to Basket'>";
  echo "</form>";
  ```

### 6. Styling (CSS)

- The layout and styling for the web app is done using an external CSS file (`mystylesheet.css`) that was downloaded and customized.

### 7. Footer and Header Files

- The **headfile.html** and **footfile.html** include the page header, navigation bar, and footer for consistent layout across the web app.


