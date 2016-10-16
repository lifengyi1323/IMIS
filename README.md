# IMIS
##电气设备保养系统
  一、
    1.数据库ER图
  ![](https://github.com/lifengyi1323/IMIS/blob/master/ER%E5%9B%BE.png)
    2.建表SQL
   ```
   （1）创建设备类型表
    CREATE TABLE `设备类型` (
    `设备类型ID` int(11) NOT NULL,
    `设备类型` varchar(45) NOT NULL,
    `保养周期` int(11) NOT NULL,
    `提前期` int(11) NOT NULL,
    `备注` varchar(45) DEFAULT NULL,
    PRIMARY KEY (`设备类型ID`)
  INSERT INTO `设备类型` VALUES (2,'防冻液喷洒设备',7,2,'11月20日-4月1日'),(5,'桶位仪',30,5,NULL),(16,'振动筛电机',180,7,NULL),(18,'球磨机',365,20,NULL);

  （2）创建设备表：
  CREATE TABLE `设备` (
    `设备ID` int(11) NOT NULL,
    `设备类型ID` int(11) NOT NULL,
    `最后一次保养时间` date NOT NULL,
    PRIMARY KEY (`设备ID`),
    KEY `k1_idx` (`设备类型ID`),
    CONSTRAINT `k1` FOREIGN KEY (`设备类型ID`) REFERENCES `设备类型` (`设备类型ID`) ON DELETE CASCADE ON UPDATE CASCADE
  INSERT INTO `设备` VALUES (1,2,'2016-10-11'),(2,5,'2016-10-10'),(3,16,'2016-10-10'),(4,18,'2016-10-09'),(5,2,'2016-10-11');
  （3）创建保养项目表：
  CREATE TABLE `保养项目` (
    `保养项目ID` int(11) NOT NULL,
    `设备类型ID` int(11) NOT NULL,
    `项目名称` varchar(45) NOT NULL,
    PRIMARY KEY (`保养项目ID`),
    KEY `k2_idx` (`设备类型ID`),
    CONSTRAINT `k2` FOREIGN KEY (`设备类型ID`) REFERENCES `设备类型` (`设备类型ID`) ON DELETE CASCADE ON UPDATE CASCADE；
  INSERT INTO `保养项目` VALUES (1,2,'防冻液喷洒系统电磁阀密封、防腐检'),(2,2,'防冻液喷洒系统泵和液压站电机接线、密封、防腐；'),(3,2,'防冻液喷洒系统现场开关检查、接线箱接线、密封、防腐；'),(4,5,'校对液位仪准确度');
  （4）创建保养记录表：
  CREATE TABLE `保养记录` (
    `保养记录ID` int(11) NOT NULL,
    `设备ID` int(11) NOT NULL,
    `保养项目ID` int(11) NOT NULL,
    `保养人` varchar(45) NOT NULL,
    `完成情况` varchar(45) NOT NULL,
    `时间` date NOT NULL,
    PRIMARY KEY (`保养记录ID`),
    KEY `k3_idx` (`设备ID`),
    KEY `k4_idx` (`保养项目ID`),
    CONSTRAINT `k3` FOREIGN KEY (`设备ID`) REFERENCES `设备` (`设备ID`) ON DELETE NO ACTION ON UPDATE NO ACTION,
    CONSTRAINT `k4` FOREIGN KEY (`保养项目ID`) REFERENCES `保养项目` (`保养项目ID`) ON DELETE NO ACTION ON UPDATE NO ACTION；
  INSERT INTO `保养记录` VALUES (1,1,1,'aaa','完成','2016-10-09'),(2,1,2,'aaa','完成','2016-10-11'),(3,2,4,'bbb','完成','2016-10-10');

  5.创建保养消耗表：
  CREATE TABLE `保养xiaohao` (
    `保养消耗ID` int(11) NOT NULL,
    `保养记录ID` int(11) NOT NULL,
    `消耗材料名称` varchar(45) NOT NULL,
    `数量` int(11) NOT NULL,
    `单位` varchar(45) NOT NULL,
    PRIMARY KEY (`保养消耗ID`),
    KEY `k5_idx` (`保养记录ID`),
    CONSTRAINT `k5` FOREIGN KEY (`保养记录ID`) REFERENCES `保养记录` (`保养记录ID`) ON DELETE CASCADE ON UPDATE CASCADE；
  INSERT INTO `保养xiaohao` VALUES (1,1,'电线',2,'根');
  ```
  3.查询SQL
  ```
  (1)根据设备ID查询保养记录
select * from 设备 as a inner join 保养记录 as b on a.设备ID=b.设备ID
```
 ![](https://github.com/lifengyi1323/IMIS/blob/master/%E6%9F%A5%E8%AF%A2%E4%BF%9D%E5%85%BB%E8%AE%B0%E5%BD%95.png)
 ```
select * from 设备 as a inner join 保养记录 as b on a.设备ID=b.设备ID where a.设备ID="1"
```
  ![](https://github.com/lifengyi1323/IMIS/blob/master/%E6%9F%A5%E8%AF%A2%E4%BF%9D%E5%85%BB%E8%AE%B0%E5%BD%951.png)
  ```
(2)根据设备ID打印检修报告
select * from 设备 as a inner join 保养记录 as b on a.设备ID=b.设备ID left join 保养xiaohao as c on b.保养记录ID=c.保养记录ID where a.设备ID="1"  
```
  ![](https://github.com/lifengyi1323/IMIS/blob/master/%E6%89%93%E5%8D%B0%E6%A3%80%E4%BF%AE%E6%8A%A5%E5%91%8A.png)

