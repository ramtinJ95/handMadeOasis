+++
title = "State of AI Assisted Workflows July 2026"
date = "2026-07-26T22:28:03+02:00"
draft = false

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an
# automatically created summary will be used."

tags = ["ai agents","developer tooling","generative ai","software engineering",]
+++

Three months since the
[last update](/state-of-ai-assisted-workflows-april-2026/), give or take a day.
In that post I landed on the idea that the layer around the model is where the
leverage is, the harness, the context, the review loops, all the boring pieces.
I still think that's true. What changed this quarter is which problem I point
all of that at.

Because what I have actually been optimizing for is not speed of generation
anymore, it's keeping my mental model current at a rate of change that used to
be impossible. That's the whole post really. Everything below is either
something that helps with that, or something I stopped doing because it quietly
worked against it.

I did not arrive at this by thinking it through. I got there by trying the
opposite first, taking myself out of the loop almost entirely and letting the
thing run, then noticing what that did to me over a couple of months. More on
that further down.

I'm at somewhere around 30-35B tokens total now. That's barely up from where I
was in June and I think that flatness is the most interesting number in this
post. I use these tools more than ever but I have also gotten way better at not
wasting tokens. I am very deliberate about what goes into my system prompts,
skills and what I expose to the agent through my harness.

## What I'm currently doing

### Harness and models

Still Pi, and I don't see that changing. Nothing I have tried comes close in
terms of how much of it I can bend to how I actually work. My defaults right
now are 5.6 with thinking on high, and I reach for Opus 4.8/5 quite a bit, the
frontend gap I mentioned in April has mostly closed but I still find the
Anthropic models nicer to think out loud with.

I still keep Codex and Claude Code installed and use them regularly, but at
this point they are observation posts more than tools. I want to see what the
labs think a harness should look like, because they are the ones RL'ing the
models against their own scaffolding, so when they add something it's worth
asking why. Then I go see if I want that in Pi, and usually I can have it in an
afternoon.

I do use the ChatGPT desktop app quite a bit now, it's got really nice UX for
things that are not code related and that I don't really care that much about.
It is admittedly still way too bloated and does include a bit too much in its
context as system prompt and tool schemas and skill descriptions etc but all
things considered it's still useful.

The thing I use most in Pi that I did not really use in April is the tree
structure, properly this time. With auto-trees and a `/prime` command to set up
the root context, the branching stopped being a feature I knew about and became
the actual shape of how I work. Go down a path, hit a dead end, go back up,
take the learnings with me, branch again. Sessions are cheap, being wrong is
cheap, what would be expensive is carrying a polluted context forward and
having it interfere with the task at hand or just fill the context window with
unrelated tokens.

### Herdr instead of tmux

I have used tmux for years and I have now fully replaced it with Herdr. It's
clearly built by someone who works the way we work now in this new era of
agentic coding. It can do all the things that tmux does in a very nice way but
the new capability that makes it very valuable for me at least is that agents
can drive it, which sounds small but changes what you can automate. As an
example one of my favorite things to do is have the main session agent spin up
an explore subagent in a new Herdr pane, that way I can see exactly what it's
doing and also even steer it since it becomes like a normal session that
happens to talk to the main agent through Herdr, it's phenomenal really.

Herdr also adapts to a phone viewport really gracefully when I'm connected over
ssh to my machines. That last part matters more than I expected, which brings
me to the next thing.

### Working from my phone

I set up Termius on my phone and a `remote-access` script on my laptop, which
brings up a user-space sshd, mosh and keeps the machine awake, all over
Tailscale. So now I can be out somewhere and drop into my actual machine, with
my actual environment, my actual keys, my actual repos. Not a watered down
mobile app pretending to be a dev environment, the real thing.

I did not expect to like this as much as I do. It's not for writing code, it's
for the moments where you're away from the desk and you want to kick something
off, check on a run, or capture an idea before it evaporates.

I do enjoy dictating over SSH mostly, and for getting ideas out of my head fast
when I don't want to type on a clunky phone interface. Note that this is all
non-coding use. In January I wrote that friction between thought and
instruction is a feature and not a bug and I still believe that for anything
that touches code. For dumping raw thoughts, friction is just friction.

### The machines

The Mac mini is still doing heavy lifting and is more useful now than when I
bought it. ScribeBase and Voxcraft both run there and are exposed as APIs over
Tailscale, so anything on my network can hit them.

Then there's my old 2019 MacBook running Arch, which is now an infra testbench.
The reason it's a separate machine is boring and practical, it has an Intel CPU
and more RAM than I want to tie up on the mini, which makes it ideal for
Kubernetes labs and standing up a full ArgoCD setup to poke at. Having a
machine where the whole point is that it can be destroyed and rebuilt has been
really good for learning.

### Skills grew, and got locked down

