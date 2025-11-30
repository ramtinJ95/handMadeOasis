+++
title = "Introducing New Series: We Got Agents at Home"
date = "2025-11-29T21:50:01+01:00"
draft = true

tags = ["ai agents","generative ai","software engineering", "developer tooling",
"agent at home series"]
+++

Ever since I switched to Linux as my main OS and started building my own
tools, I've had this nagging feeling about the coding agents I use daily. I
understand the principles how LLMs work, how tool calling happens, the
general architecture. But there's a difference between understanding the
concepts and truly knowing what's happening in the implementation.

A few days ago I saw someone on Hacker News compare building your own coding
agent to crafting your own lightsaber. That clicked. Just like a Jedi needs
to understand their weapon intimately, not just conceptually, I want to move
from theoretical understanding to practical mastery.

So I'm building a coding agent from scratch. This series documents that
journey no frameworks, every line explained, all the features you'd expect
(tool calling, subagents, MCPs, the works). I'm calling it "We got agents at
home" because spoiler: there's no magic, just programming we can understand
deeply by building it ourselves.

## What to expect from this series
I will write a coding agent from scratch using no agent framework because
I'm going to explain every line of code to the best of my ability so we know
what is going on at each step. At the end of the series I imagine our coding
agent will have the following features/capabilities:
* Tool calling
* Slash commands
* Running code in a safe way
* Context management/ context pruning
* Exploring caching strategies
* Subagent being used by the primary agent
* Ability to use MCPs
* Possibility to read agent.md files to define different primary and subagents
* Exploring different long term memory solutions

At some point in this journey, probably pretty early on, we will have to
create a nice CLI or TUI using some kind of framework. But I'm not yet sure
if that will be integrated into this series or if those parts will be
optional.

The cadence of posts will be at least one new entry to the series weekly.

## Why this matters
The build-it-yourself ethos isn't just about control. Mainly, it's about
understanding. I've been on this kick of either building my own tooling or
having a deep understanding of what I use daily. I don't know if it's part of
the "culture" of Linux or me maturing as a software engineer, but this has
become quite important to me.

To be clear, the idea isn't that we'll create something that can compete
with Claude Code or OpenCode et al. Rather, the purpose is to demystify and
bring clarity around what's going on under the hood. That's the reason for
the name, it comes from the "we have x at home" meme. My aim is to show that
there is no magic, it's all good ol' programming still.

## Beyond coding agents: side quests
I will also explore creating and using agents for tasks that are not related
to coding. The reason for this is that I actually think that building agents
is a space that is bottlenecked by creativity, and I find this entire
problem space very interesting. It reminds me of creating small little games
when I had just learned the basics of programming and was starting to get
"fluent" with communicating with the computer.

One of the first non-coding agents I will try to build eventually is an agent
that helps organize notes and ideas and integrate them with my existing
personal wiki.

What I cannot create, I do not understand. Time to start creating.
