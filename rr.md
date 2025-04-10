```mermage
flowchart TD
    subgraph Client_Layer[Client Layer]
        client[ผู้ใช้งาน (Client)<br/>เช่น หน้าเว็บหรือ Mobile Application]
    end

    subgraph Edge_Layer[Edge/Gateway Layer]
        gateway[Managed Cloud API Gateway<br/>เช่น AWS API Gateway, Google Cloud Gateway<br/>จัดการ Routing, Security<br/>ประหยัด Managed Service]
    end

    subgraph App_Layer[Application Layer]
        function[Serverless Function (หลัก)<br/>เช่น AWS Lambda, Google Cloud Functions<br/>Logic หลัก, จ่ายตามการทำงาน]
    end

    subgraph Data_Layer[Data Layer]
        db[Managed Serverless/Small DB<br/>เช่น Aurora Serverless, Cloud SQL<br/>ปรับขนาดได้เมื่อไม่มีโหลด]
    end

    subgraph Async_Layer[Async Processing Layer (ถ้าจำเป็น)]
        queue[Managed Cloud Queue<br/>เช่น AWS SQS, รับ Task]
        async_function[Serverless Function (Async Worker)<br/>Trigger โดย Queue]
    end

    client -->|"ส่ง Request"| gateway
    gateway -->|"Route Request"| function
    
    %% Normal Flow
    function -->|"อ่าน/เขียนข้อมูล"| db
    function -->|"ส่ง Response"| gateway
    gateway -->|"ส่ง Response กลับให้ผู้ใช้"| client

    %% Async Flow
    function -->|"ส่ง Task เข้า Queue"| queue
    queue -->|"Trigger"| async_function
    async_function -->|"ประมวลผล Task<br/>อ่าน/เขียนข้อมูล"| db
```
