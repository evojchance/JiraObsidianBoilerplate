Heyo, here I will show you some features and setup instructions on how to navigate this software and the custom scripting that is in this boilerplate!

In short, Obsidian is a glorified and highly customizable markdown/HTML editor/viewer. You can read more about all the cool core features [on their website help page](https://help.obsidian.md/Home). You can learn those yourself, I will focus on the custom features mostly.

<hr>

```table-of-contents
title: Table of Contents
style: nestedList # TOC style (nestedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 0 # Include headings up to the specified level
includeLinks: true # Make headings clickable
debugInConsole: false # Print debug info in Obsidian console
```

<hr>

# Setup

You need to be able to communicate with your Jira account.
Navigate to **Settings > Jira Issue (Under community plugins section) > Accounts > Edit Default

![[Pasted image 20240411095710.png]] ![[Pasted image 20240411095757.png]]

The default settings should look like this:

![[Pasted image 20240411100204.png]]
Next, fill out your Slack email address (1) and your API Token.
The API token can be found by going into your Jira **Account settings (Manage Account) > Security (Tab at the top) > Create and Manage API Tokens**

When you create a new token, make sure to copy it down and put it in field (2).
For more information about authentication, [view the plugin docs](https://marc0l92.github.io/obsidian-jira-issue/docs/configuration/authentication)

And you are done! Everything is now ready to go!

<hr>

# Usage

## Issue Template

The main power behind this boilerplate, is the templates, which use several plugins combined to create and automated way of viewing and managing Issues.

```ad-attention
collapse: closed

Depending on what project you want to automatically work with, you must change the templates project identifier (Default is `WB`)

Near the top of [[@ Issue Template|the issue template]], change variable `issueKey` to use the correct prefix if not using `WB`.
```

To import an issue you are working on into Obsidian:
1) create a new note (Probably in `Jira Issues/Issues` for organization, but it will work anywhere).
2) Name the note the funny little numbers that identify the issue in Jira.
3) Run `@ Issue Template` template on the note.

![[Pasted image 20240411104054.png]]

And voilà, you imported your issue and can get working on it!

### Features

- Renames the file to the issue name/summary
- Creates and tags the file based on the type of issue (Great for organizing)
- All issue relations are listed and connected
	- Works with the graph viewer very well, showing a web of all related issues (See "Issue Graph Examples" below)
- Metadata refresh button.
	- If anything in the metadata changes, use this button to refresh that section with the live data.
- Issue quick information
- Imported description
	- Atlassian software uses their own weird and cringe text formatter, so translating that to markdown sucks. Feel free to contribute to updating the template text translation and create a PR
- User notes section
- User checklist section
- Auto-populated workflow checklist.
	- Changes based on if you should demo the card or not.
- Jira communication section
	- I usually put my "Notes to QA" here as I am working on a card.
	- Has a nice generated codeblock which you can copy/past into Slack `lou_qa` channel when you push the card.
- Assignment detector
	- If you are just not assigned the card, extra sections are left out as to not add clutter.
	- Removes "Workflow checklist" and "Jira communication" sections
	- Good for if you are just reviewing a card.

And note, ***everything*** here is fully customizable via the template file. All code is there, change it to suit your needs/formatting!

```ad-example
collapse: closed
title: Issue Graph Examples

![[Pasted image 20240411104842.png]] ![[Pasted image 20240411104928.png]]
```

View the example generated note [[WB-946 - Performance - AutoCompleteForMendix Widget Deprecation|here]]

## Daily Notes Template

Daily notes are my way to start the day. A customizable experience to show you your tasks during the day, what you've accomplished, and much more information! To create or go to the current day's note, just click the "Open today's daily note" in the sidebar.
![[Pasted image 20240411110801.png]]

### Features

- Folder structure atomization.
	- Creates new daily notes in an easy to navigate format, separated via year/month folders.
- Accomplishments to show you what you've accomplished today.
- Daily tasks
	- General morning/evening routine checklist with links
