8. **Describe the user registration logic in your project. Provide a pseudo-code workflow from the registration request to storing the user in the database.**

## Answer:
## fastapi project - User Registration Logic - Step by Step :
1) The user accesses the login page at http://localhost:8000/docs

2) Under the Login and registration / POST method , click on the try it out link.Provide the required fields like email, first name, last name , profile url, user name , password etc for the first user and then hit the execute button.

[Login&Registration_newUser_Creation](/screenshots/Question2/Login_Registration_newuser.png)

3) Verify that there is successful response with response code 200

[New_User_SuccessfulResponse](/screenshots/Question2/New_User_Response.png)

4) Note down this user's email and password from the request that has been executed.

5) Login into database - http://localhost:5050/browser/ (PGADMIN link)
Access the PGADMIN link using credentials as admin@example.com (user) and adminpassword (password)
Search for the above ( first user ) record in database

[New_User_Database_updation](/screenshots/Question2/Database_Records.png)

6) Note : The first user created is by default an ADMIN user.

[New_User_First_User_Admin_role](/screenshots/Question2/Database_Records_screenshot.png)

7) Verify if new user verification email is sent to the above new Admin user ( first user )

[Email_Trigger_Screenshot](/screenshots/Question2/email.png)

8) To verify the email, access the same link http://localhost:8000/docs,
under the Login and Registration / Get method , click on try it out and provide the user email, token.
Click on execute and verify the email of the new user.

NOTE - We can also verify the email, through verification email being sent to  the user.
 In the email received for the user click on the "verify email" link available inside the email. Check in database that for the first user (ADMIN) the User role is NOT changed and remains in ADMIN role, email verified column set to true and verification_token is set to NULL for this user.

9) Follow the same steps as above and create second user.
NOTE - Check if this user is updated in the database and for this,the Role updated by default is ANONYMOUS.

10) Check for the email received and click on the verify email link available inside the email triggered for the second user OR can also verify the email of the user through
Login and Registration\GET method.

11) Once email is verified,  Check in database that for the second user the User role is changed from ANONYMOUS to AUTHENTICATED, email verified set to true and verification_token is set to NULL now for this second user.

12) Note down this second user's ( Basically an AUTHENTICATED USER ) name and password

13) Access the application login page by accessing the link - http://localhost:8000/login-form and provide the second user (Authenticated user's user name and password )

[AUTHENTICATED_USER_LOGIN](/screenshots/Question2/Login_As_AUTHENTICATED_USER.png)

14) Click on the login button. JWT token gets displayed.
[AUTHENTICATED_USER_JWT_TOKEN](/screenshots/Question2/AUTHENTICATED_USER_JWT_TOKEN.png)

15) Access the application - http://localhost:8000/docs and click on the Authorize button.Provide the authenticated user name and password and login into the application as an AUTHENTICATED USER.

[AUTHENTICATED_USER_SUCCESSFUL_LOGON](/screenshots/Question2/LOGGEDIN_AS_AUTHENTICATED_USER.png)

16) An AUTHENTICATED user can do all the CRUD operations - create another user, update user details etc.

## Code Snippets from the project:
 ### Register User Code Snippet from the project:
 [user_routes.py](/app/routers/user_routes.py)
 ```
 async def register(
    user_data: UserCreate = Body(...),
    session: AsyncSession = Depends(get_db),
    email_service: EmailService = Depends(get_email_service)):
    try:
        user = await UserService.register_user(session, user_data.model_dump(), email_service)
        return UserResponse.model_construct(
            nickname=user.nickname,
            first_name=user.first_name,
            last_name=user.last_name,
            bio=user.bio,
            profile_picture_url=user.profile_picture_url,
            github_profile_url=user.github_profile_url,
            linkedin_profile_url=user.linkedin_profile_url,
            role=user.role,
            email=user.email,
            last_login_at=user.last_login_at,
            created_at=user.created_at,
            updated_at=user.updated_at,
        )
    except EmailAlreadyExistsException as e:
        raise HTTPException(status_code=status.HTTP_400_BAD_REQUEST, detail=str(e))
 ```

### Add New User to Database Code Snippets from the project:
[user_service.py](/app/services/user_service.py)
```
@classmethod
    async def create(cls, session: AsyncSession, user_data: Dict[str, str], email_service: EmailService) -> User:
        try:
            validated_data = UserCreate(**user_data).model_dump()
            existing_user = await cls.get_by_email(session, validated_data['email'])
            if existing_user:
                raise EmailAlreadyExistsException("User with given email already exists.")

            validated_data['hashed_password'] = hash_password(validated_data.pop('password'))
            new_user = User(**validated_data)
            new_nickname = generate_nickname()
            while await cls.get_by_nickname(session, new_nickname):
                new_nickname = generate_nickname()
            new_user.nickname = new_nickname
            user_count = await cls.count(session)
            new_user.role = UserRole.ADMIN if user_count == 0 else UserRole.ANONYMOUS
            if new_user.role == UserRole.ADMIN:
                new_user.email_verified = True


            new_user.verification_token = generate_verification_token()
            session.add(new_user)
            await session.commit()
            await email_service.send_verification_email(new_user)
            return new_user
        except ValidationError as e:
            raise e
```

```
@classmethod
    async def register_user(cls, session: AsyncSession, user_data: Dict[str, str], email_service: EmailService) -> User:
        return await cls.create(session, user_data, email_service)
```

[Return back to answer.md](/answer.md)