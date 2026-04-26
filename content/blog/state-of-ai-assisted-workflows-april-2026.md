+++
title = "State of AI Assisted Workflows April 2026"
date = "2026-04-26T11:04:08+02:00"
draft = true

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an
# automatically created summary will be used."

tags = ["ai agents","developer tooling","generative ai","software engineering",]
+++

It's been three months almost exactly since I
[last documented](/state-of-ai-assisted-workflows-january-2026/) my workflows
and setup. Although it does not feel like it for me, reading back what I wrote
in the last update there are quite a few differences.

The system I have going now really does feel like an exoskeleton, to quote a
recent interview with DHH where he described it like having 12 arms and a
totally different kind of flow. Not necessarily because the tools themselves are
magical, but because the layer around them has become much more important to me.
The harness, the context, the workflow, the small scripts, the review loops and
all the other boring pieces around the model are increasingly where the leverage
seems to be.

## What I'm currently doing
Let's start with going through which tools I'm using currently and my general
approach.

### CLI agents of choice and model choice
Compared to 3 months ago this is probably one of the largest changes I have made
in my stack. I have completely stopped using Opencode and use three different
agents now to varying extents. My primary agent is Pi now because it is so
barebones and allows for true adaptation to me as the user and the way it's
built is just so damn nice that nothing comes close to it in terms of developer
ergonomics. My secondary agent is Codex just to see how OpenAI adapts their
harness to their models since I'm sure they RL the actual models to use their
own harness so I want to see what it does better than my Pi setup so I can
replicate the feature/capabilities should I want to. Last but not least I do
use Claude Code for two reasons. The first one is just to see what Anthropic
does and comes up with when it comes to harness engineering and the second
reason is to continuously use their model also so my biases and opinions are my
own always and based on actual experience instead of some proxy opinion based on
blog posts etc.

For models I use mostly 5.5 now that it's released and before that 5.4 on xhigh
when doing serious work. Although for 5.5 I keep it at high since there is no
difference between xhigh and high for the work I do except higher compute usage
so it makes no sense. I also use opus 4.6 and 4.7 quite a bit for frontend
specific things I think that model family is still the best for that even though
the gap is shrinking with each release.

### MCP exodus
From the beginning I have argued that MCPs are not the answer to most
situations as was their initial promise. So I have zero MCPs being used in my
current setup and have replaced context7 with ctx7, their own CLI, and the same
for Playwright. I must say, however, that I have come around a bit to MCPs and
think that they can be useful for situations where you don't want everyone to
have a CLI installed for some company-related internal tooling. But again that
MCP should be enabled only for specific subagents etc., not always on and
polluting the context of each session.

### Skills
I have seen a huge improvement in harnesses' and models' ability to actually
make use of skills which is really cool. Because of this I have been making more
and more skills and seen quite an improvement in robustness when it comes to my
day to day usage of AI.

I have a few skills at user level like conventional-commits that format commits
and the git workflow as I like to do it, a meta skill creator skill, and a few
related to how to do research and search depending on the harness being used.
Again even with skills it's important to keep the main context clean and slim so
that everything in the current session is relevant for the task at hand.

Then for each repo I have relevant skills. I also have entire workflows as
skills, like investigate this pipeline deployment run and tell me what's wrong;
these skills are not callable by the model itself they can only be called by me.
The way I build these skills is I do the troubleshooting or whatever workflow
with the agent by hand-holding it through the process the first time and then
ironing out the rough edges. Then I tell the agent to use the meta skill to
create a skill that allows it to replicate that whole workflow for the future,
this works so surprisingly well and is a huge productivity boost. Like anything
I do more than a handful of times now, I "automate" as a skill so that the AI
can do it by itself in the future and save me a bunch of time.

The benefit of converting these things to skills is that they can be shared in
the repo itself for other team members, which is quite nice.

### Review
Reviewing with agents is improving quite rapidly also and I have been trying
a new pattern which I have had some success with. Once something is ready to be
reviewed, in the session that was used to do the implementation I ask the model
to give me a summary of what it has done and interview me for the intent of
the change. That document then gets fed into the context of a new session which
is then used to review the changes. This makes it so that the review session has
some understanding of the context in which the PR was created and not just the
code itself or even worse that it tries to assume some intent or end goal by
itself. I have not quite got this to where I want yet but it's a clear
improvement over my previous more naive approach so that's always something.
Essentially it's a continuation of the idea of setting the stage in which the
agent is spawned in, which I wrote about in the previous update post.

### OpenClaw and Mac mini
I gave in to my FOMO like 6 weeks ago, very late by AI standards, and bought a
Mac mini and ran OpenClaw on it. Honestly I was not blown away with OpenClaw,
most of the things I was already doing and the new things with the heartbeat and
personality and stuff it just is not for me. Like I have no desire to bond or
treat AI as my friend, it's just a tool to me. Having said that however it's
been great to expose proper AI to my family through Tailscale and OpenClaw with
some tweaks. I got them interacting with actual SOTA models and not just the
free chat interfaces and they are really enjoying it and understanding why I'm
so into it so that's been great.

I'm finding though that the Mac mini is quite useful as a machine to SSH into
and run some workflows which I will get into later.

