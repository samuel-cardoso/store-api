Desenvolver uma API ou aplicação mvc utilizando C# como linguagem e asp net core api ou asp net core mvc. Essa aplicação deve os cruds completos e também retornar listagem dos elementos no banco. 

Tecnologias e conceitos que devem ser utilizadas:
- A aplicação deve ser feita respeitando o concetio de DDD;
- A camada de dados deve utilizar o micro ORM Dapper;
- Banco de dados utilizado deve ser o MySql;
 
  Abaixo o script para a criação da base de dados store e suas respectivas tabelas:

```sql
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema store
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema store
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `store` DEFAULT CHARACTER SET utf8 ;
USE `store` ;

-- -----------------------------------------------------
-- Table `store`.`Brand`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `store`.`Brand` (
  `BrandId` INT NOT NULL AUTO_INCREMENT,
  `Name` VARCHAR(250) NULL,
  `ImageRelativePath` VARCHAR(650) NULL,
  PRIMARY KEY (`BrandId`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `store`.`Product`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `store`.`Product` (
  `ProductId` INT NOT NULL AUTO_INCREMENT,
  `Name` VARCHAR(250) NULL,
  `IsActive` VARCHAR(45) NULL,
  `Price` DECIMAL(13,2) NULL,
  `Descripton` VARCHAR(650) NULL,
  `BrandId` INT NOT NULL,
  PRIMARY KEY (`ProductId`, `BrandId`),
  INDEX `fk_Product_Brand1_idx` (`BrandId` ASC) VISIBLE,
  CONSTRAINT `fk_Product_Brand1`
    FOREIGN KEY (`BrandId`)
    REFERENCES `store`.`Brand` (`BrandId`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `store`.`Category`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `store`.`Category` (
  `CategoryId` INT NOT NULL AUTO_INCREMENT,
  `Name` VARCHAR(45) NULL,
  PRIMARY KEY (`CategoryId`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `store`.`ProductCategory`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `store`.`ProductCategory` (
  `ProductId` INT NOT NULL,
  `CategoryId` INT NOT NULL,
  PRIMARY KEY (`ProductId`, `CategoryId`),
  INDEX `fk_Product_has_Category_Category1_idx` (`CategoryId` ASC) VISIBLE,
  INDEX `fk_Product_has_Category_Product_idx` (`ProductId` ASC) VISIBLE,
  CONSTRAINT `fk_Product_has_Category_Product`
    FOREIGN KEY (`ProductId`)
    REFERENCES `store`.`Product` (`ProductId`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Product_has_Category_Category1`
    FOREIGN KEY (`CategoryId`)
    REFERENCES `store`.`Category` (`CategoryId`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
