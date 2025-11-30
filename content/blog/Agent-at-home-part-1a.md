+++
title = "Agent at Home Part 1a"
date = "2025-11-30T09:54:01+01:00"
draft = true
#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["ai agents","generative ai","software engineering", "developer tooling",
"agent at home series"]
+++

Before we get into the coding part of the series I think it would be beneficial
to give some background to what the ReAct loop is that we are going to make use
of to build this agent and also describing a bit how the code is going to look
like. This will not be that long to be fair because the basic loop is fairly
simple once you grok the ReAct loop.

Note: Just to be clear the goal of this series is not to explain how LLMs work
in detail and so on. While it is valuable to know how they work and I derive
quite a bit of delight from doing that, this series is about engineering. Which
means it is not necessary to know how a LLM works under the hood to use it in a
productive and efficent way. For me this is the best example of the difference
between a scientist and an engineer. (todo make this section better)

## What is an agent
Lets start with me defining what I think an agent is: An agent is essentially a
while loop that leverages an AI model (i.e Claude, GPT, Gemini etc) to interact
with and reason about its environment in order to accomplish a user defined goal(the
user prompt basically), those interactions are mediated through external tools.

Now it is sometimes useful to categorize agents on a spectrum of different
agency levels. On such a spectrum this definiton of an agent is fairly high on
up on the agency level and would be called something like a multi-step agent.
Since the output controls the program flow. The goal of this series is to start
with a multi-step agent and move towards fully multi agent which means an agent
that can start other agentic workflows.

I mentioned the spectrum because there are other simpler agents that can also be
very useful such as a router agent which with its output determines some basic
control flows, i.e which path code execution should go through based on some
decision that would be hard to code for in a deterministic way. But that is not
the focus of this series.


## ReAct (Reasoning + Acting)
The core ReAct loop is actually very simple conceptually. All that is
happening is: Thought → Action → Observation → (repeat)

This means that you send some prompt and that prompt includes your system
prompt, what tools are available and what the objective is. The LLM then reasons
about the task and then figures out it needs to use one of the tools in the tool
list you sent in your request. So it sends that back in its response, the agent
then executes that tool, this is the action step. The output of that action is
sent back to the LLM with the entire message history (this is the context) and
the LLM again reasons about the next step. This then repeats until the final
response from the LLM does not require any more action, which means it either
has an answer or needs more input from the user. Thats all there is to it.
For more details read the [ReAct Paper].

I think the reasoning part is worth going into a bit deeper though because there
is some terminology and methods that interact in that step that I initially
found a bit confusing.

### Chain of thought's role in the reasoning step
Chain of thought (CoT) was all the rage like a year ago, maybe less. That is an
eternity in AI so I think its worth explaining CoT's role in the ReAct loop. 

Basically CoT is a prompting technique where us as users nudged the models to
produce intermediate reasoning steps before coming up with the answer, or at
least show the intermediate steps, and this improved the accuracy on reasoning
tasks. 

So this means that the ReAct loop is using CoT in its thought/reason step as
explained earlier above. The confusing part is what is shown to the user and
what the model is doing implicitly so lets dig abit deeper still.

State of the art AI models are, and have been for a while, trained
(fine-tuned after initial model training to be precise) to generate these
internal chains of thought by default and then distill them into a final answer.

The word internal is doing a lot of heavy lifting there. Essentially all the
"thinking" AI models are doing internal/hidden CoT steps even if you as a user
dont ask for it. 


