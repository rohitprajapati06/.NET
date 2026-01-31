
# 📊 Monitoring and Troubleshooting Solutions  
## Azure Monitor & Application Insights (AZ-204)

> **Exam:** AZ-204 – Developing Solutions for Microsoft Azure  
> **Skill Area:** Monitor, troubleshoot, and optimize Azure solutions  

---

## 1. Azure Monitor – Overview
Azure Monitor is the centralized monitoring service in Azure that collects, analyzes, and acts on telemetry from applications and infrastructure.

---

## 2. Application Insights
Application Insights is an Application Performance Monitoring (APM) service used to monitor live applications.

---

## 3. Architecture
![Architecture](https://learn.microsoft.com/en-us/azure/azure-monitor/app/media/app-insights-overview/overview.png)

---

## 4. Telemetry Types
- Requests
- Dependencies
- Exceptions
- Performance counters
- Custom telemetry

---

## 5. Enabling Application Insights
```csharp
builder.Services.AddApplicationInsightsTelemetry();
```

---

## 6. Key Features
- Live Metrics
- Application Map
- Failures & Performance
- Availability Tests

---

## 7. KQL Queries
```kql
requests | where success == false
```

---

## 8. Alerts & Sampling
- Metric alerts
- Adaptive sampling

---

## 9. Exam Tips
- Uses KQL
- Part of Azure Monitor
- Best for App Service & Functions

---

## 10. Summary
Critical AZ-204 topic for monitoring and troubleshooting.
