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
mcps. In the same way I actually limit the available tools also to further limit
context pollution. Because you dont want to give the LLM too many options either
because that will just make it more likely that it uses something wrong and so
on.

Then I have quite a few custom /commands that i make use of to either prime the
context of an agent or give a specific set of instructions that I have found
myself repeting often. In these commands I often tell it to use specific skills
or subagents, and also have extra instructions around how I want certain things
done. I have found this approach works way better than having these instructions
in the global AGENTS.md file or in a project specific AGENTS.md file. I firmly
belive that those types of files that are always included in the context
regardless of which subagent is spun up or what the task at hand is, should be
kept extremly light and barebones otherwise they hurt way more than they help.
What this means in practice is that I dont think these files should be used as
long term memory. Instead just include build commands and things like general
file structure of the project. Max 100-150 lines in the project specific
AGENTS.md I think and for the global one maybe max 50 lines. 

The most recent addition to this "infrastructure" that I have made is what is
now known as the "ralph loop". This piece is essentially a script that uses
Claude code or Opencode as unix cli tools and runs them in a loop.

Now how does all of these subsystems combine into something that works well you
may ask. Let's dive into that! Before that though, if you are new to agentic
workflows this can all seem overwhelming. I just want to remind you that this is
something that has evoled over a months and heavy use, this is in no way
something that one should try to replicate. It's good inspiration, hopefully at
least, but you should allow for your own "infrastructure" to grow organically
for the best results I think. 

## Whats working really well
Thinking about subagents as libraries and commands as functions and context as
the memory in the "classical" sense of computers, i.e I view context as an array
of memory that has to be viewed as a precious resource. AGENTS.md should be thought of
as a cache layer. If you see tool call failures, or the same patterns of failing
to build the project or that it has to start calling regex patterns to find
files etc, those are cache misses essentially. But even more expensive in a
sense because they pollute the current context for the task at hand with a bunch
of tool calls and output's that is not relevant at all for the task at hand.
Building on this analogy, I think each task should be viewed as its own program
even. This means that as soon as the task at hand is changing you should start a
new session and reset the context. I have found that this keeps the agent
focused and reduces hallucinations and stops the agent from going "dumb". This
is the same thing as avoiding what is now called as the "dumb zone". This is my
current part of my mental model when working with agentic coding systems at
least and I think it has served me well thus far.

Working from a concept of setting the stage for the agent. This is the second
part of my mental model for working with agentic coding. This idea is what
guided me to be so cautious about what I include in the context. Basically the
idea is that we set the environment for the agent with a goal. By being very
deliberate and conscious about the environment which we put the agent in we can
limit the degrees of freedom it has but at the same time make use of the fact
that these are dynamic agents that can actually do useful things without us
having to give detailed instructions of each step.

Use subagents. If you are not already doing it yourself, I'm pretty sure most
harnesses actaully have started to do this out of the box already. Get
comfortable with them and start using them extensively is my recommendation. The
value unlock of proper use of subagents is just phenomanal. If there is one
thing you really really should do out of all the things I have presented its
this.

Using tests to check my understanding and confirming whats actually been done.
This is about probing the system and making sure that whatever has been produced
matches your mental model of it. Reason for this way of working for me is
because the amount and speed of code generation is so high now that reviewing it
the old way is just not possible for larger features. We also dont want to lose
the connection to the way the system works and the interfaces between different
parts of the code, because the intent and architecture parts are still not
within the realm of what these systems can deal with. I also dont feel comfortable,
at least for actual production code, with trusting fully what the AI generates.

Going back and forth with the LLM's through these CLI/TUI tools to learn, plan
and research is actually both fun and very fruitful. Asking it to monitor a pod
thats running in a kubernetes cluster and present its findings so the requests
for the pod can be set in a good way. Or using that report to research further
what the actual documentation recommends against what Im doing, its just
speeding up learning so much. Although having said this, I also make it question
me and grade my answers and so on to actually make sure I'm always understanding
whats going on and that I dont have only surface level learning. It's not
perfect yet but its way better than just accepting whatever the agent generates,
and way more fun for me.

Using guiding document + different phases of implementation together with ralph
loop for super quick prototyping of full products. This build on the back and
forth with the agent concept. I have found it quite fun and useful to first
create a big architecture document around what is going to be built. Through use
of research agents to guide my own assumptions and understanding. This is the
bulk of the work. This is the actual engineering that I do, what used to be the
code is now this pretty much. I actually can spend hours going back and forth
here, this is the exploration of the problem space part but also the
architecturing of the system. Then from this I split it into smaller phases,
then i split the phases into individual tasks. Then I ask the agent to generate
a summary of the architecture document and call it the guiding document. This is
essentially the stage that I was talking about earlier. Each phase is its own
document containing tasks just to be clear. Once all this upfront work is done,
much of the writing of these documents is actually done by the agent itself once
I have taken all the descions and been interviewed, challenged and thought
through the architecture. At this point I just run the ralph loop script that I
have created. What it does is include the guiding document + the phase document.
Then for each iteration of the loop its only allowed to do one task. The tasks
are ordered in such a way that they are chronological for what needs to be built
according to my understanding. I just let it rip at this point and just watch in
amazement as what I had in my mind just hours earlier is being built. 

## Whats not working well 
ralph pluging in claude code.
opencode running playwright mcp.
Skills.
Compaction in opus 4.5

## What I want to explore more
Local LLMs for task
Voice inputs
Building an agent harness for different use cases. Meaning moving beyond
subagents into even more specific and narrow harnesses for task at hand.
Start using more of the small fast models and not use SOTA models for
everything.
