sequenceDiagram
    participant Customer
    participant Channel
    participant Adapter
    participant Orchestrator
    participant IntentSvc as Intent Service
    participant Flow as Flow Engine
    participant MenuSvc as Menu Service

    Customer->>Channel: "Show me the menu"
    Channel->>Adapter: Convert to OrchestrateRequest
    Adapter->>Orchestrator: POST /orchestrate

    Note over Orchestrator: Step 1<br>Intent Detection
    Orchestrator->>IntentSvc: classify(text)
    IntentSvc-->>Orchestrator: intent = "menu"

    Note over Orchestrator: Step 2<br>Call Flow Engine
    Orchestrator->>Flow: FlowRequest(intent="menu")

    Note over Flow: Step 3<br>Call Menu Service
    Flow->>MenuSvc: ServiceEnvelope(action="get_menu")

    Note over MenuSvc: Step 4<br>Return structured menu
    MenuSvc-->>Flow: SuccessResponse(data = menu structure)

    Note over Flow: Step 5<br>Create user reply
    Flow-->>Orchestrator: reply_text = formatted menu

    Orchestrator-->>Adapter: OrchestrateResponse(reply_text)
    Adapter-->>Customer: "Here is the menu..."