The skill library kept growing. The big additions this quarter are `teach`,
`practice`, `anki-cards`, `pr-diary` and `visualize-code-changes`, plus a few
around ScribeBase for ingest and retrieval. The teach skill is an adaptation
of Matt Pocock's skill.

At the same time I did something that sounds contradictory, I stopped letting
the model invoke most of them. Almost everything is marked so that only I can
call it. The reason is that a skill is a strong instruction, and a model
deciding on its own that now is a good time for a strong instruction is a
decision I would rather make myself. When I invoke a skill I know exactly what
stage I'm setting. It also keeps me way more engaged and in the loop in a way
that helps me understand and build a proper mental model of what is going on in
the session and what decisions are being taken and why, something that skills
which the models invoke themselves rob me of.

### Automations

I have a handful of scheduled automations running through the ChatGPT app, some
on the laptop and some on the mini. A daily one that summarizes what actually
happened across my repos, a weekly rollup on top of that, and a weekly security
review. Output lands in a folder on the machine, and depending on what it is it
also goes to Slack or Discord.

None of this is clever. It's the same principle as everything else here, if I
do something more than a handful of times I want the machine doing it, and I
want the output somewhere I will actually see it.

I am however way more attentive and looking for things that I can automate both
for myself but also for my family now in a way I was not months ago. Because
now it's so easy to build these types of throw away software and there is not
much effort friction since I have set up my harness and can ssh into the
machine at a moment's notice to use the same harness and just speak ideas and
let the agent iterate on it in the background and then I can check in and steer
etc.

## What's working well

### The review loop, manually driven

This is the one I'm happiest with really, I use a persistent diff review window
that keeps a checkpoint, so when I review, everything currently changed gets
marked reviewed and becomes the new baseline. Next time I look, I only see what
the agent did after that point. This review loop extension in Pi also makes use
of the tree structure in Pi, so that each review actually becomes a checkpoint I
can always go back to and take a different path from, bringing back a summary
of the failed path and so on.

That's the whole trick and it sounds trivial, but it completely changes the
experience of reviewing agent output. Instead of one enormous diff at the end,
which nobody actually reads properly no matter what they tell you, I get a
sequence of small ones that I can hold in my head. I stay in the loop the
entire time and it costs me almost nothing to stay there.

For things I care about this is manual, and that is quite deliberate, more on
why below.

### Reading a core subset and graphing the rest

The problem is that I merge tens of PRs a day, my own, and I cannot read all of
that code, nobody can really. If I try I either become the bottleneck or I
start skimming and lie to myself about it. But reading none of it is how you
end up with no mental model of your own system, which is worse.

So the answer for me has been to split it. There is a core subset that I read
properly, line by line, the stuff that I think is important and core to how the
system behaves or that I will have to reason about later. Everything else I
review through diagrams instead, I built a skill around this that takes a
change and produces before, after, and what-changed views of the behaviour,
colour coded.

Here's the what-changed view for a real PR of mine, one that moved a repo from
direct destination writes to atomic staged writes:

![Before and after diagram of a PR that made artifact writes
atomic](/images/pr-17-what-changed.svg)

Red dotted is gone, green is new, yellow changed, grey stayed the same. I can
look at that for fifteen seconds and know what happened to the system,
including the failure paths, which is the part I would most likely gloss over
reading the diff. It's not a replacement for reading code. It's a replacement
for pretending to read code.

### The PR diary

When a PR merges I write a diary entry for it, which lives in a folder per repo
per year. Context, decisions, code path, and importantly what I deliberately
left out and why. It's written in first person because it's mine, and the agent
helps me produce it but the intent in it has to be real.

The value is not documentation, I have written plenty of documentation nobody
reads including me. The value is that writing down why I made a call, at the
moment I still remember it, is what keeps the mental model attached to the
code. Three weeks later the diary is the only reason I know why the Qwen temp
file path got collapsed into the shared writer instead of being left alone.

### Local infrastructure that isn't a toy

ScribeBase and Voxcraft have both grown into things I use daily.

ScribeBase is a local-first knowledge node, it takes PDFs, scans, handwritten
notes, markdown, articles submitted by my automations, runs OCR and extraction,
chunks and embeds everything locally, indexes into Weaviate, and hands back
cited context to whatever agent asks. It deliberately does not call a
generation model, that's not its job, it's a retrieval surface with citations.

Voxcraft is the YouTube tool from April, all grown up. Local ASR on Apple
Silicon, forced alignment, speaker diarization, chunking, with summarization as
the only step that leaves the machine because that's the one place where I
actually want a SOTA model.

The pattern in both is the same and I think it's the right one. Do everything
locally that can be done locally, spend the API call only where quality
matters, such as the summaries in the case of Voxcraft.

### AI pointed at me instead of at the code

