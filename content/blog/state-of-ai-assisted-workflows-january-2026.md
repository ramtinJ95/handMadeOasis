+++
title = "State of AI Assisted Workflows January 2026"
date = "2026-01-24T21:41:48+01:00"
draft = false
#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["ai agents","developer tooling","generative ai","software engineering",]
+++

Last time I wrote about my AI assisted workflows was 3 months ago. Paradoxically,
everything has changed while nothing feels different. Many of the techniques and
methods I was using 3 months ago were new to me. As I described in
[that post](/blog/state-of-ai-assisted-workflows-october-2025/), it felt like a
Frankenstein type of
workflow—many different systems duct taped together into something. That
something I, in hindsight, think of as the prototype. Now what I have is much
more refined and streamlined, and I think that is the reason for why it feels
like nothing has changed but at the same time everything has changed. Alright
let's get more concrete and into the actual stuff I'm doing

## What I'm currently doing

### CLI Tools of Choice
Day to day I use only CLI tools when it comes to agentic coding. The CLI tools I
use daily are currently Opencode and Claude Code. There is really not much
difference between my workflows for these 2 tools currently, I have managed to
replicate the "infrastructure" I like in both. The only reason I have started to
use Claude Code is because they did not like that people were using their
Claude Max subscriptions with Opencode and started messing with requests from
other harnesses. So when I use opus 4.5 I use Claude Code, it's as simple of a
reason as that really. I still much prefer Opencode though.

### Subagent Infrastructure
The "infrastructure" mentioned above consists of the following. I have a set of
subagents that I call upon, often directly, depending on the situation. The most
used ones are a research agent, review agent, test agent, ui test agent,
documentation agent and explore agent. These agents are then enriched with
different tools and permissions suitable for their focus. For example the
research agent is the only one with access to context7 and gh-grep MCPs, the
others do not get their contexts polluted with the tool descriptions from those
mcps. In the same way I actually limit the available tools also to further limit
context pollution. Because you don't want to give the LLM too many options either
because that will just make it more likely that it uses something wrong and so
on.

### Custom Commands and AGENTS.md Philosophy
Then I have quite a few custom /commands that I make use of to either prime the
context of an agent or give a specific set of instructions that I have found
myself repeating often. In these commands I often tell it to use specific skills
or subagents, and also have extra instructions around how I want certain things
done. I have found this approach works way better than having these instructions
in the global AGENTS.md file or in a project specific AGENTS.md file. I firmly
believe that those types of files that are always included in the context
regardless of which subagent is spun up or what the task at hand is, should be
kept extremely light and barebones otherwise they hurt way more than they help.
In practice, these files shouldn't be used as long-term memory. Instead just
include build commands and things like general file structure of the project.
Max 100-150 lines in the project specific AGENTS.md I think and for the global
one maybe max 50 lines.

### The Ralph Loop
The most recent addition to this "infrastructure" that I have made is what is
now known as the "ralph loop". This piece is essentially a script that uses
Claude Code or Opencode as unix cli tools and runs them in a loop.

Now how do all of these subsystems combine into something that works well? Let's
dive into that! 

## What's working really well

### Context as Memory Mental Model
Thinking about subagents as libraries and commands as functions and context as
the memory in the "classical" sense of computers, i.e I view context as an array
of memory that has to be viewed as a precious resource. AGENTS.md should be thought of
as a cache layer. If you see tool call failures, repeated build errors, or the
agent calling regex patterns to find files, those are cache misses. But even more expensive in a
sense because they pollute the current context for the task at hand with a bunch
of tool calls and outputs that is not relevant at all for the task at hand.
Building on this analogy, I think each task should be viewed as its own program
even. This means that as soon as the task at hand is changing you should start a
new session and reset the context. I have found that this keeps the agent
focused and reduces hallucinations and stops the agent from going "dumb". This
is the same thing as avoiding what is now called as the "dumb zone". This is
part of my current mental model when working with agentic coding systems at
least and I think it has served me well thus far.

### Setting the Stage
Working from a concept of setting the stage for the agent. This is the second
part of my mental model for working with agentic coding. This idea is what
guided me to be so cautious about what I include in the context. Basically the
idea is that we set the environment for the agent with a goal. By being very
deliberate and conscious about the environment which we put the agent in we can
limit the degrees of freedom it has but at the same time make use of the fact
that these are dynamic agents that can actually do useful things without us
having to give detailed instructions of each step.

### Subagent Utilization
Use subagents. If you are not already doing it yourself, I'm pretty sure most
harnesses actually have started to do this out of the box already. Get
comfortable with them and start using them extensively is my recommendation. The
value unlock of proper use of subagents is just phenomenal. If there is one
thing you really really should do out of all the things I have presented it's
this.

