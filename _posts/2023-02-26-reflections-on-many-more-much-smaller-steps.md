---
layout: post
title:  "Reflections on Many More Much Smaller Steps"
date:   2023-02-26 21:30:00 -0500
categories: misc
---

I've been following GeePaw Hill (Michael Hill) for some time now, and recently decided to try putting into practice one of the ideas he writes about: Many More Much Smaller Steps.

If you haven't encountered this idea, go read [his series of blog posts explaining it](https://www.geepawhill.org/2021/09/29/many-more-much-smaller-steps-first-sketch/). The short summary is that when you make changes to code (or to a product, or process, or anything), you aim for changes that are small, that are shippable on completion, and that don't make things worse. When you are working towards larger goals that don't fit into the space of a single step, you compose a meandering path of short steps that don't always reach directly at the goal, rather than one longer step aimed at the goal. The blog series above is a fuller exposition of the benefits of making changes this way, and I don't want to rehash that here. Instead, I want to recount what I've tried and how well it has worked so far.

# What I tried

For context, at my day job where I wanted to try this, I am responsible for several interrelated repositories or sub-repositories hosted in GitHub. The bigger ones all require pull requests that pass automated build and test scripts in order to make changes to trunk, and some also require code reviews.

To try MMMSS, I wanted to commit to:
- Short changes (ideally around 15-30 minutes, but more realistically under 1 hour)
- Turning each short change into a PR to trunk
- Using feature flags to manage larger changes that could not be turned on in production all at once

In retrospect, there were some other factors that lowered the barrier to trying this:
- Our team already had an established pattern for defining and setting feature flags: environment variables with a specific name prefix, plus some tooling for setting values for these in different dev or production environments (k8s namespaces).
- I had built the projects where I first wanted to apply these practices with Test-Driven Development pretty much from the ground up, so they already had test coverage that I felt was *mostly* sufficient to prove shippability.
- The person I was going to have to send most of my PRs to for review was amenable to the idea of more PRs that were shorter to review.

# How it went

So how did it go? I didn't really set myself any criteria for whether this was successful, other than whether the plan I wanted to try was workable in practice. I explicitly did not want to set a goal of a specific number of PRs merged, partly because I still have to spend time on things other than changing code, and partly because I think measuring that kind of thing is starting down a dark path. Here are some things I observed:
- Overall, the basic strategy worked: I was able to make mostly short changes and merge them pretty quickly to trunk. I don't have a good way to measure the average wall-clock time for making those changes, since even when making short changes it's easy for me to get distracted or interrupted, but it felt like roughly an average of 30 minutes from start to finish. I'd have to go back and count, but my guess would be somewhere around 40 PRs that I've merged since I started. Only once did I check in a change that caused a bug in previously existing code, but it was easy to fix (and, more importantly, write a test for).
- I was expecting that the overhead of running builds and waiting on code reviews would slow me down the most. That was totally wrong; those two things were easy to work around. What actually slowed me down the most was trying to make changes in repos whose automated test coverage wasn't sufficient to prove shippability, and where I had to spend extra effort on setting up for, and doing, manual testing.
- I'd never used git interactive rebases much before, but they turned out to be helpful with using branches to switch to a second change while waiting on a first change to be merged. Overall, I think the size and rapidity of the changes I was making helped minimize the number of merge conflicts I had to deal with, and I think this is definitely an argument in favor of making these changes to trunk rather than just making a series of shippable changes off in a branch.
- I think my instincts about using feature flags were spot-on; I had several goals I wanted to achieve that had to be spread out over multiple days, and feature flags helped both to isolate the code for those goals and to selectively enable them in specific test environments. There's more I could write about how I use feature flags, but that should be a separate post.
- When I separate the act of merging a PR from the state of a goal being achieved, I found I had to be a lot more intentional about keeping track of all the goals I had in flight at once. My existing practice was to keep individual digital notebook pages (for work, in Notion) for each initiative. I modified that so that each page representing a project with more than a few steps would have dedicated step checklists and step planning areas; the former is to be able to tell at a quick glance how far along a path I was, and the latter is for a running conversation with myself about what steps I should take. Having summary writeups of what overall goals I was trying to achieve with each smaller step was also helpful to my reviewers; smaller changes are easier to review, but also make it easier to lose track of the bigger picture.

# Conclusion

At this point, my overall impression of MMMSS is that I endorse the ideas, and I want to continue practicing and refining my implementation of them and driving my step size down, but I am not nearly qualified enough to make general recommendations on how to put MMMSS into practice. I think that anything that shortens the cycle time from a developer making a change to a user using it is a good thing, and I think MMMSS takes this idea to its logical conclusion. However, in any particular development organization, there any many factors that determine the length and complexity of that cycle--I know I've certainly worked on teams before where my approach above would have been harder to do.
If any of the above is interesting to you, I encourage you to figure out how to apply MmMto your own situation.