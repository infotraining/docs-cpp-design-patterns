# Abstract Factory

## Intent

The **Abstract Factory** is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.

## Example - from problem to solution

Let's write a framework that allows us to use different database engines. We can start with introducing a set of interfaces that represent:
- a connection to the database
- a command that can be executed on the database
- a transaction that can be started and committed

```cpp
class DbConnection
{
public:
    virtual ~DbConnection() = default;
    virtual void connect() = 0;
    virtual void disconnect() = 0;
};

class DbCommand
{
public:
    virtual ~DbCommand() = default;
    virtual DbResult execute(DbConnection& conn, const SqlCmd& cmd, const SqlParams& params) = 0;
};

class DbTransaction
{
public:
    virtual ~DbTransaction() = default;
    virtual void begin(DbConnection& conn) = 0;
    virtual void commit() = 0;
};
```

In order to handle specific database engines, we can create a set of concrete classes that implement the above interfaces. For example, we can create a `OracleConnection`, `OracleCommand`, and `OracleTransaction` for Oracle database engine. The same applies to other database engines like PostgreSQL or SQLite.

```cpp
// Family of related products for Oracle database engine
class OracleConnection : public DbConnection
{
public:
    void connect() override
    {
        // connect to Oracle database
    }

    void disconnect() override
    {
        // disconnect from Oracle database
    }
};

class OracleCommand : public DbCommand
{
public:
    DbResult execute(DbConnection& conn, const SqlCmd& cmd, const SqlParams& params) override
    {
        // execute command on Oracle database
    }
};

class OracleTransaction : public DbTransaction
{
public:
    void begin(DbConnection& conn) override
    {
        // begin transaction on Oracle database
    }

    void commit() override
    {
        // commit transaction on Oracle database
    }
};
```

Classes that handle Sql Server or PostgreSQL can be implemented in a similar way. The important part is that all of them implement the same interfaces, which allows us to use them interchangeably.

Having the different families of products we still need a way to create them. We don't want to expose the concrete classes to the client code. This would lead to a situation where the client code is tightly coupled to the concrete classes, making it hard to change the implementation later. For example, if we want to switch from Oracle to PostgreSQL, we would need to change all the places in the code where we create `OracleConnection`, `OracleCommand`, and `OracleTransaction` objects. This is not a good design. 

```cpp
void insert_to_db()
{
    auto connection = std::make_unique<OracleConnection>("localhost", "db", "user", "password", 5432);
    connection->connect();

    auto transaction = std::make_unique<OracleTransaction>();
    transaction->begin(*connection);

    auto command = std::make_unique<OracleCommand>();
    command->execute(*command, "INSERT INTO users (name, age) VALUES (?, ?)", { "John", 30 });
    
    transaction->commit();        
}
```

Instead, we want to provide a way to create the objects without specifying their concrete classes. This is where the **Abstract Factory** comes into play. We can create an interface that defines methods for creating the different families of products.

```cpp
class DbFactory
{
public:
    virtual ~DbFactory() = default;
    virtual std::unique_ptr<DbConnection> create_connection(const std::string& url, const std::string& db_name, const std::string& user, const std::string& password) = 0;
    virtual std::unique_ptr<DbCommand> create_command() = 0;
    virtual std::unique_ptr<DbTransaction> create_transaction() = 0;
};
```

We can then create concrete factories for each database engine. For example, we can create `OracleFactory`, `PostgreSqlFactory`, and `SqlServerFactory`.

```cpp
class OracleFactory : public DbFactory
{  
public:
    std::unique_ptr<DbConnection> create_connection(const std::string& url, const std::string& db_name, const std::string& user, const std::string& password) override
    {
        return std::make_unique<OracleConnection>(url, db_name, user, password);
    }

    std::unique_ptr<DbCommand> create_command() override
    {
        return std::make_unique<OracleCommand>();
    }

    std::unique_ptr<DbTransaction> create_transaction() override
    {
        return std::make_unique<OracleTransaction>();
    }
};
```

Now we can use `DbFactory` interface to create the objects without specifying their concrete classes. This allows us to switch between different database engines easily.

```cpp
void insert_to_db(DbFactory& factory)
{
    auto connection = factory.create_connection("localhost", "db", "user", "password");
    connection->connect();

    auto transaction = factory.create_transaction();
    transaction->begin(*connection);

    auto command = factory.create_command();
    command->execute(*command, "INSERT INTO users (name, age) VALUES (?, ?)", { "John", 30 });
    
    transaction->commit();        
}

int main()
{
    OracleFactory oracle_factory;
    insert_to_db(oracle_factory);

    PostgreSqlFactory postgresql_factory;
    insert_to_db(postgresql_factory);

    SqlServerFactory sqlserver_factory;
    insert_to_db(sqlserver_factory);
}
```

In this example, we can easily switch between different database engines by passing a different factory to the `insert_to_db` function. This allows us to create the objects without specifying their concrete classes, which is the main goal of the **Abstract Factory** pattern.

## Abstract Factory - Context

We have a families of related objects that differ in their implementation. For example, we have a family of database engines (Oracle, PostgreSQL, SQL Server) and we want to create a set of objects that represent the connection, command, and transaction for each database engine. 

## Abstract Factory - Problem

* We want to create a family of related objects without specifying their concrete classes
* We want to be able to switch between different families of products easily
* We want to avoid tight coupling between the client code and the concrete classes
* We should be able to add new families of products without changing the existing code      

## Abstract Factory - Solution

* Create an interface that defines methods for creating the different families of products
* Create concrete factories for each family of products that implement the interface
* Use the factory interface to create the objects without specifying their concrete classes

```{image} ../img/creational/abstract-factory.png
:width: 800px
:align: center
```

## Abstract Factory - Consequences

* The client code is decoupled from the concrete classes, which makes it easier to switch between different families of products
* The client code is easier to maintain and extend, as we can add new families of products without changing the existing code
* Adding a new product to a family is difficult, as we need to modify the factory interface and all the concrete factories

## Abstract Factory vs. Factory Method

* The **Factory Method** pattern provides an interface for creating a single object without specifying its concrete class. It is used when we want to create a single object that can be of different types, but we don't want to expose the concrete classes to the client code.
* The **Abstract Factory** pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes. It is used when we want to easily replace a set of related objects that are designed to work together, ensuring consistency among the objects created by the factory.
* Typical solution for the **Abstract Factory** pattern is to use an interface that consists of several factory methods - each for a product in the family. 