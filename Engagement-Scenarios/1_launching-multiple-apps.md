# Engagement Scenario — Launching Multiple Apps in an Engagement

This test validates how your app behaves when multiple Zoom Apps are open during the same engagement.  
Your app should still receive engagement status events even if it is not the foreground app at the time of wrap-up and end.

Mark each box as you test. ✅ = Pass, ❌ = Fail.
---
### Prerequisites
* Contact Center License
* Zoom Global Phone Number
* For Web Chat, Zoom Contact Center SDK or
* For SMS, 10DLC enable on your Zoom account 
---

### Trigger

1. Start a **new engagement** (e.g., inbound voice, chat, or SMS) → agent accepts  
2. Launch **App A** (your app)  
3. Launch **App B** (a different Zoom App) while the engagement is active  
4. Switch back and forth between **App A** and **App B**  
5. Keep **App B** in the foreground  
6. End the engagement while **App B** is visible  

---

### Checklist

- [ ] `getRunningContext()` shows **Contact Center** when App A is visible  
- [ ] `getEngagementContext()` and `getEngagementStatus()` work normally when App A is in foreground  
- [ ] Switching to **App B** does **not** trigger any event in App A (no foreground/background events)  
- [ ] Engagement ends while App B is visible  
- [ ] App A (in background) still receives `onEngagementStatusChange` with **wrap-up**  
- [ ] App A still receives `onEngagementStatusChange` with **end**  
- [ ] App A gracefully processes events even though its UI is not visible  

---

### Pass criteria

- Your app continues to receive and process **engagement status events** even when not in the foreground  
- No reliance on foreground/background visibility events (since none are sent)  
- At engagement end, App A’s webview correctly logs **wrap-up → end** even if App B is visible  
- No errors when engagement lifecycle completes outside of your app’s view  

---
