
2. **Outline the complete process of handling a user login request in your FastAPI application. Provide a step-by-step explanation with code examples from the project.**

## Answer :

## User Login Step by Step:

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

8) In the verification email received, click on the "verify email" link available inside the email. Check in database that for the first user (ADMIN) the User role is NOT changed and remains in ADMIN role, email verified column set to true and verification_token is set to NULL for this user.

9) Follow the same steps as above and create second user.
NOTE - Check if this user is updated in the database and for this,the Role updated by default is ANONYMOUS.

10) Check for the email received and click on the verify email link available inside the email triggered for the second user.

11) Once email is verified,  Check in database that for the second user the User role is changed from ANONYMOUS to AUTHENTICATED, email verified set to true and verification_token is set to NULL now for this second user.

12) Note down this second user's ( Basically an AUTHENTICATED USER ) name and password

13) Access the application login page by accessing the link - http://localhost:8000/login-form and provide the second user (Authenticated user's user name and password )

[AUTHENTICATED_USER_LOGIN](/screenshots/Question2/Login_As_AUTHENTICATED_USER.png)

14) Click on the login button. JWT token gets displayed.
[AUTHENTICATED_USER_JWT_TOKEN](/screenshots/Question2/AUTHENTICATED_USER_JWT_TOKEN.png)

15) Access the application - http://localhost:8000/docs and click on the Authorize button.Provide the authenticated user name and password and login into the application as an AUTHENTICATED USER.

[AUTHENTICATED_USER_SUCCESSFUL_LOGON](/screenshots/Question2/LOGGEDIN_AS_AUTHENTICATED_USER.png)

13) An AUTHENTICATED user can do all the CRUD operations - create another user, update user details etc.

Code Snippets from the project:

[user_routes.py](/app/routers/user_routes.py)

## Login with form  - code snippet - POST method

@router.post(
        "/login_with_form/",
        include_in_schema=False, tags=["Login and Registration"])
async def login(
    form_data: OAuth2PasswordRequestForm = Depends(),
    session: AsyncSession = Depends(get_db)
    ):
    try:
        user = await UserService.login_user(session, form_data.username, form_data.password)
        access_token_expires = timedelta(minutes=settings.access_token_expire_minutes)
        access_token = create_access_token(
            data={"sub": user.email, "role": str(user.role.name), "user_id": str(user.id)},
            expires_delta=access_token_expires
        )
        response = RedirectResponse(url="/dashboard", status_code=status.HTTP_303_SEE_OTHER)
        response.set_cookie(key="access_token", value=access_token, httponly=True)
        return response
    except InvalidCredentialsException as e:
        raise HTTPException(status_code=status.HTTP_401_UNAUTHORIZED, detail=str(e))
    except AccountLockedException as e:
        raise HTTPException(status_code=status.HTTP_403_FORBIDDEN, detail=str(e))

## Login  - code snippet - POST method

@router.post("/login/",
             response_model=TokenResponse,
             tags=["Login and Registration"]
             )
async def login(
    form_data: OAuth2PasswordRequestForm = Depends(),
    session: AsyncSession = Depends(get_db)
    ):
    try:
        user = await UserService.login_user(session, form_data.username, form_data.password)
        access_token_expires = timedelta(minutes=settings.access_token_expire_minutes)
        access_token = create_access_token(
            data={"sub": user.email, "role": str(user.role.name), "user_id": str(user.id)},
            expires_delta=access_token_expires
        )
        # Immediately decode to verify
        try:
            decoded = jwt.decode(access_token, settings.jwt_secret_key, algorithms=[settings.jwt_algorithm])
            logging.info(f"Immediate decode check: {decoded}")
        except jwt.PyJWTError as e:
            logging.error(f"Immediate decode failed: {e}")

        return {"access_token": access_token, "token_type": "bearer"}
    except InvalidCredentialsException as e:
        raise HTTPException(status_code=status.HTTP_401_UNAUTHORIZED, detail=str(e))
    except AccountLockedException as e:
        raise HTTPException(status_code=status.HTTP_403_FORBIDDEN, detail=str(e))



## Logout  - code snippet - POST method

@router.post("/logout", tags=["Login and Registration"])
async def logout(response: Response):
    response = RedirectResponse(url="/", status_code=status.HTTP_303_SEE_OTHER)
    response.delete_cookie(key="access_token")
    return response

[Return back to answer.md](/answer.md)