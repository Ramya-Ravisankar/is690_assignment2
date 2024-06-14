
17. **Explain the different user roles defined in your project (ANONYMOUS, AUTHENTICATED, MANAGER, ADMIN) and their permissions.**

### Answer:
ADMIN and MANAGER :
1) The user roles of ADMIN and MANAGER are able to completely manage all users for the project - Complete User Management
2) Apart from managing users, these roles can also Manage events for users - Event  Management.
3) Modify login and registration information for their own accounts under My Account.

AUTHENTICATED:
1) A User with this role will be able to login into the application, register new users and retrieve their own user information under My Account. They will be able to use the GET/UPDATE/POST methods.
2) This role also helps to know if a user has been able to login successfully and that they are assigned with a jwt token.

ANOMYMOUS:
1) This role has very few permissions compared to AUTHENTICATED, ADMIN Or MANAGER roles.
2) The user with ANONYMOUS role can login into the application amd register and can use only PUT method under MyAccount API endpoint, under My Account and update their own accounts.

[Return back to answer.md](/answer.md)