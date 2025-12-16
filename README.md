graph TD
    A[Firewall/SIEM Trigger] -->|JSON Data| B(Normalize Data)
    B --> C{Enrich via AbuseIPDB}
    C --> D[Risk Scoring Engine]
    
    %% Parallel Fork
    D -->|Log Data| E[(Google Sheets Database)]
    D -->|Alert Logic| F{IF High Risk?}
    
    %% Alert Paths
    F -->|True| G[🚨 Telegram Alert]
    F -->|False| H[End Workflow]
    
    %% Error Handling
    I[Error Trigger] -.->|On Failure| J[🔥 System Admin Alert]
    
    style G fill:#ff9999,stroke:#333,stroke-width:2px
    style E fill:#99ccff,stroke:#333,stroke-width:2px
