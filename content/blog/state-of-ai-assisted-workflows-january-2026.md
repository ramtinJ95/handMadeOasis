+++
title = "State of AI Assisted Workflows January 2026"
date = "2026-01-01T25:41:48+01:00"
draft = true
#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["ai agents","developer tooling","generative ai","software engineering",]
+++

Last time I wrote about my AI assiteed workflows was 3 months ago. It's quite
paradoxical but I feel like not much has changed but everything has changed at
the same time. Many of the techniques and methods I was using 3 months ago where
new to me and as I described in that post it felt like a frankenstein type of
workflow with many different systems silver taped together into something. That
something I, in hindsight, think of as the prototype. Now what I have is much
more refined and streamlined, and I think that is the reason for why it feels
like nothing has changed but at the same time everything has changed. Alright
lets get more concerete and into the actual stuff im doing

## What I'm currently doing
Day to day I use only CLI tools when it come to agentic coding. The CLI tools I
use daily are currently Opencode and Claude Code. There is really not much
difference between my workflows for these 2 tools currently, I have managed to
replicate the "infrastructure" I like in both. The only reason I have started to
use Claude Code is because they did not like that people where using their
Claude Max subscriptions with Opencode and started messing with requests from
other harnesses. So when I use opus 4.5 I use Claude code, its as simple of a
reason as that really. I still much prefer Opencode though.

The "infrastructure" mentioned above consists of the following. I have a set of
subagents that I call upon, often directly, depending on the situation. The most
used ones are a research agent, review agent, test agent, ui test agent,
documentation agent and explore agent. These agents are then enriched with
different tools and permissions suitable for their focus. For example the
research agent is the only one with access to context7 and gh-grep mcp's, the
others do not get their context's polluted with the tool descriptions from those
mcps. 

Then I have quite a few custom /commands that i make use of to either prime the
context of an agent or give a specific set of instructions that I have found
myself repeting often. In these commands I often tell it to use specific skills
or subagents, and also have extra instructions around how I want certain things
done. I have found this approach works way better than having these instructions
in the global AGENTS.md file or in a project specific AGENTS.md file. I firmly
belive that those types of files that are always included in the context
regardless of which subagent is spun up or what the task at hand is, should be
kept extremly light and barebones otherwise they hurt way more than they help. 



### Whats working really well

### What did not work well
ralph pluging in claude code.
opencode running playwright mcp.
Skills.
Compaction in opus 4.5
## What I want to explore more
Local LLMs for task
Voice inputs
Building an agent harness for different use cases. Meaning moving beyond
subagents into even more specific and narrow harnesses for task at hand.

## Suggested guidelines compressing all my learnings
