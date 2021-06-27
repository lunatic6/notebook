# Acemap数据爬取项目

## 1. 项目简介

Acemap是一个学术可视化和分析的网站，需要以大量的学术论文数据作为基础，因此就有了这个大规模爬取论文数据的项目

整个项目主要是分为两个方面爬取和存储

## 2. 爬取

​	我们的**爬取对象**主要是CCF推荐的各大出版商，比较典型的有ACM, IEEE, ARXIV等；

​	工具方面主要用到了python的**Scrapy框架**和**Redis**

	1. 通过对Scrapy框架的settings进行一些通用的配值，比如log级别，是否去重，可以使我们针对每个出版商网站只写spider就可以
 	2. Redis主要是两个用途
      	1. 作为分布式爬虫的请求url队列
      	2. 作为ip代理池的存储：
          - 用到zset数据结构，依据分数给代理排名
          - expire设置代理过期时间，不需要另外写脚本维护过期的功能

	## 3. 存储

​	遇到问题：对于html数据，我们最初是以html文档的形式存储在本地服务器上，但随着爬取的数据量增加，遇到了一个问题，html文件**小且多**，直接存在文件系统会导致**inode**（可以理解为文件的唯一id，由操作系统分配的）爆炸

​	解决：把html文件以二进制的形式和对应的mete信息，存到mongodb中，每个出版商对应一个crawlerHtml的表

​	解析：解析的时候从crawlerhtml表恢复html，用xpath解析出论文的相关字段数据，存到对应的/crawlerpaper表，字段：id，paperid，reference，publishdate，conference, author(name, affiliation),url,title，等等

​	以前解析的时候是遍历文件，需要大量的IO，现在是迭代一张表的response_body字段，速度自然快了很多

