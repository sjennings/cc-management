# CLAUDE.md Management - Order from Chaos

My personal tools for wrangling large projects with Claude Code. 

Please note I am a very hands-on and optinionated coder, and my version of "vibe coding" involves furiously screaming at the screen while jabbing the escape key to tell the stupid AI how I know better than it does. Sometimes I'm even right.

These files address my personal need for organization and project planning, based on 30 years of working on insanely large projects. (Seriously, MMOs. They get big.)

## Requirements

### Install the context7 MCP server and use it

https://github.com/upstash/context7

Almost a requirement for serious development, it allows Claude Code to query for the current state of library and API documentation for [a dizzying array of platforms and APIs](context7.com).

### Install the Zen MCP server and use it

https://github.com/BeehiveInnovations/zen-mcp-server

It's used by several commands here and gives Claude Code the invaluable ability to get a second opinion.

### Get Claude Max

I make no guarantees that any of this will not immediately blow up your Pro plan or god forbid, API calls you're paying for directly. The 100 plan is fine if you don't mind using Sonnet (which is perfectly fine for coding; Opus' strength is in planning).  Get the 200 plan if you just want to run Opus all day. 

### FOR THE LOVE OF ALL THAT IS HOLY USE GIT AND CHECKPOINT REGULARLY

I don't know if you've noticed yet but AI is kind of dumb sometimes and Claude Code is not immune to this.

## What's provided here

### CLAUDE.md

Has instructions to save the results of Plan Mode (you should always use Plan Mode) as a markdown file in your docs folder. In addition to being required for sprint planning, it's useful in general - when your context gets full, clear it and reload it from your plan mode file. Also includes the structure used by the /plan command.

If you don't have a foul mouth like I do, remove the last line. Instead tell it to tell you you're a cool cat or something, I dunno. What experienced coder doesn't curse like a sailor? (And I've actually been a sailor, I'm an authority on this.)

### Claude Code Commands

### /style-guide

This analyzes your project and researches best coding practices and writes them to your CLAUDE.md file. Be sure to edit them later if it writes something you disagree with. Claude does a pretty good job of following them.

### /plan

(Adapted from [this reddit post](https://www.reddit.com/r/ClaudeAI/comments/1ljv2kz/tips_for_developing_large_projects_with_claude/))

This is a fairly complete project management plan that will step through the project management process, build out sprints, interrogate you on your priorities and decisions for them, and build implementation documentation. Very thorough. Will ask you for a plan created from a Plan Mode session to begin.

### /code-review

Analyze your recent work. Then defend the choices you (or Claude Code) made. 

The discussion is with a senior/CTO who thinks he's John Carmack. Edit if you don't want to talk to John Carmack (I don't blame you, he's kinda a jerk) or your feelings are easily hurt.


These are regular markdown files. Edit to suit.
