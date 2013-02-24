---
layout: post
title: "The Path to Programming Greatness"
description: "Improving from a good programmer to a great programmer."
category: Programming
tags: [design patterns, refactoring, testing]
---
{% include JB/setup %}

There was a time when I thought the height of programming was the ability to take a problem, and solve it. Take an idea or a specification, and start writing code. Once the code does what you wanted it to do, you're done! Just keep learning about new frameworks and technologies to help you figure out how to get things done.

I think at that time I was in the "don't know how much you don't know" phase. I've since learned that being able to write code that matches a spec is only the first step in becoming a better programmer.

After you can write code, now you have to figure out how to write **good** code. Why does this matter? This has been discussed by greater minds than mine, but here are a few reasons why I think you need to always work to make yourself a better programmer:

* **Maintainability**: If you come back to your code in a month (or a year, or 5 years), and your code is a mess, you can't assume you'll remember why you did everything the way you did.
* **Working with others**: Even if you're able to remember what you've done and keep everything in your head straight (certainly not something I can claim about myself), when you start to add other people into the mix, you get problems. The larger the team grows, the more important it becomes that code is easy to understand and add to.
* **Growth**: There are always going to be new features, or changes to existing features. Good code is flexible enough to handle this. If a change requires updating 50 different classes, or refactoring half the project, then the code is brittle. If developers aren't diligent, a project can quickly become unmanageable. Often at this point the team just throws up their hands and decides it's time for a "rewrite" -- something that drives fear into the hearts of managers.

I've found a few general ways to improve code:

* **Refactoring**: Remember when I said that once the code did what it was supposed to do, I thought it was done? Now I've realized that this is just the beginning. What comes next is refactoring. I think a lot of people see code from great programmers and think that the code came straight out of their heads exactly as it reads on-screen. In my experience, even the great programmers start with ugly but workable code, and refactor from there. Usually we just get to see the end result.
* **Design Patterns**: Think of these as "pre-built" solutions. When you look at a project as a whole, you probably won't find an existing solution that already does exactly what you need (otherwise, why would you be writing it?). But design patterns are a way to keep your code clean on the small to medium scale. The tricky part is knowing when to use them.
* **Testing**: This is what makes the previous two concepts work. I used to think that tests existed mainly to catch bugs; either before they happen by testing complex code, or afterwards to prevent regressions. While I certainly saw the value in this, I found writing tests to be very difficult, and I questioned whether it was worth the time. What I realize now is that if writing a test is hard, it's often a sign that the code under test has problems. Code that is well factored and designed is easier to test. I now use tests as much for design as I do to catch bugs.

Those are the general concepts. But how do you get there? Over time I've kept a list of resources that I've found especially useful, and I'd like to share them here.

I've found that there's a sort of path that comes between the wide-eyed programmer who isn't even aware of these concepts, and the seasoned programmer:

1. **Step 1: The Basics.** Learning the semantics of clean code, refactoring and testing.
1. **Step 2: Refinement.** Design patterns and advanced testing concepts.
1. **Step 3: Limitation.** Knowing what to use and when.

I think the last step is key; even if you were to learn all there was to know, you still wouldn't be a good developer until you knew when to use these principles, and when **not** to use them. Any of them can be taken too far: for example endless refactoring without making progress on features, or writing so many tests so that the codebase is dwarfed by the tests.

The resources below will help guide your learning at each step. Note that not all of these resources are specific to Ruby. While there are differences between languages, it would be a mistake to limit yourself only to the Ruby world. In particular, a lot of the concepts thought of as being core to Ruby come from Java in particular.

I also want to point out that by no means do I feel that I've reached the limit of my learning. In fact I think the great programmers are the ones who are always learning, questioning and improving themselves.

## Step 1: The Basics

### Books:

* [Clean Code: A Handbook of Agile Software Craftsmanship](http://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
* Refactoring ([Ruby edition](http://www.amazon.com/Refactoring-Ruby-Jay-Fields/dp/0321603508/) or [Java edition](http://www.amazon.com/Refactoring-Improving-Design-Existing-Code/dp/0201485672/))

### Articles & Videos:

* <http://robots.thoughtbot.com/post/10125070413/unobtrusive-ruby>
* <http://robots.thoughtbot.com/post/27572137956/tell-dont-ask>
* <http://en.wikipedia.org/wiki/Test-driven_development>
* <http://dannorth.net/introducing-bdd/>
* <http://blog.daveastels.com/files/BDD_Intro.pdf>

## Step 2: Refinement

### Books:

* [Design Patterns: Elements of Reusable Object-Oriented Software](http://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612/)
* [Design Patterns in Ruby](http://www.amazon.com/Design-Patterns-Ruby-Russ-Olsen/dp/0321490452/)
* [Practical Object-Oriented Design in Ruby](http://www.amazon.com/Practical-Object-Oriented-Design-Ruby-Addison-Wesley/dp/0321721330/)
* [Test Driven Development: By Example](http://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530/)

### Articles & Videos:

* <http://confreaks.com/videos/240-goruco2009-solid-object-oriented-design>
* <https://practicingruby.com/articles/shared/ozkzbsdmagcm>
* <http://www.mockobjects.com/2009/09/brief-history-of-mock-objects.html>
* <http://jmock.org/oopsla2004.pdf>
* <http://www.youtube.com/watch?v=983zk0eqYLY>

## Step 3: Limitations

### Books:

* [Growing Object-Oriented Software, Guided by Tests](http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627/)

### Articles & Videos:

* <http://martinfowler.com/articles/mocksArentStubs.html>
* <http://web.archive.org/web/20080827212731/http://www.moseshohman.com/blog/2008/07/06/too-many-mock-objects-ruby-refactoring-death/>
* <http://blog.davidchelimsky.net/2008/12/11/a-case-against-a-case-against-mocking-and-stubbing/>
