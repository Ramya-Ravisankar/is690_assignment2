
14. **What are REST APIs, and how do they function in your project? Provide an example of a REST endpoint from your user management system.**

## Answer:
REST (Representational State Transfer) APIs are application programming interfaces (APIs) that allow different software applications to communicate with each other over a network. They follow specific rules and standards that enable users and applications to access and use data using HTTP requests.

An API is a set of definitions and protocols for building and integrating application software. It’s sometimes referred to as a contract between an information provider and an information user—establishing the content required from the consumer (the call) and the content required by the producer (the response).

## Diff between REST API and RESTful API's :
REST API uses web services and is based on request and response, whereas RESTful API works completely based on REST application and infrastructure. REST apps have strong protocols and pre-configured architecture layers as security measures, whereas RESTful apps have multi-layered transport protocols.

Below is an example of the "Get User" REST endpoint using HTTP GET method from the user management system project:

All the rest APIs are defined in the routers folder in our project.

[APPLICATION_GET_USER_METHOD_REST_API_ENDPOINT](/screenshots/Question14/APPLICATION_GET_USER_METHOD_REST_API_ENDPOINT.png)


[user_routes.py](/app/routers/user_routes.py)

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
The above is a GET Method - REST API which returns us the user details for the given user id.

[APPLICATION_GET_USER_METHOD_REST_API_ENDPOINT](/screenshots/Question14/APPLICATION_GET_USER_METHOD_REST_API_ENDPOINT.png)

[Return back to answer.md](/answer.md)