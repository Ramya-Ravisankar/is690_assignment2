
20. **How does FastAPI use Pydantic schemas to generate Swagger documentation? Provide an example from your project where a Pydantic schema is used and show the corresponding Swagger documentation.**

### Answer
FastAPI automatically generates interactive API documentation based on the Python code and type annotations. This documentation includes detailed information about endpoints, request/response models, and parameter descriptions.

FastAPI utilizes Pydantic schemas for Swagger documentation generation.
By defining Pydantic models and using them in our route handlers, FastAPI can automatically create and validate request and response schemas, providing a robust and developer-friendly API documentation experience. This approach ensures your API is both well-documented and reliable, with minimal manual effort required.

In addition to the automatic documentation generation, FastAPI allows developers to customize and enhance the documentation through various options and settings. For example, we can add descriptions to API models, configure security schemes, and customize the documentation’s appearance and layout.

### Benefits of Using Pydantic with FastAPI
1) Automatic Data Validation:
Pydantic models automatically validate the data against the defined schema. This ensures that incoming requests are well-formed and adhere to the expected structure.

2) Type Safety:
Using Pydantic models as type hints in your route functions provides type safety and better IDE support, including autocompletion and type checking.

3) Automatic Documentation:
FastAPI uses the Pydantic models to generate comprehensive API documentation without additional effort. This includes request and response models, parameter types, and example values.

4) Error Handling:
FastAPI handles validation errors gracefully by returning a clear error response indicating what went wrong, making it easier to debug and use the API.

### Pydantic Models and FastAPI
1) Define Pydantic Models:
Pydantic models are used to define the structure and types of request bodies, query parameters, and responses.

2) Annotate Endpoint Functions:
Use these Pydantic models as type hints in your FastAPI route handler functions.

3) Automatic Validation:
FastAPI uses Pydantic to validate incoming requests and ensure they match the defined schema.

4) Automatic Documentation:
FastAPI automatically generates API documentation, including the request and response schemas, based on the Pydantic models.

Here pydantic schema is used in the CreateEvent API endpoint.

### Code Snippet from project - One such example illustrated below
[event_schemas.py](/app/schemas/event_schema.py)

```
class EventBase(BaseModel):
    title: str = Field(..., example="Company Tour", min_length=1)
    description: Optional[str] = Field(None, example="A tour of our company's facilities.")
    start_datetime: datetime = Field(..., example="2023-06-01T10:00:00")
    end_datetime: datetime = Field(..., example="2023-06-01T12:00:00")
    published: bool = Field(default=False, example=True)
    event_type: EventType = Field(..., example=EventType.COMPANY_TOUR)

    @validator('title')
    def validate_title(cls, v):
        if not v.strip():
            raise ValueError("Title must not be empty")
        return v

    @validator('end_datetime')
    def validate_end_datetime(cls, end_datetime, values):
        start_datetime = values.get('start_datetime')
        if start_datetime and end_datetime < start_datetime:
            raise ValueError("End datetime must be after start datetime.")
        return end_datetime

```

```

class EventResponse(EventBase):
    id: uuid.UUID = Field(..., example=uuid.uuid4())
    creator_id: uuid.UUID = Field(..., example=uuid.uuid4())
    created_at: datetime = Field(..., example="2023-05-30T09:00:00")
    updated_at: datetime = Field(..., example="2023-05-30T09:00:00")
    links: List[dict] = Field([], example=[
        {"rel": "self", "href": "/events/{id}"},
        {"rel": "creator", "href": "/users/{creator_id}"}
    ])

```

[event_routes.py](/app/routers/event_routes.py)

```
@router.post("/events/", response_model=EventResponse, status_code=status.HTTP_201_CREATED, tags=["Event Management Requires (Admin or Manager Roles)"], name="create_event")
async def create_event(event: EventCreate, request: Request, db: AsyncSession = Depends(get_db), token: str = Depends(oauth2_scheme), current_user: dict = Depends(require_role(["ADMIN", "MANAGER"]))):
    created_event = await EventService.create(db, event.model_dump())
    if not created_event:
        raise HTTPException(status_code=status.HTTP_500_INTERNAL_SERVER_ERROR, detail="Failed to create event")

    return EventResponse.model_construct(
        id=created_event.id,
        title=created_event.title,
        description=created_event.description,
        start_datetime=created_event.start_datetime,
        end_datetime=created_event.end_datetime,
        published=created_event.published,
        event_type=created_event.event_type,
        creator_id=created_event.creator_id,
        created_at=created_event.created_at,
        updated_at=created_event.updated_at,
        links=create_event_links(created_event.id, request)
    )

```

NOTE - Event Management Requires ADMIN / MANAGER roles.

[event_management_application_snapshot_swaggerdocumentation](/screenshots/Question20/Q20_EventManagement.png)

[Return back to answer.md](/answer.md)