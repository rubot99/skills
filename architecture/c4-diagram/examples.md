# Worked Examples

One example at each level, all describing the same fictional product —
**TaskFlow**, a small project-management web app — so you can see how the
same elements evolve as you zoom in. Every example follows every rule in
`c4-notation.md`; use them as a reference for syntax and for how much detail
is appropriate at each level, not as content to copy verbatim.

## System Context

```mermaid
C4Context
    title System Context diagram for TaskFlow

    Person(user, "TaskFlow User", "A team member who creates and tracks tasks")
    Person_Ext(auditor, "Compliance Auditor", "Third-party auditor who reviews activity logs")

    System(taskflow, "TaskFlow", "Lets teams create, assign, and track tasks")

    System_Ext(emailGateway, "Email Gateway", "SendGrid - transactional email delivery")
    System_Ext(ssoProvider, "SSO Provider", "Okta - identity and single sign-on")

    Rel(user, taskflow, "Creates and manages tasks using", "HTTPS")
    Rel(auditor, taskflow, "Reviews audit logs via", "HTTPS")
    Rel(taskflow, emailGateway, "Sends notification emails via", "HTTPS/API")
    Rel(taskflow, ssoProvider, "Authenticates users via", "OIDC")

    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

> **Legend:** dark blue = your system/people, grey = external, lighter blue = deeper zoom level.

Notice: no internals of TaskFlow are shown, both external dependencies are
grey, and every relationship is a verb phrase with a protocol.

## Container

Zooming into TaskFlow itself:

```mermaid
C4Container
    title Container diagram for TaskFlow

    Person(user, "TaskFlow User", "A team member who creates and tracks tasks")

    System_Boundary(taskflow, "TaskFlow") {
        Container(webApp, "Web Application", "React SPA", "Lets users manage tasks in the browser")
        Container(apiApp, "API Application", "Node.js / Express", "Handles task CRUD, auth, and notifications")
        ContainerDb(db, "Task Database", "PostgreSQL", "Stores tasks, projects, and users")
        ContainerQueue(queue, "Notification Queue", "Amazon SQS", "Buffers outgoing notification jobs")
    }

    System_Ext(emailGateway, "Email Gateway", "SendGrid - transactional email delivery")
    System_Ext(ssoProvider, "SSO Provider", "Okta - identity and single sign-on")

    Rel(user, webApp, "Uses", "HTTPS")
    Rel(webApp, apiApp, "Makes API calls to", "HTTPS/JSON")
    Rel(apiApp, db, "Reads from and writes to", "SQL/TCP")
    Rel(apiApp, queue, "Publishes notification jobs to", "AMQP")
    Rel(apiApp, ssoProvider, "Authenticates users via", "OIDC")
    Rel(queue, emailGateway, "Triggers email send via", "HTTPS/API")

    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

> **Legend:** dark blue = your system/people, grey = external, lighter blue = deeper zoom level.

Notice: `System_Boundary` groups TaskFlow's own containers with a dashed box
and no fill, external systems stay grey and outside the boundary, and IDs
(`user`, `emailGateway`, `ssoProvider`) are reused unchanged from the Context
diagram.

## Component

Zooming into the API Application container:

```mermaid
C4Component
    title Component diagram for TaskFlow API Application

    Container(webApp, "Web Application", "React SPA", "Lets users manage tasks in the browser")

    Container_Boundary(apiApp, "API Application") {
        Component(authController, "Auth Controller", "Express router", "Handles login and session validation")
        Component(taskController, "Task Controller", "Express router", "Handles task CRUD requests")
        Component(notificationService, "Notification Service", "Node module", "Builds and enqueues notification jobs")
        Component(taskRepository, "Task Repository", "Node module", "Encapsulates all task database access")
    }

    ContainerDb(db, "Task Database", "PostgreSQL", "Stores tasks, projects, and users")
    ContainerQueue(queue, "Notification Queue", "Amazon SQS", "Buffers outgoing notification jobs")
    System_Ext(ssoProvider, "SSO Provider", "Okta - identity and single sign-on")

    Rel(webApp, authController, "Calls", "HTTPS/JSON")
    Rel(webApp, taskController, "Calls", "HTTPS/JSON")
    Rel(authController, ssoProvider, "Validates tokens via", "OIDC")
    Rel(taskController, taskRepository, "Reads/writes tasks via")
    Rel(taskController, notificationService, "Triggers on task changes")
    Rel(taskRepository, db, "Reads from and writes to", "SQL/TCP")
    Rel(notificationService, queue, "Publishes jobs to", "AMQP")

    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

> **Legend:** dark blue = your system/people, grey = external, lighter blue = deeper zoom level.

Notice: only the *one* container being zoomed into (`apiApp`) is broken into
components; its sibling containers (`webApp`, `db`, `queue`) appear as
un-expanded boxes for context, at their own Container-level color — this is
what keeps a Component diagram readable instead of dumping the whole
system's internals into one picture.
