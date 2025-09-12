# Engagement Scenario — Simple Engagement Sequence / App Launch

This test validates that your app can launch inside Zoom Contact Center during an active engagement and correctly handle engagement context and status lifecycle events.

Mark each box as you test. ✅ = Pass, ❌ = Fail.
---
### Prerequisites
* Contact Center License
* Zoom Global Phone Number
---

### Trigger

1. Start a **new engagement** (e.g., inbound voice, video, chat, or SMS) to a queue the agent is opted into  
2. Agent **answers** the engagement  
3. Launch your Zoom App from the ZCC desktop client **after the engagement is already active**  

---

### Checklist

- [ ] On launch, `getRunningContext()` shows **Contact Center**  
- [ ] `getEngagementContext()` returns valid engagementId and queue data  
- [ ] `getEngagementStatus()` shows **active** state after answer  
- [ ] During the call or chat, app receives **onEngagementStatusChange** events (e.g., hold, resume) if triggered  
- [ ] When the engagement disconnects, app receives **wrap-up** status  
- [ ] When the agent completes disposition, app receives **end** status  
- [ ] After `state:end`, app webview still runs long enough to log the event  
- [ ] All agent input is captured **before** the agent ends the engagement  

---

### Pass criteria

- App launches cleanly in ZCC context and retrieves correct engagement context  
- Status transitions follow lifecycle: **active → wrap-up → end**  
- Final `state:end` event is logged even though the UI is no longer visible  
- No missing or stale data at the end of the engagement  

---
