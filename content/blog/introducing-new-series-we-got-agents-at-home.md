+++
title = "Introducing New Series We Got Agents at Home"
date = "2025-11-23T21:50:01+01:00"
draft = true

tags = ["ai agents","generative ai","software engineering", "developer tooling"]
+++

This is the introduction to a new series I'm calling "We got agents at home".
The main idea of the series is very simple: I want to share my journey of
creating a capable coding agent from scratch without using any agent framework.
But the motivation and inspiration for making it into a series and actually
committing to going from 0 to a capable coding agent that can make use of
subagents, context pruning, custom tooling, MCPs and other table stakes
features such as /commands, custom agent definitions together with a good looking
TUI is a bit less simple. 

I have been thinking of creating a series like this for a while, but until
recently for some reason something was stopping me. However, a few days ago I saw
a comment on Hacker News describing the process of building your own agent as
creating your own lightsaber, and as someone who grew up really enjoying Star
Wars, that comment really set the right gears in motion for me, so I finally bit
the bullet and started this series. 

If the lightsaber comment was the motivation that got the wheels turning, the
inspiration and discipline that will keep it turning is the fact that I have
been on a "build it yourself" kick ever since I fully switched my main OS to
Linux. I don't know if it's part of the "culture" of Linux or me maturing as a
software engineer, but building my own tooling or at least having a deep
understanding of the tools I use frequently is becoming quite important to me.

The understanding part is also going to be a major part of this new series. The
idea is not that we will create something that can compete with Claude Code or
OpenCode et al, but rather the purpose is to demystify and bring clarity around
what's going on under the hood. This point is the reason for the name of the
series—it's from the "we have x at home" meme. My aim is to show that there is 
no magic, it's all good ol' programming still.

The third major contributor/reason for creating this series is that we will
use it as a base for the future where I will create some non-coding agents as
forks from the main series, or side quests. On these side quests we will try out
crazy ideas or some new ideas that pop up in the community. This is to allow for
some creative exploration of agents, because I strongly believe that the
bottleneck for creating interesting and useful agents is creativity, and also to
be fair, this entire problem space is very under-explored in my opinion. Which is
super fun!

## What to expect from this series
We will write a coding agent from scratch using no agent framework because I'm
going to explain every line of code to the best of my ability so we know what is
going on at each step. At the end of the series I imagine our coding agent will
have the following features/capabilities:
* Tool calling
* Slash commands
* Running code in a safe way
* Context management/ context pruning
* Subagent being used by the primary agent
* Ability to use MCPs
* Possibility to read agent.md files to define different primary and subagents

At some point in this journey, probably pretty early on we will have to create a
nice CLI or TUI using some kind of framework. But I'm not yet sure if that will
be integrated into this series or if those parts will be optional. We will start
the series with using Python but I may provide alternative implementations in Go
also because I'm currently really enjoying learning and using Go.

The cadence of posts will be at least one new entry to the series weekly on
Friday's.

