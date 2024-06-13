9. **Detail the steps involved in the user email verification process. Provide a pseudo-code workflow from sending a verification email to activating the user's account.**

### Answer
1) Access the link - http://localhost:8000/docs
2) Navigate to the Login and Registration and under the GET method / Verify email /userid /token, click on try it out button. Enter the new user id and the token and click on the execute button.
The email will get verified for the user.

### MORE DETAILED STEPS ( SIMILAR TO THE ONE AVAILABLE IN QUESTION 2 AND QUESTION 8)

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

### Code Snippets from the project for Verify email:

[user_routes.py](/app/routers/user_routes.py)
```
@router.get("/verify-email/{user_id}/{token}", status_code=status.HTTP_200_OK, name="verify_email", tags=["Login and Registration"])
async def verify_email(user_id: UUID, token: str, db: AsyncSession = Depends(get_db), email_service: EmailService = Depends(get_email_service)):
    try:
        await UserService.verify_email_with_token(db, user_id, token)
        return RedirectResponse(url=settings.account_verfiy_destination)
    except InvalidVerificationTokenException as e:
        raise HTTPException(status_code=status.HTTP_400_BAD_REQUEST, detail=str(e))
    except UserNotFoundException as e:
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail=str(e))
```

[user_service.py](/app/services/user_service.py)
```
@classmethod
    async def verify_email_with_token(cls, session: AsyncSession, user_id: UUID, token: str) -> None:
        user = await cls.get_by_id(session, user_id)
        if user is None or user.verification_token != token:
            raise InvalidVerificationTokenException("Invalid or expired verification token")

        user.email_verified = True
        user.verification_token = None  # Clear the token once used
        if user.role == UserRole.ANONYMOUS:
            user.role = UserRole.AUTHENTICATED
        session.add(user)
        await session.commit()
        return True
```
[email_service.py](/app/services/email_service.py)

```
async def send_user_email(self, user_data: dict, email_type: str):
        subject_map = {
            'email_verification': "Verify Your Account",
            'password_reset': "Password Reset Instructions",
            'account_locked': "Account Locked Notification"
        }

        if email_type not in subject_map:
            raise ValueError("Invalid email type")

        html_content = self.template_manager.render_template(email_type, **user_data)
        self.smtp_client.send_email(subject_map[email_type], html_content, user_data['email'])

    async def send_verification_email(self, user: User):
        verification_url = f"{settings.server_base_url}verify-email/{user.id}/{user.verification_token}"
        await self.send_user_email({
            "name": user.first_name,
            "verification_url": verification_url,
            "email": user.email
        }, 'email_verification')
```

[Return back to answer.md](/answer.md)