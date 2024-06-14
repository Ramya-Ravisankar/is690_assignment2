
19. **Explain how route parameters are used in FastAPI. Provide an example of a route that takes a parameter and demonstrate how it is used within the endpoint.**

###  Answer
FastAPI is a modern web framework for building APIs with Python. Path parameters are one of the ways to handle dynamic routes in FastAPI. They allow you to define parts of the URL as variables, which can then be accessed within your API route functions.Route parameters in FastAPI allow you to capture values from the URL and use them within your API route functions. These parameters are part of the URL path and can be used to define dynamic routes that respond to specific patterns.

Route parameters are variable parts of a URL segment that can be used to pass extra information to a route. They capture values specified at their position in the URL and populate them in an object. The name of the route parameter in the path is used as the key for each captured value in the object.

Route parameters are named URL segments that are used to capture the values specified at their position in the URL. The captured values are populated in the req. params object, with the name of the route parameter specified in the path as their respective keys.

## How Route Parameters Work :
1) Definition: Route parameters are defined within curly braces {} in the path.
2) Type Hints: By using Python's type hints, FastAPI can automatically convert and validate these parameters.
3) Usage: The captured parameters are passed to the route handler functions as arguments.

## Code Snippet:
[user_routes.py](/app/routers/user_routes.py)

Example 1:
In the above file user_routes.py refer to the lines 94 and 104, for endpoint - delete user.
Here the user id the route parameter which is passed to the to delete_user , which in turn is passed to the delete method of the user service class inorder to remove the user from the database.

line 94 - "/users/{user_id}",
line 104 -  await UserService.delete(db, user_id)

```
@router.delete(
        "/users/{user_id}",
        status_code=status.HTTP_204_NO_CONTENT,
        name="delete_user",
        tags=["User Management Requires (Admin or Manager Roles)"]
        )
async def delete_user(
    user_id: UUID,
    db: AsyncSession = Depends(get_db),
    current_user: dict = Depends(require_role(["ADMIN", "MANAGER"]))):
    try:
        await UserService.delete(db, user_id)
    except UserNotFoundException as e:
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail=str(e))
```

Example 2:
In the above file user_routes.py refer to the lines 26 and 36, for end point get_user.
line 26 - The {user_id} for get users
line 36 - Input parameter taken which in turn is passed to the get_by_id() function.
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
[Return back to answer.md](/answer.md)