- Todo list
	- Compiles all unfinished tasks into one view, for easy overview on your day.
	- For more information on how this works, read the [[README (in Obsidian)#Tasks|tasks]] section.
- Shows current cards in development.
- Todo section for custom items.

As always, ***everything*** here is customizable.

Check out the example note [[2024-04-11|here]].

Tips:
- You can automatically open the daily note on startup by going to Settings > Daily Notes > Open daily note on startup

## Tasks

The "Tasks" plugin is used all over the place with the templates to display useful information on tasks you should complete.

When you generate an issue note assigned to you, in the "Workflow checklist" section, there is a "Done" checkbox which will show up in your todo list in the daily note.

In order for a checkbox to show in the "todo" list, it must either be located somewhere in the "Journal" folder, or have the tag #tasks somewhere on the page. anything labeled "notask" won't show.

For more information on this plugin, check out the webpage for [tasks](https://github.com/obsidian-tasks-group/obsidian-tasks) and [dataview](https://github.com/blacksmithgu/obsidian-dataview).

## Folder Structure & Workspace

The default folder structure is simple, the only thing to explain is the "Issues" folder.
Here's some ideas on how you can organize your structure:

- "Archive" folder for cards that are done, or will no longer have work done on them.
- "Reference" folder for cardss you won't be working on, but are good to have for reference, like to expand the issue graph.
- "Reviewing" for cards you are reviewing.

Along with a proper folder structure, you can fully utilize the custom "workspace".
To view this, go to the sidebar and select the "main" workspace.

![[Pasted image 20240411114659.png]]

Here you will see a few things pop up. At the bottom, you can see outgoing/incoming links. When viewing an issue, these show all related issues. Great for navigating relationships in a list style.
On the right, you see a local graph view. When viewing a card, this shows all its relations in a graph view.

<hr>

# Plugin Overview

There are several plugins included. Here's a quick summary of each one and what it can do/is added for:

- [Admonition](obsidian://show-plugin?id=obsidian-admonition)
	- Creates collapsible and customizable dropdown fields. You've seen a few on this page.
- [Advanced Tables](obsidian://show-plugin?id=table-editor-obsidian)
	- Better markdown table auto-formatting
- [Dataview](obsidian://show-plugin?id=dataview)
	- Used to display tasks and todo items.
	- Amazing note query language and embedding of dynamic data. Fully customizable via javascript, and mega powerful for viewing data (hance the name).
- [Excalidraw](obsidian://show-plugin?id=obsidian-excalidraw-plugin)
	- Embedded drawing software. Great for making little mockup designs or writing down notes in a visual way.
- [Image Toolkit](obsidian://show-plugin?id=obsidian-image-toolkit)
	- Makes viewing images easier and full of more features.
- [Jira Issue](obsidian://show-plugin?id=obsidian-jira-issue)
	- Used to get issue data to generate template files and view JQL results from Obsidian.
- [JS Engine](obsidian://show-plugin?id=js-engine)
	- Used for the Issue template "Refresh Metadata" button, to run custom javascript.
- [Meta Bind](obsidian://show-plugin?id=obsidian-meta-bind-plugin)
	- Interactive fields in notes, such as dropdowns, dates, buttons, and more.
	- Used for the "Refresh Metadata" button.
- [QuickAdd](obsidian://show-plugin?id=quickadd)
	- Additional information for templates to use.
- [Tasks](obsidian://show-plugin?id=obsidian-tasks-plugin)
	- See [[#Tasks]]
- [Templater](obsidian://show-plugin?id=templater-obsidian)
	- Custom scripting and data manipulation based around templates.
	- The bread and butter of this vault.

There's also like, a billion different features with each plugin not used here. Play around with them, they are fun :)

<hr>

Do create your own structure, style, and workflow yourself. This boilerplate is simply an example of the power Obsidian can bring.

Have suggestions or feedback, or usage questions about anything Obsidian or this vault, [ask me](https://evosus.slack.com/team/U054KRVDAG5)!
