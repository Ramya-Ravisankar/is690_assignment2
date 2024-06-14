
18. **Provide a code example showing how RBAC is enforced in one of your FastAPI endpoints.**

## Route Parameters and Pydantic Schemas

### Code Snippets:
Create Event End point (GET and POST):
Refer the file - [event_routes.py](/app/routers/event_routes.py)

```
@router.get("/events/{event_id}", response_model=EventResponse, name="get_event", tags=["Event Management Requires (Admin or Manager Roles)"])
async def get_event(event_id: UUID, request: Request, db: AsyncSession = Depends(get_db), token: str = Depends(oauth2_scheme), current_user: dict = Depends(require_role(["ADMIN", "MANAGER"]))):
    event = await EventService.get_by_id(db, event_id)
    if not event:
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail="Event not found")

    return EventResponse.model_construct(
        id=event.id,
        title=event.title,
        description=event.description,
        start_datetime=event.start_datetime,
        end_datetime=event.end_datetime,
        published=event.published,
        event_type=event.event_type,
        creator_id=event.creator_id,
        created_at=event.created_at,
        updated_at=event.updated_at,
        links=create_event_links(event.id, request)
    )

```


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


[Return back to answer.md](/answer.md)