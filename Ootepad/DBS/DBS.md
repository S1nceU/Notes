```sql
SET FOREIGN_KEY_CHECKS=0;

CREATE TABLE `world`.`ADMIN`  (
  `user_id` int NOT NULL AUTO_INCREMENT,
  `username` varchar(255) NOT NULL,
  `account` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `email` varchar(255) NULL,
  `user_status` bit(1) NULL,
  PRIMARY KEY (`user_id`)
);

CREATE TABLE `world`.`CART_PRODUCT`  (
  `product_id` int NOT NULL,
  `user_id_c` int NOT NULL,
  `amount` int NULL,
  CONSTRAINT `fk_CART_PRODUCT_PRODUCT_1` FOREIGN KEY (`product_id`) REFERENCES `world`.`PRODUCT` (`product_id`),
  CONSTRAINT `fk_CART_PRODUCT_CUSTOMER_1` FOREIGN KEY (`user_id_c`) REFERENCES `world`.`CUSTOMER` (`user_id_c`)
);

CREATE TABLE `world`.`CATEGORY`  (
  `product_id` int NOT NULL,
  `label_id` int NOT NULL,
  CONSTRAINT `fk_CATEGORY_LABEL_1` FOREIGN KEY (`label_id`) REFERENCES `world`.`LABEL` (`label_id`),
  CONSTRAINT `fk_CATEGORY_PRODUCT_1` FOREIGN KEY (`product_id`) REFERENCES `world`.`PRODUCT` (`product_id`)
);

CREATE TABLE `world`.`CUSTOMER`  (
  `user_id_c` int NOT NULL AUTO_INCREMENT,
  `username` varchar(255) NOT NULL,
  `account` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `email` varchar(255) NOT NULL,
  `address` varchar(255) NOT NULL,
  `phone` varchar(255) NOT NULL,
  `id_number` varchar(10) NOT NULL DEFAULT '',
  `user_status` bit(1) NOT NULL,
  PRIMARY KEY (`user_id_c`),
  UNIQUE INDEX `123`(`account`),
  UNIQUE INDEX `321`(`email`),
  UNIQUE INDEX `2`(`id_number`)
);

CREATE TABLE `world`.`LABEL`  (
  `label_id` int NOT NULL AUTO_INCREMENT,
  `label` varchar(255) NOT NULL,
  PRIMARY KEY (`label_id`)
);

CREATE TABLE `world`.`manage_t`  (
  `user_id` int NOT NULL,
  `ticket_id` int NOT NULL,
  CONSTRAINT `fk_manage_t_ADMIN_1` FOREIGN KEY (`user_id`) REFERENCES `world`.`ADMIN` (`user_id`),
  CONSTRAINT `fk_manage_t_TICKET_1` FOREIGN KEY (`ticket_id`) REFERENCES `world`.`TICKET` (`ticket_id`)
);

CREATE TABLE `world`.`ORDER`  (
  `order_id` int NOT NULL AUTO_INCREMENT,
  `total_price` float(10, 2) NOT NULL,
  `order_time` datetime NOT NULL,
  `address` varchar(255) NOT NULL,
  `status` int NOT NULL,
  `user_id_c` int NOT NULL,
  `ticket_id` int NULL,
  PRIMARY KEY (`order_id`),
  CONSTRAINT `fk_ORDER_CUSTOMER_1` FOREIGN KEY (`user_id_c`) REFERENCES `world`.`CUSTOMER` (`user_id_c`),
  CONSTRAINT `fk_ORDER_TICKET_1` FOREIGN KEY (`ticket_id`) REFERENCES `world`.`TICKET` (`ticket_id`)
);

CREATE TABLE `world`.`ORDER_PRODUCT`  (
  `order_id` int NOT NULL,
  `product_id` int NOT NULL,
  `amount` int NULL,
  CONSTRAINT `fk_ORDER_PRODUCT_ORDER_1` FOREIGN KEY (`order_id`) REFERENCES `world`.`ORDER` (`order_id`),
  CONSTRAINT `fk_ORDER_PRODUCT_PRODUCT_1` FOREIGN KEY (`product_id`) REFERENCES `world`.`PRODUCT` (`product_id`)
);

CREATE TABLE `world`.`PRODUCT`  (
  `user_id_s` int NOT NULL,
  `product_id` int NOT NULL,
  `product_name` varchar(255) NOT NULL,
  `price` int NOT NULL,
  `description` varchar(255) NULL,
  `publish_date` datetime NOT NULL,
  `status` bit(1) NOT NULL,
  `total_amount` int NOT NULL,
  PRIMARY KEY (`product_id`),
  CONSTRAINT `fk_PRODUCT_SELLER_1` FOREIGN KEY (`user_id_s`) REFERENCES `world`.`SELLER` (`user_id_s`)
);

CREATE TABLE `world`.`SALES_REPORT`  (
  `report_id` int NOT NULL AUTO_INCREMENT,
  `sales_results` varchar(255) NOT NULL,
  `sales_score` int NOT NULL,
  `user_id_s` int NOT NULL,
  PRIMARY KEY (`report_id`),
  CONSTRAINT `fk_SALES_REPORT_SELLER_1` FOREIGN KEY (`user_id_s`) REFERENCES `world`.`SELLER` (`user_id_s`)
);

CREATE TABLE `world`.`SELLER`  (
  `user_id_s` int NOT NULL AUTO_INCREMENT,
  `username` varchar(255) NOT NULL,
  `account` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `email` varchar(255) NOT NULL,
  `address` varchar(255) NOT NULL,
  `phone` varchar(255) NOT NULL,
  `id_number` varchar(10) NOT NULL,
  `user_status` bit(1) NOT NULL,
  PRIMARY KEY (`user_id_s`)
);

CREATE TABLE `world`.`TICKET`  (
  `ticket_id` int NOT NULL AUTO_INCREMENT,
  `effective_date` datetime NOT NULL,
  `amount` varchar(255) NOT NULL,
  `discount` varchar(255) NOT NULL,
  `user_id_s` int NOT NULL,
  PRIMARY KEY (`ticket_id`),
  CONSTRAINT `fk_TICKET_SELLER_1` FOREIGN KEY (`user_id_s`) REFERENCES `world`.`SELLER` (`user_id_s`)
);

CREATE TABLE `world`.`TO_DO_THING`  (
  `event_id` int NOT NULL AUTO_INCREMENT,
  `event_content` varchar(255) NULL,
  `event_state` bit(1) NOT NULL,
  `user_id` int NOT NULL,
  PRIMARY KEY (`event_id`),
  CONSTRAINT `fk_TO_DO_THING_ADMIN_1` FOREIGN KEY (`user_id`) REFERENCES `world`.`ADMIN` (`user_id`)
);

DROP TABLE `world`.`city`;

DROP TABLE `world`.`country`;

DROP TABLE `world`.`countrylanguage`;

SET FOREIGN_KEY_CHECKS=1;
```