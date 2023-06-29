# chapter 0 : 환경 구축

<br>

## 1. MySQL, Workbench 설치 (환경 : Mac, CPU : Apple Sillicon, M1 MAX)
### 1️⃣ install MySQL
- install MySQL using terminal
```shell
$ brew install mysql
```
<img width="579" alt="install" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/65186012-acf7-4636-9bbb-2e556b684004">

<br>

### 2️⃣ check installation
- check version
```shell
$ mysql -V
```
<img width="367" alt="version check" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/b9b01c4d-d4e7-4e16-82d1-325000148de8">

- check running state
```shell
$ mysql.server start
```
<img width="341" alt="run server" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/192e8a46-a921-4103-a989-38cbb1bfc5f4">

<br>

### 3️⃣ configure MySQL
- set up configuration using terminal
```shell
$ mysql_secure_installation
```
<img width="631" alt="configure" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/3ffffc58-cf32-4ae6-9116-a75f9fa34978">

<br>

### 4️⃣ install Workbench
- install <a href="https://dev.mysql.com/downloads/workbench/">Workbench</a>

<br>

## 2. Create example tables
### 1️⃣ create schema
```mysql
CREATE SCHEMA `exampleSchema` DEFAULT CHARACTER SET utf8 ;
```
<img width="1068" alt="create schema" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/eeb2e894-3f2f-4596-87c4-934b7b06b44a">

<br>

### 2️⃣ create databases
- database : customer

```mysql
CREATE TABLE `exampleSchema`.`customer` (
  `customer_id` INT NOT NULL,
  `customer_name` VARCHAR(45) NULL DEFAULT NULL,
  `birthday` DATETIME NULL DEFAULT NULL,
  `membertype_id` INT NULL DEFAULT NULL,
  PRIMARY KEY (`customer_id`));
```

<img width="1071" alt="customer database" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/60776e8e-9913-405a-ba30-b442affd6a18">

<br><br>

- database : membertype

```mysql
CREATE TABLE `exampleSchema`.`membertype` (
  `membertype_id` INT NOT NULL,
  `membertype` VARCHAR(45) NULL,
  PRIMARY KEY (`membertype_id`));
```

<img width="1072" alt="membertype database" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/1aad125c-aca1-4064-9609-1497d85038cb">

<br><br>

- database : product

```mysql
CREATE TABLE `exampleSchema`.`product` (
  `product_id` INT NOT NULL,
  `product_name` VARCHAR(20) NULL DEFAULT NULL,
  `stock` INT NULL,
  `price` DECIMAL(10,0) NULL,
  PRIMARY KEY (`product_id`));
```

<img width="1065" alt="product database" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/b6deb2c6-2e6d-4490-a8ea-79e24d18846d">

<br><br>

- database : productorder

```mysql
CREATE TABLE `exampleSchema`.`productorder` (
  `order_id` INT NOT NULL,
  `customer_id` INT NULL,
  `product_id` INT NULL,
  `quantity` INT NULL,
  `price` DECIMAL(10,0) NULL,
  `order_time` DATETIME NULL,
  PRIMARY KEY (`order_id`));
```

<img width="1064" alt="productorder database" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/52ff9b16-2370-45b6-9083-8d69fd7f9283">

<br>

### 3️⃣ insert sample data
- insert sample datum in product table

```mysql
INSERT INTO `exampleschema`.`product` (`product_id`, `product_name`, `stock`, `price`) VALUES ('1', '약용 입욕제', '100', '70');
INSERT INTO `exampleschema`.`product` (`product_id`, `product_name`, `stock`, `price`) VALUES ('2', '약용 핸드솝', '23', '700');
INSERT INTO `exampleschema`.`product` (`product_id`, `product_name`, `stock`, `price`) VALUES ('3', '천연 아로마 입욕제', '4', '120');
INSERT INTO `exampleschema`.`product` (`product_id`, `product_name`, `stock`, `price`) VALUES ('4', '거품 목욕제', '23', '120');
```

<img width="270" alt="insert sample data" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/fed75dae-fe2e-406a-a5d7-84a69bdffdd3">

<br>
