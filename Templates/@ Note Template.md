---
tags: []
---

### Accomplishments

```dataview
TASK
FROM (#tasks or "Journal") and !#notask
WHERE completion = date("{{date}}") and !notask
GROUP BY section
```

<hr>

### Tasks

#### Daily
- Morning
   - [ ] [Clock in](https://app.rippling.com/time-attendance/dashboard/timeclock) [notask:: true]
   - [ ] Check Slack Messages [notask:: true]
   - [ ] [Daily Standup](https://evosus.slack.com/archives/C0RMHG24D) [notask:: true]
   - [ ] [Check Email](https://mail.google.com/mail/u/0/#inbox) [notask:: true]
   - [ ] [Check Calendar](https://calendar.google.com/calendar/u/0/r?pli=1) [notask:: true]
- Evening
   - [ ] [Clock out](https://app.rippling.com/time-attendance/dashboard/timeclock) [notask:: true]
   - [ ] [Sign off](https://evosus.slack.com/archives/C0RMHG24D) [notask:: true]

<%* if (moment().day() == 1) { %>
#### Monday
<%* } else if (moment().day() == 2)  { %>
#### Tuesday
<%* } else if (moment().day() == 3)  { %>
#### Wednesday
<%* } else if (moment().day() == 4)  { %>
#### Thursday
<%* } else if (moment().day() == 5)  { %>
#### Friday
<%* } %>

#### General
```dataview
TASK
FROM (#tasks or "Journal") and !#notask
WHERE !fullyCompleted and !notask
GROUP BY section
```

```jira-search
type: TABLE
query: assignee = currentUser() AND status != "Done" AND sprint in openSprints()
columns: KEY, SUMMARY, -TYPE, -PRIORITY, -$STORY POINTS, STATUS
```

#### ToDo



<hr>