
## Role-Based Access Control (RBAC)

16. **What is Role-Based Access Control (RBAC) and how is it implemented in your project?**

### Answer:
Role-based access control (RBAC) refers to the idea of assigning permissions to users based on their role within an organization. It offers a simple, manageable approach to access management that is less prone to error than assigning permissions to users individually.

### Implementation in our project:

## 1) The user management project requires Admin Or Manager roles.

[Application_Screenshot_Roles_Required_RBAC](/screenshots/Question16/Q16_user_management_required_roles_RBAC.png)

## 2) The Roles available are :

ADMIN

ANONYMOUS

AUTHENTICATED

MANAGER

[Application_Available_Roles_ScreenshotfromDB](/screenshots/Question16/Q16_user_management_available_user_roles.png)

### Code Snippet
```
class UserRole(Enum):
    """Enumeration of user roles within the application, stored as ENUM in the database."""
    ANONYMOUS = "ANONYMOUS"
    AUTHENTICATED = "AUTHENTICATED"
    MANAGER = "MANAGER"
    ADMIN = "ADMIN"
```
[Return back to answer.md](/answer.md)