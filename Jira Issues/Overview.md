

```ad-info
collapse: closed

These queries are all written in [JQL](https://support.atlassian.com/jira-service-management-cloud/docs/use-advanced-search-with-jira-query-language-jql/).

Everything here should work outside of the box, except for [[Overview#Open Reviews]]. You will have to chagne the line `"Team[Select List (multiple choices)]" = "Scrum of the Earth (L2L/Bugs)"` to whatever your team name is. Also if you want senior reviews or second peer reviews, you'll have to make the JQL yourself (I am lazy).
```

### Cards ToDo
```jira-search
type: TABLE
query: assignee = currentUser() AND resolution = EMPTY AND sprint in openSprints()
columns: KEY, SUMMARY, -TYPE, -PRIORITY, -$10022, STATUS,
```

### Review Assigned To Me
```jira-search
type: TABLE
query: "Peer Code Reviewer[User Picker (single user)]" = currentUser() AND status = "In Code Review"
columns: KEY, SUMMARY, -TYPE, -PRIORITY, STATUS,
```

### Open Reviews
```jira-search
type: TABLE
query: "Peer Code Reviewer[User Picker (single user)]" = EMPTY AND assignee != currentUser() AND status = "In Code Review" AND "Team[Select List (multiple choices)]" = "Scrum of the Earth (L2L/Bugs)"
columns: KEY, SUMMARY, -TYPE, -PRIORITY, ASSIGNEE, 
```

### My Cards
```jira-search
type: TABLE
query: creator = currentUser() AND sprint in openSprints() AND resolution = EMPTY
columns: KEY, SUMMARY, -TYPE, -PRIORITY, STATUS
```

### Watching
```jira-search
type: TABLE
query: watcher = currentUser() AND resolution = Unresolved ORDER BY Updated
columns: KEY, SUMMARY, -TYPE, -PRIORITY, UPDATED, STATUS,
```

### Done!
```jira-search
type: TABLE
query: assignee = currentUser() AND resolution != EMPTY AND sprint in openSprints()
columns: KEY, SUMMARY, -TYPE, -PRIORITY, -$10022, STATUS,
```

