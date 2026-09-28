# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

I write software for a living, mostly Python and TypeScript, but I am
new to contributing to other people's repos. This is one of my first
issues.

Here I am reproducing a bug before I try to fix it. What you can
expect from me: I show what I actually ran, I do not guess at causes I
have not checked, and I say plainly when I am unsure.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: name the reported behavior, don't diagnose it

My claim comment names the specific behavior the issue reports, in the
reporter's terms, so a maintainer can tell I read it. It does not
explain the cause: at claim time I have not reproduced anything yet, so
any theory I offer is a guess dressed as a finding.

- Wrong: "Very interested in this project, I'd love to work on this issue!"
- Right: "Picking this up — pressing `s` on an untracked file opens the stash-name prompt and then does nothing, as described."

### Rule: no promised timelines

I never commit to when something will land. I am doing this around a
job, I do not know the repo yet, and a date I miss costs the maintainer
more than saying nothing would have. I say what I have done and what I
am doing next; if I want to signal I am still on it, I post an update
instead of a deadline. This applies to soft promises too — "shortly",
"tonight", "by the weekend", "should be quick" are all dates.

- Wrong: "Repro below — I'll have a PR up by the end of the week."
- Right: "Repro below. Next I'm looking at where the decoder handles non-string keys; I'll comment here with what I find, and if I stall I'll say so rather than sit on it."

### Rule: one idea, starting from the friction

Each comment carries one idea, and it opens with the concrete thing
that goes wrong for a person using this, not with how I feel about the
project. Technical detail earns its place by saying what it makes
easier or more trustworthy for that person. No slogans, no generic
enthusiasm, no em dashes.

- Wrong: "Love this project and excited to dig in — this looks like a really impactful fix!"
- Right: "Anyone piping HCL through yq hits this on the first non-string key, and the panic gives them a stack trace instead of a parse error they could act on."

## Things I never post

- A date, or a word that works like one: "shortly", "tonight", "by the
  weekend", "should be quick", "almost done". See the no-promised-
  timelines rule.
- A claim that I will fix something, before I have reproduced it.

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->
