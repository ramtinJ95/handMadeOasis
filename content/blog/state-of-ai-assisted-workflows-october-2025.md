+++
title = 'State of Ai Assisted Workflows October 2025'
date = "2025-10-21T00:00:00+02:00"
draft = true
+++

This space is moving at a dizzing pace currently and as such there are many new
"frameworks", methods and approaches popping up and promising the world. But
having spent the past 2 months trying out many of these on non-trivial tasks I
have managed to nail a workflow which I have pretty good success rate with. This
post is intended to be like a context compaction of all the things I have
explored and learned during this time.

## Subagents and /commands
I wrote about a ladder of AI proficency in an earlier [post](/brain-dump-ai-assisted-workflows/) which essentially has these 4 levels:
* Level 1: Using a chat interface provided by one of the big model provided
* Level 2: Using the chat interface next to your code with something like cursor
  or github copilot in your editor of choice
* Level 3: Going full agentic and letting the agent iterate a few steps before
you as the human in the loop review, course correct and provide further
prompting
* Level 4: Having 2 or more agents work together without the involvement of a
human at every step.

I have now moved up from Level 3 to Level 4 and consistently make use of
subagents in my workflow. This really boils down to a context hack more than
anything else. Having one agent orchestrate a plan where each task in that plan
is done by a subagent that then throws away that entire context and only shows
the final step to the main agent is really useful to keep the main agent focused
and on the right track. Notice here that I'm not talking about having one
session that uses different "kinds" of agents, this is about one primary agent
that itself can spin up new sessions with subagents when it deems it necessary
or useful. Anthropic for example has come to the same realisation and released
the new memory tools which allows for deletion of older tool calls in the
context. 

In this category of useful hacks are /commands. Basically these are ideal for
repetitive prompts that can be used in different code bases, such as /review or
/brainstorm etc. Comparing them to the other techniques I will mention they are
probably the simplest, as they are basically stored prompts that can be reused
with some modifications. One of them is for example that in opencode a /command
can spin up a subagent or a specific agent with access to specific tools to
execute the prompt. But I mostly use it as a way to use a carefully crafted
prompt in many different scenarios, like /review and /brainstorm.

## SDD (Spec Driven Development)
This set of methodologies and tools are your spec-kit, openspec and bmad type of
approaches. The idea here is to use .md files as a way of making it very very
clear to the LLM what to do, how it should be done and breaking everything down
to small tasks that are reviewable by a human. Usually it also invovles many
steps of the human actually reviewing text in .md files before ever seeing a
single line of code. 

In their current iteration these methods and "frameworks" are completly unusable
when introducing them into an existing codebase, the amount of work it would
require is just not worth it at all to get slightly more deterministic results.
In general I think this whole idea is something like UML was when it was
introduced, in theory it sounds great in reality its just not working. Why would
I spend all this time writing and review text that will then be used by an
ultimately random text processing engine that will use this as input to generate
some code or text? Its just way faster to just iterate on the code and steer the
agents and write the damn code myself. If I had such a clear understanding of
the problem that I could write all these specs, why not sure write the code at
that point?

I dont know maybe its a personality thing or a workflow thing, but this method
is does not make sense to me at all. To me they sell a false sense of control.
Just come to grips with the fact that LLM's as a tool are not deterministic and
move on and apply them where that is not a problem. 

Just to be clear, yes spend some time on your prompt and try to manage your
context as much as possible but all this extra work around .md files in multiple
steps and the amount of front loading, its just not the way in my honest
opinion.

## MCP and SKILL.md
This section could be an entire post but I will try to keep it as short as
possible.

## Current workflow
My current workflow is a frankenstein of all the aformentioned methodolgies
bundled together where I have discarded what i dont find useful and kept what I
have found works for me. Also to further drive home the point that what I'm
doing and how I'm working with these tools probably will not work for you is
that my workflow is based on how i derive satisfaction when programming, for
more detail and a thorugh explanation see this [post](/ai-and-software-engineering-the-conflict-within/).
### Tools and resources used currently
* opencode, cli tool for using llms. Open source claude code pretty much.
* Gemini pro, only using the deep research capability
* z.ai, a super cheap llm provider, I use it for access to GLM 4.6. Unlimited
usage for 45 dollars per quarter. This is my main model currently
* zen.ai a non-profit llm provider, I use it to try out different models at like
  20 dollars a month. Been really liking kimi k2 from here recently.
* Github copilot subscribtion, only using claude 4.5 from here.
* Different MCPs not that much anymore but still happens.

## Conclusion
Conclude with something about how these tools are useful but for a fairly
limited set of cases. Talk about scaffolding, boilerplate, quick reviews, deep
research, etc


