# A2A communication illustrations

## Azure Copilot interaction with A2A integrated Agent

Below diagrams depicts the Azure copilot integrated agents communication.

```mermaid
sequenceDiagram
    autonumber
    participant User as Copilot User
    participant Client as Copilot Orchestrator
    participant Server as A2A Server
    participant Agent as Partner Agent

    User->>Client: User Prompt

    Client->>Server: Send A2A Message (User intent)
    
    activate Server
    Server->>Agent: User intent
    Agent-->>Server: Agent Response
    
    Server->>Server: Translate Agent Response to A2A Message

    alt Processing Successful
        Server-->>Client: A2A Message (200 OK + Agent Response)
    else Processing Failed
        Server-->>Client: Error Response (4xx / 5xx)
    end

    deactivate Server

    Client-->>User: Copilot Response
```

## Simple A2A Client and Server Interaction

### Synchronous interactions

```mermaid
sequenceDiagram
    autonumber
    participant Client as A2A Client
    participant Server as A2A Server
    participant Agent as AI Agent

    Client->>Server: Get Agent Card
    Server-->>Client: Agent Card

    Client->>Server: Send A2A Message (Query)
    
    activate Server
    Server->>Agent: Query
    Agent-->>Server: Agent Response
    
    Server->>Server: Translate Agent Response to A2A Message

    alt Processing Successful
        Server-->>Client: A2A Message (200 OK + Agent Response)
    else Processing Failed
        Server-->>Client: Error Response (4xx / 5xx)
    end

    deactivate Server
```

### Asynchronous interactions

```mermaid
sequenceDiagram
    autonumber
    participant Client as A2A Client
    participant Server as A2A Server
    participant Agent as AI Agent

    Client->>Server: Get Agent Card
    Server-->>Client: Agent Card

    Client->>Server: Send A2A Message (Query)
    
    activate Server

    Server-->>Client: Agent Task (Id:XXX)

    Server->>Agent: Query

    Client->>Server: Get Agent Task (Id:XXX)
    Server-->>Client: Agent Task (Id:XXX, Progress Details)

    Agent-->>Server: Agent Response
    
    Server->>Server: Translate Agent Response to Agent Task

    Client->>Server: Get Agent Task (Id:XXX)
    alt Processing Successful
        Server-->>Client: Agent Task (200 OK + Agent Response)
    else Processing Failed
        Server-->>Client: Error Response (4xx / 5xx)
    end

    deactivate Server
```
