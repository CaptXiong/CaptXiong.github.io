---
title: How to write the right logstash sql
author: Spike Xiong
date: 2020-11-30 15:10:00
categories: [ELK]
tags: [elsticsearch, logstash]
---

## Incremental update
#### tracking_column
It is recommended to use the update time as the tracking_column of the logstash when we do the incremental updating.

The update time should be the exactly same with the column in your database.

The default tracking_column_type is "numeric", the
 tracking_column_type should be "timestamp" when the date type data is tracked.

#### Synchronization time
If the database is writting data during the logstash synchronization, the situation may leads absence of the data.

Adding "updatedAt<NOW()" into the sql will ensure the data's completeness 

#### Timezone
Different timezones may leads mistakes during the synchronization.

It can be finetuning in the sql or filter part in xxx.conf file.

TIMESTAMPADD is a way in Mysql to finetuning the time.

### Result

example sql:

"SELECT * FROM `paper`
		 WHERE  updatedAt >=TIMESTAMPADD
		 (HOUR, -8
		 , :sql_last_value) and 
		 updatedAt<NOW() order by 
		 updatedAt ASC limit 
		 100000"




