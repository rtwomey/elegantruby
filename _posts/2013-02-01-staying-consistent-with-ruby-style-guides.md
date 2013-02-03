---
layout: post
title: "Staying consistent with Ruby style guides"
description: >
  A good Ruby style guide helps a programmer focus on the logic of the system and less about the
  mechanics of writing code. Learn other benefits and how to create your own.
category: Refactoring
tags: [refactoring, ruby]
---
{% include JB/setup %}

Writing code for many years, there's one thing I prize above nearly all else: consistency. As a
contractor, it's delightfully refreshing and unfortunately rare to jump onto a project where code
followed a style guide (especially one that I roughly agreed with), and it noticeably decreased the
time it takes to get familiar with the codebase.

Contrast that to a project where code is a horrible mish-mash, inexplicably inconsistent; a
downright eyesore. I hate starting a project like this and I usually spend time upfront just
cleaning things up, adding a bit of organization, and simply getting things to a readable
state.

Having a good style guide that's rigorously (though not religiously) enforced is a key component
to introducing elegance in your Ruby apps. Discipline starts with a style guide.

## In the beginning...

We're all hackers. However long you've been a programmer, in whatever language you choose, we've
all started off in the same place: tinkering and bashing until something works. As we become more
proficient we build bigger and bigger programs with more ambitious goals, and more complicated
codebases. As we do this, we often struggle with organization and style considerations.

At some point, you've probably come across Ruby and realized the inherent elegance of the
language. There's a strong push towards readability over cleverness, which usually results in more
English-like structure.

As projects become more complicated, and more people join the team, we realize it's important to
standardize around a commonly-accepted formatting style. This reduces the context-switching we
mentally have to perform every time we jump from one team member's contributions to another, and
it lets us focus on the system itself.

## What exactly is a style guide?

It's whatever you want! But generally, the best ones are clear, unambiguous documents of opinions
that comprise:

  * Language formatting instructions (i.e. two spaces instead of tabs)
  * Which language and/or framework features to prefer over others (for instance, "prefer `map`
    over `collect`").

The best style guides evolve over time, and often require quite a bit of experience to be solid.

## Discipline begets transcendence

When a programmer is in _the zone_, he or she is no longer thinking of the keys being tapped on the
keyboard, the characters entered into a text editor, the number of lines being written. Instead,
you're completely focused on the _system_, building each of the instruments that will soon form a
symphony. This is coding transcendence, and it's only achievable when the act of programming melts
away. When you can completely forget about syntax, language rules, and other minutiae, then you
enter _the zone_.

Discipline is all about internalizing syntax to the point where you don't even have to think about
it any more. This is key: a style guide reduces the number of mental cycles you need to expend to
write a line of code because it takes all the tiny little decisions out of your hands. For
instance, if you didn't have a style guide, you might be equally likely to write the following:

    users.map { |user| user.company.touch :updated_at }

    # later, in some other part of your code...
    companies.map do |company|
      company.trade_organization.touch :updated_at
    end

Both are perfectly legit, and neither is right or wrong. However, if you're trying to get into the
zone (or, conversely, a new member of a team just looking to get up-to-speed), the last thing you
want to do is see code that accomplishes similar goals but doesn't follow the same written
pattern. You have to _think_, for however brief a moment, to put it together.

A style guide, however, trains your eye to recognize a syntactic pattern and skip over the step of
mentally unrolling it, and instead focus only on the differences from the last time you saw the
pattern. Here's the same thing above, but internally consistent:

    users.map { |user| user.company.touch :updated_at }

    # later, in some other part of your code...
    companies.map { |company| company.trade_organization.touch :updated_at }

Reading the internally consistent lines goes faster because you can focus strictly on the
differences. The syntax rules melt away and you can focus on the _system_.

Style guides are all about removing the decision making in styling your code. Once you've
internalized the concepts, you'll write code faster that's easier to read and gives you more time
in _the zone_.

## Consistency is King

Starting a new project with a style guide in place is always easier than going back and converting
a large codebase. Whatever situation you're in, first decide what you personally like and what is
going to work for your team.

  * Start by reading through the [Ruby Community Style Guide](https://github.com/bbatsov/ruby-style-guide)
    and [GitHub's Style Guide](https://github.com/styleguide/ruby). You don't have to agree with all
    of these, but start thinking about what you prefer so you can decide what you want to pull into
    your own style guide.

  * Next, check out [thoughtbot's style guide](https://github.com/thoughtbot/guides/tree/master/style),
    which goes a bit deeper on the opinion side of choosing what elements of a language or framework
    to use over others.

You can go in any number of directions with your own style guide, but I suggest keeping things simple
and not spending much time on edge cases or hypotheticals. Instead, treat your style guide as a
living document, updating it as you encounter new situations. But, once you've committed to a
style, stick to it and only consider modifying it if you are clearly sure it's a change your
strongly prefer.

Do you have a formal style guide? Do you only follow a style guide on team projects, or your
personal stuff as well? What other guides are good resources people should check out?
