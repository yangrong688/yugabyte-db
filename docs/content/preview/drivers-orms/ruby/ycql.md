---
title: Connect an application
linkTitle: Connect an app
description: Ruby drivers for YCQL
image: /images/section_icons/sample-data/s_s1-sampledata-3x.png
menu:
  preview:
    identifier: ycql-ruby-driver
    parent: ruby-drivers
    weight: 420
type: docs
---

<div class="custom-tabs tabs-style-2">
  <ul class="tabs-name">
    <li>
      <a href="../ysql-pg/" class="nav-link">
        YSQL
      </a>
    </li>
    <li class="active">
      <a href="../ycql/" class="nav-link">
        YCQL
      </a>
    </li>
  </ul>
</div>

<ul class="nav nav-tabs-alt nav-tabs-yb">
   <li >
    <a href="../ycql/" class="nav-link active">
      <i class="icon-cassandra" aria-hidden="true"></i>
      YugabyteDB Ruby Driver
    </a>
  </li>
</ul>

## Install the Yugabyte Ruby Driver for YCQL

To install the [Yugabyte Ruby Driver for YCQL](https://github.com/yugabyte/cassandra-ruby-driver), run the following `gem install` command:

```sh
$ gem install yugabyte-ycql-driver
```

## Create a sample Ruby application

Create a file `yb-ycql-helloworld.rb` and copy the following content to it.

```ruby
require 'ycql'

# Create the cluster connection, connects to localhost by default.
cluster = Cassandra.cluster
session = cluster.connect()

# Create the keyspace.
session.execute('CREATE KEYSPACE IF NOT EXISTS ybdemo;')
puts "Created keyspace ybdemo"

# Create the table.
session.execute(
  """
  CREATE TABLE IF NOT EXISTS ybdemo.employee (id int PRIMARY KEY,
                                              name varchar,
                                              age int,
                                              language varchar);
  """)
puts "Created table employee"

# Insert a row.
session.execute(
  """
  INSERT INTO ybdemo.employee (id, name, age, language)
  VALUES (1, 'John', 35, 'Ruby');
  """)
puts "Inserted (id, name, age, language) = (1, 'John', 35, 'Ruby')"

# Query the row.
rows = session.execute('SELECT name, age, language FROM ybdemo.employee WHERE id = 1;')
rows.each do |row|
  puts "Query returned: %s %s %s" % [ row['name'], row['age'], row['language'] ]
end

# Close the connection.
cluster.close()
```

## Run the application

To use the application, run the following command:

```sh
$ ruby yb-cql-helloworld.rb
```

You should see the following output.

```output
Created keyspace ybdemo
Created table employee
Inserted (id, name, age, language) = (1, 'John', 35, 'Ruby')
Query returned: John 35 Ruby
```