The `teach`, `practice` and `anki-cards` skills are the ones I did not see
coming. `teach` builds an actual course in a workspace on disk, with a mission
document explaining why I want to learn the thing, learning records that track
what I have already understood, and lessons generated from real sources rather
than the model's own recollection. `practice` gives me exercises and refuses to
show me solutions until I have genuinely attempted them, which I need more than
I would like to admit. `anki-cards` turns the result into something I will
still remember in six months.

This is the counterweight to everything else in this post. The more the machine
writes, the more deliberate I have to be about my own understanding, and it
turns out the machine is quite good at helping with that too if you point it in
that direction.

## What I have stopped doing

### The dark factory

I tried the dark factory pattern properly and it was bad. The setup was nothing
exotic, an artifact that is essentially a PRD, then implementation, then review
loops running until nothing is found and at that point just merge the PR, no
human anywhere in the middle.

It produced subpar work and it derailed fast. The specific way it derails is
what interests me. The review loop terminates when nothing is found, which is
not the same thing as nothing being wrong, it's the same system grading its own
homework with the same blind spots it had while writing it. So the loop
converges on something that looks finished. Meanwhile every iteration drifts a
bit further from what I actually wanted, because there is nobody in the loop
who knows what I actually wanted. It also diverges and loses focus on the task
at hand and without guidance starts getting into super edge cases and then
keeps looping around wasting so many tokens for zero value.

What I got out of it was not working code, it was the realization that I had
been quietly accumulating cognitive debt for months. I was merging things I
understood at the level of the summary and not at the level of the system. It
compounds silently and you don't notice until you have to change something and
find out you're a tourist in your own codebase. This was for my own side
projects and it also started making me not want to build out side projects
because while it was fun to move fast the first hours, not having any real
understanding or connection to the codebase and how it was working, for me,
took the point and fun out of side projects.

Both the diagram skill and the PR diary came directly out of that. They are
both attempts at the same thing, keep the mental model current without going
back to reading everything, because going back to reading everything is not an
option at this volume.

I do want to be fair to the pattern. I think it can work for genuinely
throwaway things, prototypes I intend to delete, exploration of a design space
where the code is a means to an answer. Autonomy is not worthless, it's that
autonomy and care do not currently coexist, and I have to choose which one this
particular piece of work needs.

### Letting the model decide what enters the session

I touched on both of these elsewhere in this post but they are really the same
thing and they belong here. Most of my skills are now invokable by me only, and
subagents went from being a context preservation trick to two narrow tools.

What connects them is that in both cases the model was deciding what context to
load and when. That sounds like a pure efficiency question and I used to treat
it as one. It isn't. Every time the model loads a skill I did not ask for, or
sends a subagent off to figure something out and comes back with a conclusion,
it has taken a decision I never saw. The work still gets done, often fine, but
I was not there for the part where the decision happened, and that is exactly
the part I need to be there for if I want to still understand the system
afterwards.

So now I decide what enters the session. It is slightly more work per session
and considerably less work three weeks later.

### OpenClaw

Quietly gone. I gave it a fair shot and wrote about it in April, and it just
never became something I reached for. It just felt so bloated and I kept having
opinions about how things are done and what I would do differently. Then with
the release of the ChatGPT desktop app and the improvements it's seen lately,
the spirit of OpenClaw is pretty much embedded into a way nicer UI and form
factor so there is no point to ever reach for something like OpenClaw for
someone that's so concerned with what is fed into context and why, like me. Not
saying it's a bad tool or anything, but it's not for me.

### Subagents got demoted

In April I said subagents should be treated strictly as context preservation.
I have gone further than that. I now use exactly two, an explore agent and a
review agent, and I no longer think of them as a context trick at all. They are
tools that happen to be implemented as agents. Narrow input, narrow output,
nothing to guess about.

## Rounding off

So to say it plainly, I have stopped measuring any of this by how fast code
appears. Small checkpointed reviews, a core subset
read properly, diagrams for the rest, a diary entry at merge time, and skills
pointed at my own understanding rather than at the code. None of that makes the
machine faster, it makes me still know what's going on after the machine has
been fast.

I firmly believe that while the SOTA models are great at solving individual
problems now, at a speed and quality that matches and possibly even exceeds
most engineers, they cannot deal with systems yet. This lack of systems level
understanding is not getting better actually, or rather, its rate of
improvement is way way slower than the benchmark improvements we are seeing
every new model release. It's because these types of tasks and long horizon
rewards stretch out over weeks and months not minutes and hours which is what
the current benchmarks measure and also what the current heavy RL post-training
practices train the models for.

Cognitive debt is the real bill and it comes due later, quietly, at the worst
possible moment. You can outsource the typing. You cannot outsource the
understanding, or rather you can, and then it isn't yours anymore.

What I cannot create, I do not understand. And what I did not review, I did not
create.
