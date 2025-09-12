# Zoom Contact Center Engagement Scenarios – Context Switching

This document provides step-by-step test flows for validating how your Zoom App handles **context switching between active engagements** in the Zoom Contact Center (ZCC).  

Mark each checkbox as ✅ (Pass) or ❌ (Fail) as you go.
---
### Prerequisites
* Contact Center License
* Zoom Global Phone Number
* For Web Chat, Zoom Contact Center SDK or
* For SMS, 10DLC enable on your Zoom account 
  
---

## Pre-checks (required for all flows)

- [ ] Agent signed in with **ZCC license** and opted into required queues  
- [ ] App installed and launchable in the ZCC desktop client  
- [ ] App requests capabilities in `zoomSdk.config`:
  - `getRunningContext`
  - `getEngagementContext`
  - `getEngagementStatus`
  - `onEngagementContextChange`
  - `onEngagementStatusChange`  
- [ ] Admin has configured queue concurrency to allow **multiple active engagements** (messaging channels)  
- [ ] Developer console open to observe logs  

---

## Flow A — Voice + SMS

**Trigger**

1. Start **Engagement A (Voice)** → agent answers  
2. Save draft `"voice-note A1"` in your app  
3. Start **Engagement B (SMS)** → agent accepts  
4. Switch focus A ↔ B  
   - On A, use hold/resume or mute/unmute  
   - On B, send/receive SMS and save a draft  
5. End Voice A; remain on SMS B  

**Checklist**

- [ ] Switching triggers `onEngagementContextChange` with correct engagementId  
- [ ] Voice-specific status changes (hold, resume) are logged for A  
- [ ] SMS engagement remains unaffected when voice ends  
- [ ] App continues functioning for SMS after voice ends  

**Pass criteria**

- Mixed channel behavior works correctly  
- Status changes for voice are isolated and do not disrupt SMS  
- No UI errors after voice engagement ends  

---

## Flow B — SMS + Web Chat

**Trigger**

1. Start **Engagement A (Web Chat)** → agent accepts  
2. Save draft `"chat-draft A1"` in your app  
3. Start **Engagement B (SMS)** → agent accepts  
4. Switch focus A ↔ B several times  
5. Update drafts independently for each  

**Checklist**

- [ ] Switch triggers `onEngagementContextChange` with correct engagementId  
- [ ] UI rehydrates correctly for both SMS and Web Chat  
- [ ] Drafts remain isolated between A and B  
- [ ] Closing one engagement does not impact the other  

**Pass criteria**

- Channel differences (SMS vs Web Chat) do not break draft isolation  
- Your UI always reflects the focused engagement’s state  

---

# Overall Pass Criteria

For your app to pass context switching validation:

- [ ] App correctly processes **`onEngagementContextChange`** events  
- [ ] App always displays the **currently focused engagement**  
- [ ] Drafts or partial workflows are preserved per engagementId  
- [ ] **No stale data** leaks between engagements  
- [ ] Ending one engagement does not disrupt others  
- [ ] App handles status transitions (active → wrap-up → end) gracefully  

---