### Spec documents and PRDs
This way of working I flip flop on quite a bit. Sometimes I really like it but
most of the time it just feels like "waterfally" instead of being like
iterative. I like going back and forth with these models, poking and prodding
their answers seeing what they can come up with. Giving them a full spec works
sometimes when I know kind of what has to be done and have done that thing
before though so in those cases it's quite useful, but most of the time I like
the back and forth approach way more. Also spinning up and doing the actual
thing in worktrees or going down a path then summarize and go back to an earlier
point in the conversation with the learnings then branch off in a different
direction. This is possible and easy with Pi because the conversation is
structured as a tree.

## What's working well

### Pi
This harness is just above anything else right now. The ergonomics of how to use
it and customize it, the no fluff defaults and the fact that it does not keep
changing under you all the time makes it a super reliable tool that does not
break all the damn time. Other harnesses might have some fancier tools etc but
all of that can be replicated so easily and just by looking at the extensions
for Pi it's easy to see that, that benefit is not just theoretical. I mean my
system prompt when using Pi is like 2K tokens, Claude code improved for a while
but now its like back at above 20K same for Codex. And nobody seems to be
talking about this, I dont quite get it. There is nothing more important to get
what you want out of these models then keeping context as small and clean as
possible its all to increase the probability that the tokens generated is in
line with what you actually want. 

This is the thing I keep coming back to now: the model matters, but the shape
of the workflow around the model matters more than I used to think.

### Own tooling
I have been using and building my own tooling for my own needs to an extent I
simply did not have the time to do before which has been really fun and actually
made me more in tune with the machine so to speak.

I built one tool running on my Mac mini which takes a YouTube link and
transcribes and summarizes the whole video, this has been really useful to
combine with some workflows where I have an agent look at blogs and Hacker News
every 6 hours to find me content relevant to me based on some filters and send
them to me on Slack. This runs totally on my Mac mini, the only API calls are
for the summaries of the transcribed texts because I want high quality ones so
I send it to a SOTA model but other than that it's all local on that machine.

I built my own agent orchestrator so that I can send a bunch of prompts to
different agents and then in one TUI view I can see what they are all doing and
if they are waiting for input. This one is useful for sending the same prompt
also to many agents and see what they come up with and cross compare, there are
some funky things that this orchestrator makes really easy so it's been a blast
playing around with it.

I built a news aggregator for my dad which sends him an email based on what he
is interested in knowing from the news; this also runs on a schedule on my Mac
mini.

I have a bunch of agent tools specific for Pi that I have been building with Pi,
that's been really fun and super useful because they are totally fitted to my
workflow and way of working. This is something the other harnesses does not come
close to at all. Literally any aspect of Pi can be modified through the
extensions, it's unbelievable.

### Experimentation and simulation
This one is the newest addition to my toolbox. I find that it is so useful to
have the agent just spin up different simulations, stress test the code and
write reports, or run through a matrix of possible permutations for different
parameters for hours in the background while I'm doing other work.

To be a bit more concrete, I had to write a pipeline that will be moving almost
400GB of data every night. This is just one of many pipelines and it will run on
a shared node pool on our Kubernetes cluster which was not sized for these types
of workloads and so on. Long story short the performance matters here. So what
I did to get this really fast and resource efficient was that I downloaded a
representative sample of the data around 10-12GB and then ran different
configurations of the pipeline on that data fully end to end as it would in
production. This allowed me to find the best throughput while staying within the
pod resource limit.

Since then, I keep this kind of thing running in the background. Simulations and
experiments for different things to see if I have missed something or can
improve something. So far, every time some optimization is found I learn a ton
also about the underlying frameworks and tools used. It has been a really
enjoyable way of using AI at work.

## What I have stopped doing

### Overuse of subagents
Not a skill issue I think, at almost 10B total tokens burned by now I have a
hunch about how things go. I think having too many subagents doing different
parts of the work, unless that work is very narrow and really well explained
leads to intent guessing which rapidly decays into slop generation. It is much
better to treat subagents as strictly context preservation. Meaning send them to
do some research and come back then summarize the session yourself with the
agent and bring that back to the main context for example. This way there is no
intent guessing from the subagent, and you only bring what you needed back to
the main context with no context pollution with subagent calls etc.

I frequently jump into subagent sessions now and steer them, if the harness
makes this easy which Pi does. This is not something I used to do, before I was
much more prone to tell the main "use a subagent to look x up and research it".
Now I do that still but the way I have it setup is that I can jump into that
session and guide the subagent. I also have added so that the main agent always
asks me about my intent, this is an agents.md rule at the user level and mostly
only works for Pi, the other two, i.e. Codex and Claude, have too many system
prompt level rules already for it to actually pay attention to anything most of
the time but that's a whole other discussion.

## Rounding off
The main shift since the last update is not that the workflow is suddenly clean
or finished. It is more that I have stopped expecting the default shape of these
tools to be the right shape for me.

The models are good enough now that the bottleneck is often everything around
them. The harness, the context, the review flow, the way tools are exposed, the
way sessions branch, the way intent is carried from one step to another. That
whole layer matters way more than I think I understood three months ago.

So the direction for me is pretty clear. No more waiting for someone else to
build the perfect AI workflow, more small sharp tools that fit how I actually
think and work. More building the system myself, because as usual, what I
cannot create I do not understand.
