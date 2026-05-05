# Single Responsibility Principle

```{important}
A class should have one, and only one, reason to change. This means that a class should have only one responsibility.
```

When a class has multiple responsibilities, any change in one responsibility might affect the others, leading to unexpected bugs or making the system fragile. 

SRP promotes modularity and separation of concerns, allowing developers to make changes to one part of the code without fear of breaking other parts.

SRP ensures that a class focuses on a single purpose, which makes your code easier to:

- **Understand**: Since the class has a clear, specific role.
- **Maintain**: Changes related to that role don't affect unrelated functionality.
- **Test**: Simplifies testing, as you only need to verify one responsibility.

## Cohesion of Implementation

Cohesion refers to how well the methods and properties of a class work together to achieve a single, well-defined purpose. A class with high cohesion contains only methods and attributes that are closely related to its specific responsibility, which aligns directly with the SRP.

### High Cohesion

- The component is focused and self-contained.
- Easier to understand, test, and debug.
- For example a `UserManager` class that only has CRUD methods that manage users in the database

  ```cpp
  class UserManager 
  {
  public:
      void create_user(const User& user) 
      {
          // Code to create a user
      }
      {
          // Code to create a user
      }
  
      void delete_user(const std::string& user_name) 
      {
          // Code to delete a user
      }
  
      void update_user(const User& user) 
      {
          // Code to update a user
      }
      {
          // Code to update a user's password
      }
  
      void get_user(const std::string& username) 
      {
          // Code to retrieve user information
      }
  };
  ```

### Low Cohesion

- The component tries to do too many unrelated things.
- Difficult to maintain because changes in one aspect may break others.
- For example, a `UserManager` class that handles user management, email sending, generating reports, and logging:

  ```cpp
  class UserManager 
  {
  public:
      void create_user(const User& user) 
      {
          // Code to create a user
      }
  
      void send_email(const std::string& email) 
      {
          // Code to send an email
      }
  
      void log_activity(const std::string& activity) 
      {
          // Code to log user activity
      }
  
      void generate_report() 
      {
          // Code to generate a report about user activities
      }
  };
  ```

## Example of SRP

Imagine a class that handles both user authentication and logging:

```cpp
class UserManager 
{
public:
    void authenticate_user(const std::string& username, const std::string& password) 
    {
        // Code for user authentication
    }

    void log_activity(const std::string& activity) 
    {
        // Code for logging user activity
    }
};
```

This class violates SRP because it has two responsibilities which can independently change:

- Authenticating users.
- Logging user activities.

We can fix this by separating the responsibilities into two classes:

```cpp
class Authenticator 
{
public:
    void authenticate_user(const std::string& username, const std::string& password) 
    {
        // Code for user authentication
    }
};

class Logger 
{
public:
    void log_activity(const std::string& activity) 
    {
        // Code for logging user activity
    }
};
```

Now, each class has a single responsibility:

- `Authenticator` handles user authentication.
- `Logger` handles logging activities.

## Benefits of SRP

- **Encapsulation**: Keeps responsibilities isolated.
- **Flexibility**: You can modify or replace specific functionalities without impacting others.
- **Clean Code**: Makes your architecture more modular and easier to work with.