# The Log: What every software engineer should know about real-time data's unifying abstraction

[Blog link](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying)


- [What is Log?](#what-is-log)
  - [Logs in DB](#logs-in-db)
  - [Logs in distributed system](#logs-in-distributed-system)
  - [Distributed system with 2 broad approach to processing and replicating](#distributed-system-with-2-broad-approach-to-processing-and-replicating)
  - [Changelog 101: Tables and events are dual](#changelog-101-tables-and-events-are-dual)
- [Data Integration](#data-integration)
  - [Log structured data flow](#log-structured-data-flow)
  - [Relation to ETL and Data warehouse](#relation-to-etl-and-data-warehouse)
  - [Better approach](#better-approach)
  - [Log files and events](#log-files-and-events)
  - [Building a scalable log](#building-a-scalable-log)

## What is Log?
- smallest possible storage abstraction
- append only, totally ordered sequence of records ordered by time. 

Data log - built for programmatic access and are different from syslog or log4j

### Logs in DB
- DB uses a log to write out information about the records they will be modifying to keep DB in sync in case of DB crash. 

### Logs in distributed system
- Two problems logs solve
  - ordering changes
  - distributing data

### Distributed system with 2 broad approach to processing and replicating
1. State machine model - Active - active model
    - keep a log of incoming request and each replica process each request. 
    - Ex Calculator service (starts with zero) - "+1", "*2"
2. Primary backup model - elect 1 leader and allow the leader to process request in the order they arrive. 
    - Leader logs out the changes to its state from processing the request. 
    - Ex Calculator service (starts with zero) - "1", "3", "6"

### Changelog 101: Tables and events are dual
- IF you have a log of changes, ,you can apply these changes in order to create the table capturing the event state. 

## Data Integration
- making the data available

### Log structured data flow
* Log  - natural data structure for handling data flow between system. 
* Data source - can be an application that logs out event or a database table that accept modification
* Subscribing system - read logs, process it and advances its position in the log. 

* Logs also acts as a buffer that makes data production async from data consumption. 
* Logs act as a kind of messaging system with durability guarantees and strong ordering semantics. 

### Relation to ETL and Data warehouse
* Data warehouse - contains clean, integrated data. We run batch queries which is well suited for reporting. 
* ETL - it's an extraction and data cleanup process.
  * Data is restructured for data warehousing queries. 
* Issues - It's slow, data flow is fragile and hard to scale. 
  
  
### Better approach 
* Have a centralized pipeline, the log.
* Cleanup - done prior to publishing the data to the log by the publisher of the data. 
* Value-added transformation - post processing on the raw log feed produced. 
* Aggregation - specific to the destination system. 

### Log files and events
* Benefits - it enables decoupled, event driven system. 

### Building a scalable log

