```mermage
flowchart TD
    subgraph Client_Layer["ผู้ใช้งาน (Client Layer)"]
        A[Web Browser]
        B[Mobile App]
    end

    subgraph Edge_Layer["ชั้นนอกสุด / ทางเข้า (Edge/Gateway Layer)"]
        C[Managed Cloud API Gateway\n- Routing\n- Security\n- AWS/GCP Managed]
    end

    subgraph App_Layer["ชั้นประมวลผลหลัก (Application Layer)"]
        D[Serverless Function\n(Logic หลัก)\n- AWS Lambda\n- GCP Cloud Functions]
    end

    subgraph Data_Layer["ชั้นข้อมูล (Data Layer)"]
        E[Managed Database\n- Aurora Serverless\n- Small RDS/Cloud SQL]
    end

    subgraph Async_Layer["ชั้นประมวลผลเบื้องหลัง (Optional)"]
        F[Managed Queue\n- AWS SQS]
        G[Async Worker Function\n- AWS Lambda]
    end

    %% Normal Flow
    A -->|Request| C
    B -->|Request| C
    C -->|Route| D
    D -->|Read/Write| E
    E -->|Data| D
    D -->|Response| C
    C -->|Response| A
    C -->|Response| B

    %% Async Flow
    D -->|Send Task| F
    F -->|Trigger| G
    G -->|Process| E
```