### Test-Driven Verification
Using tests to check my understanding and confirming what's actually been done.
This is about probing the system and making sure that whatever has been produced
matches your mental model of it. Reason for this way of working for me is
because the amount and speed of code generation is so high now that reviewing it
the old way is just not possible for larger features. We also don't want to lose
the connection to the way the system works and the interfaces between different
parts of the code, because the intent and architecture parts are still not
within the realm of what these systems can deal with. For production code, I 
still don't fully trust what the AI generates.

### Interactive Learning
Going back and forth with the LLM's through these CLI/TUI tools to learn, plan
and research is actually both fun and very fruitful. Asking it to monitor a pod
that's running in a kubernetes cluster and present its findings so the requests
for the pod can be set in a good way. Or using that report to research further
what the actual documentation recommends against what I'm doing, it's just
speeding up learning so much. Although having said this, I also make it question
me and grade my answers and so on to actually make sure I'm always understanding
what's going on and that I don't have only surface level learning. It's not
perfect yet but it's way better than just accepting whatever the agent generates,
and way more fun for me.

One other workflow that I recently have started doing is creating curriculums
for learning based on open source libraries. I ask the agent to research and
understand the codebase then try to find papers and sources related to the
domain or techniques used in that library. Then I ask it to create the
curriculum that will guide me from nothing to having a replica of that system
but I will myself have to implement all the actual code and what not. With the
sources it finds for source material and learning. It's actually a total shift
for me, it's like on demand tutorials on any topic customized for whatever I want
to get out of it.

### The Ralph Loop Approach
Using guiding document + different phases of implementation together with ralph
loop for super quick prototyping of full products. This build on the back and
forth with the agent concept. I have found it quite fun and useful to first
create a big architecture document around what is going to be built. Through use
of research agents to guide my own assumptions and understanding. This is the
bulk of the work. This is the actual engineering that I do, what used to be the
code is now this pretty much. I actually can spend hours going back and forth
here, this is the exploration of the problem space part but also the
architecturing of the system. Then from this I split it into smaller phases,
then I split the phases into individual tasks. Then I ask the agent to generate
a summary of the architecture document and call it the guiding document. This is
essentially the stage that I was talking about earlier. Each phase is its own
document containing tasks just to be clear. Once all this upfront work is done,
much of the writing of these documents is actually done by the agent itself once
I have taken all the decisions and been interviewed, challenged, and thought
through the architecture. At this point I just run the ralph loop script that I
have created. What it does is include the guiding document + the phase document.
Then for each iteration of the loop it's only allowed to do one task. The tasks
are ordered in such a way that they are chronological for what needs to be built
according to my understanding. I just let it rip at this point and just watch in
amazement as what I had in my mind just hours earlier is being built. 

## What did not work well that I have stopped doing
Ralph plugin in claude code. This plugin is just not it. It does not match at
all what the ralph loop is about and furthermore it does not even work really. I
would suggest you avoid it and first run the loop iterations by hand. In general
I have found that using "off the shelf" solutions and systems does not work at
all for me. I have to first run the things manually to understand the steps
involved, then after that do I have any success with automation.

Opencode running playwright mcp. For some reason when I run the playwright mcp
using my subagent in Opencode it always times out or does not really manage to
test the ui features properly. The same subagent instructions and everything
works way better in Claude Code for some reason, not sure yet why this is.

Skills. Honestly I almost never see skills load dynamically as advised. I don't
know if it's because the LLM's are not trained to use them as they are with tools
or what if the descriptions of the skills is where I mess up but for me skills
don't work as advertised. However when combined with /commands they are
great. Although to be honest I don't really know at that point why I should not
use /commands to replace the whole thing, they are the same thing really. But I'm
willing to concede that this could be a skill issue (pun intended).

Compaction in opus 4.5 or in general. I used to hit compaction a bit 3 months
ago but since that thus far has meant total collapse of performance I never hit
it anymore. I have seen though that codex seems to be really good at compacting
but the Anthropic models are just fully lobotomized if you hit compaction so
avoid it like the plague if you are using the current generation of models from
Anthropic.

## What I want to explore more
Local LLMs for tasks. I want to try them out and see what the current generation
of local llms can do, I have not kept up with this side of things recently.

Voice inputs. I have started to see some reports of this being way faster and
more reliable recently and I'm very curious to try it out. Although I do think
that some friction between thought and instruction to a LLM is a feature and
not a bug but that is a discussion for another time.

Building an agent harness for different use cases. Meaning moving beyond
subagents into even more specific and narrow harnesses for task at hand.
Start using more of the small fast models and not use SOTA models for
everything.

## Rounding off
The irony isn't lost on me: I use AI to build faster, then turn it off to learn
slower. Both matter. Speed without understanding is just disaster waiting to happen.
Understanding without speed is a luxury we no longer have.

This system has grown with me over almost 5 months of heavy use.
It's not something to replicate wholesale. Your own "infrastructure" should
evolve organically based on what works for you.

Find your own balance. Build your own tools. What I cannot create, I do not
understand. What I create too fast, I also do not understand.
