<%*
const formatTitle = (title) => {
	return title.replace(":", " -").replace("|", "-").replace("/", "-")
}

var id = tp.file.title.match(/[0-9]+/)
var issueKey = `WB-${id}`
var issue = await $ji.base.getIssue(`${issueKey}`)
var assignedSelf = (await $ji.base.getLoggedUser()).displayName === issue.fields.assignee?.displayName
console.log(assignedSelf)
var isDemo = issue.fields.issuetype.name == 'ER' || issue.fields.issuetype.name == 'Story'
var isReview = issue.fields.issuetype.name == 'Peer Review' || issue.fields.issuetype.name == 'Senior Review'
try {
	await tp.file.rename(`${issueKey} - ${formatTitle(issue.fields.summary)}`)
} catch (e) {
	await tp.file.rename(`${issueKey}`)
}
// Relation Builder

//var file = engine.app.workspace.getActiveFile();const formatTitle = (title) => {;	return title.replace(':', ' -').replace('|', '-').replace('/', '-');};var ji = engine.getPlugin('obsidian-jira-issue');var id = file.basename.match(/[0-9]+/);var issueKey = `WB-${id}`;var issue = await $ji.base.getIssue(`${issueKey}`);
var relationObj = {}
var relationString = ''
for (i in issue.fields.issuelinks) {
	const relation = issue.fields.issuelinks[i]
	var relationIssue = relation.outwardIssue ? relation.outwardIssue : relation.inwardIssue
	var relationKey = relationIssue.key
	var relationLink = `${relationKey} - ${formatTitle(relationIssue.fields.summary)}|${relationKey}`
	if (relationObj[relation.type.inward]) {
		(relationObj[relation.type.inward]).push(relationLink)
	} else {
		relationObj[relation.type.inward] = [relationLink]
	}
}
for (const [type, issues] of Object.entries(relationObj)) {
	relationString += `${type}:\n`
	for (i in issues) {
		const issue = issues[i]
		relationString += `  - '[[${issue}]]'\n`
	}
}
const metaString = `---\n\
aliases: [${issueKey}]\n\
type: ${issue.fields.issuetype.name}\n\
tags:\n  - ${issue.fields.issuetype.name.replace(' ', '_')}\n  - tasks\n\
${relationString}\
---`
//engine.app.vault.read(file).then( (content) => {engine.app.vault.modify(file, content.replace(/---.*?---/s, metaString))})
%>
<%* tR = metaString %>

```meta-bind-button
style: primary
label: Refresh Metadata
action:
  type: inlineJS
  code: "var file = engine.app.workspace.getActiveFile();const formatTitle = (title) => {;	return title.replace(':', ' -').replace('|', '-').replace('/', '-');};var ji = engine.getPlugin('obsidian-jira-issue');var id = file.basename.match(/[0-9]+/);var issueKey = `WB-${id}`;var issue = await $ji.base.getIssue(`${issueKey}`);var relationObj = {};var relationString = '';for (i in issue.fields.issuelinks) {;	const relation = issue.fields.issuelinks[i];	var relationIssue = relation.outwardIssue ? relation.outwardIssue : relation.inwardIssue;	var relationKey = relationIssue.key;	var relationLink = `${relationKey} - ${formatTitle(relationIssue.fields.summary)}|${relationKey}`;	if (relationObj[relation.type.inward]) {;		(relationObj[relation.type.inward]).push(relationLink);	} else {;		relationObj[relation.type.inward] = [relationLink];	};};for (const [type, issues] of Object.entries(relationObj)) {;	relationString += `${type}:\n`;	for (i in issues) {;		const issue = issues[i];		relationString += `  - '[[${issue}]]'\n`;	};};const metaString = `---\naliases: [${issueKey}]\ntype: ${issue.fields.issuetype.name}\ntags:\n  - ${issue.fields.issuetype.name.replace(' ', '_')}\n  - tasks\n${relationString}---`;engine.app.vault.read(file).then( (content) => {engine.app.vault.modify(file, content.replace(/---.*?---/s, metaString))})"
```

```jira-search
type: TABLE
query: id = <%* tR += `WB-${id}` %>
columns: KEY, SUMMARY, -TYPE, -PRIORITY, -$10022, STATUS
```

<hr>

# Description
<%* tR += issue.fields.description.replace(/\#\s/gm, "## ") %>

<hr>

# Notes

<Hr>

## Checklist

<hr>
<%* if (assignedSelf) { %>
# Workflow Checklist

- [ ] Initial Work (Applied Standards) [notask:: true]
	- [ ] Associations/entities documented [notask:: true]
	- [ ] Microflows Documented [notask:: true]
		- [ ] New Microflows [notask:: true]
		- [ ] Old Microflows [notask:: true]
- [ ] Commit to local branch [notask:: true]
- [ ] Peer review pass [notask:: true]
	- [ ] Create peer review card [notask:: true]
- [ ] Senior review pass [notask:: true]
	- [ ] Create senior review card [notask:: true]
- [ ] Final Work [notask:: true]
	- [ ] Push to sprint [notask:: true]
		- Name = `<%* tR += `${issueKey} - ${issue.fields.summary}` %>`
<%* if (!isDemo) { tR += `\t- [ ] Mark [card](https://evosus.atlassian.net/browse/MX-${id}) as "Demo/QA Acceptance" [notask:: true]` } %>
	- [ ] Jira communication [notask:: true]
<%* if (isDemo) { tR += `\t- [ ] Demo the card [notask:: true]` } %>
<%* if (isDemo) { tR += `\t- [ ] Mark done after feedback [notask:: true]` } %>
- [ ] Done

<hr>
### Jira communication

<%* if (!isDemo) { %>
```
Merged to _Sprint.0323:
[<%* tR += issueKey %>](<%* tR += `https://evosus.atlassian.net/browse/WB-${id}` %>) - <%* tR += issue.fields.summary %>
```
<%* } else { %>
```
Merged to _Sprint.0323:
*[<%* tR += issueKey %>](<%* tR += `https://evosus.atlassian.net/browse/WB-${id}` %>) - <%* tR += issue.fields.summary %>*
```
<%* } %>
<%* } %>