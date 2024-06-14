
3. **Explain the service repository pattern and how it is applied in your project. Provide an example of how routes are managed and linked to services.**

## Answer
The service repository pattern is a design pattern that can help improve the structure and maintainability of software applications. It separates the data access logic into a separate layer, which can reduce code duplication and improve code organization. The pattern also promotes code testability, scalability, and flexibility
A service-repository pattern is a low-level design pattern mainly used to separate the concerns in a software project. It comprises two layers i.e; a service layer and a repository layer.
In practice, the Repository Pattern is commonly used within the Service Pattern to handle data retrieval and persistence. Services can utilize repositories to fetch data from the data source, perform any necessary transformations or validations, and then pass the data to other services or back to the user interface.

Service repository patterns can be broken down into different layers, such as the controller, repository, and service:
Controller: Exposes functionality to external entities
Repository: Stores and retrieves data
Service: Contains business logic

## Code Snippets from project on how routes are managed and linked to services :

[user_routes.py](/app/routers/user_routes.py)

Refer to the line 36 available inside the above user_routes.py file:
 user = await UserService.get_by_id(db, user_id) ==>Line 36
 Code snippet as below.

```
@router.get(
            "/users/{user_id}",
            response_model=UserResponse, name="get_user",
            tags=["User Management Requires (Admin or Manager Roles)"])
async def get_user(
    user_id: UUID,
    request: Request,
    db: AsyncSession = Depends(get_db),
    token: str = Depends(oauth2_scheme),
    current_user: dict = Depends(require_role(["ADMIN", "MANAGER"]))):
    try:
        user = await UserService.get_by_id(db, user_id)
        return UserResponse.model_construct(
            id=user.id,
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
            links=create_user_links(user.id, request)
        )
    except UserNotFoundException as e:
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail=str(e))

```

[user_service.py](/app/services/user_service.py)

Refer the lines 28 till 33 inside the above user_service.py file:
Code snippet as below.

```
@classmethod
    async def get_by_id(cls, session: AsyncSession, user_id: UUID) -> User:
        user = await cls._fetch_user(session, id=user_id)
        if not user:
            raise UserNotFoundException(f"User with ID {user_id} not found.")
        return user
```

[Return back to answer.md](/answer.md)