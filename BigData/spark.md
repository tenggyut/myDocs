##Spark

###JDBC Thrift Server

####Socket Read Timeout Issue

Hive JDBC Connection use `DriverManager.getLoginTimeout()` as SQL execution timeout setting which is just 1 second by default.

So user may want to adjust the value of DriverManager's loginTimeout by calling `DriverManager.setLoginTimeout(your_prefered_sql_execution_timeout_in_seconds)`.