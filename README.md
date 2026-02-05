# How to run the database
`podman-compose up -d`

# Dependency direction
Blazor → Api → Application → Domain

                                   ↑
                        Infrastructure

# Project Concord.Domain
This project serves the purpose of defining what a meeting is, what availability means, and what rules must never be broken. it should have no dependencies on ASP.NET, EF Core, Hangfire, Blazor, or databases. 

Business concepts which could exist here:
- Meeting
- Participant
- AvailabilitySlot
- TimeRange
- MeetingRequirement
- MeetingSuggestion

Rules and invariants which could exist here:
- Meeting cannot exceed max duration
- Availability slots must not overlap
- TimeRange start < end

Value objects could be
- ParticipantId
- MeetingId
- UtcTimeRange

# Project Concord.Application
This project services to describe what the system can do. It orchestrates domain logic. 

Examples of items belonging to this project could be
- ScheduleMeeting
- SubmitAvailability
- RecalculateSuggestions
- ConfirmMeeting
- IMeetingRepository
- IParticipantRepository
- INotificationService
- ITimeProvider

Other examples of items belonging to this project could be scheduling algorithms
- FindCommonTimeSlots
- RankMeetingSuggestions

# Project Concord.Infrastructure
This is the project who deals with technical infrastructure. 

Examples of items that could belong in this project
- MeetingDbContext
- MeetingEntity
- Configurations
- Migrations

Further it can include repository implementations, hangfire jobs, or external services
- EfMettingRepository
- RecalculateMeetingJob
- SendReminderJob
- EmailService
- SystemTimeProvider

# Project Concord.Api
This project is the translation layer between HTTP requests/reponses and Application. 

Examples of items belonging here are Controllers/Endpoints, DTOs, Validation, Auth, etc.
- POST /meetings
- POST /availability
- GET /meetings/{id}/suggestions
- CreateMeetingRequest
- AvailabilityDto
- MeetingResponse
- FluentValidation
- Authorize/Authentication

# Project Concord.Blazor
This is the human interface to the project.

It collects availability, displays suggestions, handles user interaction, etc.

What belongs in this project is pages and components, view models, and API clients
- AvailabilityEditor.razor
- MeetingSuggestions.razor
- CalendarGrid.razor
- AvailabilityViewModel
- MeetingSuggestionViewMOdel
- MeetinApiClient

# Project Concord.Tests
This serves the purpose of containing tests for each of the other projects. 

It could include:
- Domain tests
    - TimeRangeTests
    - AvailabilityoverlapTests
- Application tests
    - SchedulerFindsCommonSlots
    - RejectsImpossibleMeetings
- Integration tests
    - ApiCreatesMeetingCorrectly

# Project Concord.Experimental
This project can be used for Experimental design, workload testing and metric reporting. 