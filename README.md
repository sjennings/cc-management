# CLAUDE.md Management - Order from Chaos

My personal tools for wrangling large projects with Claude Code. 

Please note I am a very hands-on and opinionated coder, and my version of "vibe coding" involves furiously screaming at the screen while jabbing the escape key to tell the stupid AI how I know better than it does. Sometimes I'm even right.

These files address my personal need for organization and project planning, based on 30 years of working on insanely large projects. (Seriously, MMOs. They get big.)

### Why do this? Why not just ~vibe~, man?

Have you ever watched a Roomba vacuum a room? It will take 20 minutes randomly driving around, running smack into walls and pets, and trying over and over to do a simple task. But it does eventually do it, after an incredible amount of trial and error.

We are turning Claude Code into a Roomba.

Through [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development), we are going to define walls for Claude Code to bang its virtual head on. Specifically, the [Red-Green-Refactor testing model](https://martinfowler.com/bliki/TestDrivenDevelopment.html):

- RED: write a test you know will fail for a feature you haven't written yet.
- GREEN: write some code, pass the test
- REFACTOR: write more code, go back to step RED.

This can be annoying for human engineers who just want to crap out some code, make it sort of work, and go home. But Claude Code can't get annoyed. Red/Green/Refactor is perfect for it - and also the constant series of test/code/test/code helps keep Claude Code on task. Like a Roomba.

Debugging? There's a structured way to teach Claude Code to do this as well. Specifically, the [Five Whys](https://en.wikipedia.org/wiki/Five_whys): drilling down using five questions to analyze the root cause of an issue. Think of Claude Code as a small child asking "why? why? why?" -- and the answers give Claude context necessary (or at least helpful) to solve the issue. (If you think that Five Whys is a dumb methodology to debug with, just don't use the debug command. But I've found Claude does work well with it, especially with issues you might not have a handle on.)

Finally, tying this all together is a basic [project management life cycle](https://en.wikipedia.org/wiki/Systems_development_life_cycle) for any given feature:   

```
Feature Request → Requirements → Sprint Planning → Design → Task Implementation → Review → Next Task
                                                                    ↑                           ↓
                                                                    └───────────────────────────┘
```

Using the commands here, Claude Code will step you through this process, asking you questions as needed to flesh out your plan, and then implement it using the Roomba, er, Test Driven Development methodology, and then reviewing its work and possibly adding new issues that fall out from that review, or looping around to iterate some more.

*This will work.* It plays to Claude Code's strengths: a rarely brilliant, sometimes incredibly brain damaged junior coder that will never, ever run out of patience. Your job is to supervise Claude Code, kick it in the shins when it goes off track (do NOT be afraid to hit Escape and tell it to do something differently), and give it the high level guidance it needs.

## Requirements

### Linear as a project management back-end

https://www.linear.app

Linear MCP server: https://linear.app/changelog/2025-05-01-mcp

To install the MCP server, you'll need to add the following in Claude Code:

```bash
# change -s user to -s project if you don't want the MCP available user-wide
claude mcp add linear -s user -- npx -y mcp-remote https://mcp.linear.app/sse
```

Linear is best described as "Jira that doesn't suck." You may think opening issue tickets for every bug fix is overkill for your project -- it is not. It's, in fact, a way to help fight off Claude Code's well-documented issues with losing context. The commands included here use Linear as a database - storing context in comments and using Linear task IDs. It's free for personal use, so use it.

### Install the context7 MCP server and use it

https://github.com/upstash/context7

Almost a requirement for serious development, it allows Claude Code to query for the current state of library and API documentation for [a dizzying array of platforms and APIs](context7.com).

### Install the Zen MCP server and use it

https://github.com/BeehiveInnovations/zen-mcp-server

It's used by several commands here and gives Claude Code the invaluable ability to get a second opinion.

### Get Claude Max

I make no guarantees that any of this will not immediately blow up your Pro plan or god forbid, API calls you're paying for directly. The 100 plan is fine if you don't mind using Sonnet (which is perfectly fine for coding; Opus' strength is in planning).  Get the 200 plan if you just want to run Opus all day. (Well, Anthrophic seems to be cracking down on this, but Opus is overkill for most non-planning things.)

### FOR THE LOVE OF ALL THAT IS HOLY USE GIT AND CHECKPOINT REGULARLY

I don't know if you've noticed yet but AI is kind of dumb sometimes and Claude Code is not immune to this.

## What's provided here

### CLAUDE.md

After a great deal of back and forth I've come to the conclusion having a large CLAUDE.md file is a good recipe for killing your token allotment. So right now it's very minimal - you will want to populate it with details of your own project.

### Claude Code Commands

IMPORTANT: There are references throughout to PRJ as a project identifier prefix in Linear. You may want to use search and replace to change this to something closer to what you're actually working on. Or don't, I'm not your mom.

### Before you work:

### /util:style-guide

This analyzes your project and researches best coding practices and writes them to your CLAUDE.md file. Be sure to edit them later if it writes something you disagree with. Claude does a pretty good job of following them.

### /util:docs-analyze

This is a small routine that writes out a CLAUDE.md file in the root of your source code directories describing what's there. Another way to add some context to Claude Code's amnesiac goofiness.

### The Plan:

### /project:init-feature (feature description)

This begins the process of working through a new feature. It gathers requirements (from you), and moves on to the next step. Claude should tell you what command (and what Linear issue ID) to enter next when completed.

### /project:sprint-planning (task id)

This takes the requirements defined in the previous step and breaks them down into discrete tasks, using sprint grooming to collect what info it needs from the expert (that'd be you, hoss). Afterwards, it will prompt you to begin work on the first task in the sprint.

### /project:do-task (task id)

This actually implements the code. We had to get here sometime. After it's done (running through a ton of tests in the process) Claude Code will prompt you to move on to the review step.

### /project:review (task id)

This runs a code review on what's been done so far. If there's some catastrophic errors at this point, it'll try to fix them. If it has suggestions, it will prompt you to create new tasks for them (or create them itself if it thinks they're important enough). If everything else looks good, this is the point where the Linear task is closed out, and Claude Code will prompt you to move on to the next do-task command.

### /project:debug (thing that isn't working right)

This will create a linear task ID, interrogate the code using Five Whys methodolgy for why something isn't working right, and then try to fix it. Due to the nature of debugging, you'll have to go back and forth with Claude Code most likely.

The other files included are used for context and subtasking.  These are all regular markdown files. Edit to suit.

## If That's All There Is, My Friends, Then Let's Keep Dancing: Expansion

If /project:init-feature looks a bit sparse, it's because I removed instructions for code and component scaffolding that were specific to the projects I work on. You'll want to add those at that point. Claude Code itself can be an excellent assistant here: ask it to analyze your current project(s) and suggest methods for code scaffolding with new tasks suitable for addition in the init-feature.md file.

I also have a /test folder in .claude/commands which helps with automated testing, and need to make it more useful and less tied to my specific workflow before updating this repo with it. Or you could add your own!

If you come up with interesting additions, please share them in [the ClaudeAI reddit.](https://www.reddit.com/r/ClaudeAI/) It's been invaluable to me in whipping Claude Code to this point, so please do share the knowledge you discover if you find any of this of use.

If you want more unrelated opinions like these, I post on BlueSky quite often as [@brokentoys.social](https://bsky.app/profile/brokentoys.social). Note: I'm feisty and political. (you never would have guessed from reading this)
