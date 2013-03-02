---
layout: post
title: "Introducing stately: elegant state machines"
description: >
  Stately is a new gem that makes working with state machines for Ruby objects an elegant
  experience. Define states, transitions, validations, and more.
category: gems
tags: [gems, ruby, design patterns]
author: ryan
---
{% include JB/setup %}

Recently, I've been working on a new Ruby gem called [stately](https://github.com/rtwomey/stately), which aims to be a paired down, elegant, no-surprises state machine helper. It's still a work in progress, but I've used it successfully on one project so far.

## A word on state machines

First, I should point out that I'm not always an advocate for state machines. I think they are overused (and abused). Typically, I prefer to err on the side of simply tracking state with a string on my ActiveRecord (or whatever) models and handling state transitions naturally, via methods and filters. However, there are plenty of situations where it makes sense to use a state machine. Stately is designed for those situations.

In many cases, it's convienient to setup an object that could be in one of multiple potential states such that by changing its state, it triggers one or more actions. For instance, let's define an object called Order (representing a customer's order at a pizza shop) as always being in one of three states: completed, invalid (i.e. the customer's credit card failed and we can't complete the order), and refunded (for the rare occasion when our pizza shop delivers an inferior pizza).

When an Order's state is changed, we want to trigger some helpful actions. The reason why we're interested in triggering actions _in response to_ changing state on our Order object is that it allows us to cleanly separate concerns from the rest of our application. Consider the example where we are writing an application that splits off our credit card processing from other business logic:

    class OrderCharger
      def initialize(order, credit_card)
        @order = order
        @credit_card = credit_card
      end

      def charge
        if charge
          order.complete
        else
          order.invalidate
        end
      end

      private

      def charge
        # complicated order charging logic here…
        # return true if it all succeeds
        true
      end
    end

This is an example of an understated but beautiful pattern I use constantly that separates business logic into concrete, separate ruby ojects that are easily tested. Steve Klabnik calls this pattern the '[Plain Old Ruby Domain Object](http://blog.steveklabnik.com/posts/2011-09-06-the-secret-to-rails-oo-design)'. I plan on writing about this pattern in a future post.

Throughout our code, we may have a couple places where we need to charge a card and complete an order (say, on a web form when the user is done ordering, or through an admin panel where an employee can enter card details they get from a phone call). In both cases, we simply do the following:

    OrderCharger.new(order, credit_card).charge

Digging into the object, we notice that we've cleanly separated any logic about what an Order needs to do to finish an order (or invalidate itself, should things go south) from the process of charging a customer's credit card. An Order isn't really concerned with the mechanics of charging credit cards and our OrderCharger doesn't really want to know all the messy details of what an Order needs to do when, say, a credit card fails. Instead, OrderCharger just tells the Order that it should be in either the `completed` or the `invalid` states and leaves it at that.

Stately defines the methods `complete` and `invalidate` on the Order object for us.

## Introducing stately

Stately allows you to define some states on an object and setup _transitions_ between those states. A transition is usually one or more methods you want to run whenever the object is going to transition into that state. For instance:

    class Order
      stately start: :processing do
        state :completed do
          after_transition do: :email_receipt
        end
      end
    end

The example above sets up an Order object with two possible states: processing, and completed. It also defines an instance method called `complete` on Order. Calling that method, as is done in OrderCharger above, will then do the following two steps (in order):

  1. Set the Order instance attribute `state` to be the string `complete`.
  2. Call the instance method `email_receipt`.

Having an `after_transition` is a handy feature for our example above, as it's the perfect place to let the customer know their pizza is on its way.

Following the pattern above, you can quickly define the states for invalid and refunded. Stately has an elegant DSL that tries hard to be self-documenting. As I mention in the README, a couple of the design goals behind the gem are:

  * Self-documenting so anyone can pick it up and know exactly what the transitions of a state are
  * No surprises. If your Order object was actually an ActiveRecord model, you would need an `after_transition do: :save` in order to persist the object's changes to your database (in our example, this would be the `state` column in the `orders` table).
  * Minimalist. Stately is about as simple as it gets and it tries hard to focus on the basics without getting into more complicated territory best reserved for more ambitious state machine gems.

Stately can do more than just handle transitions too. For instance, it has some handy validations that will prevent a state transition if certain preconditions aren't met.

## Getting started

Stately is easy to get started with. First, be sure to check out [the repo](https://github.com/rtwomey/stately) and read through the README. I've been adding examples and other documentation to get you up to speed quickly. But, here's a couple of quick pointers to get going on a Rails 3 project:

First, add stately to your Gemfile:

    gem stately

Be sure to run `bundle install` to install the gem.

Second, find the object that you'd like to use Stately with, and add the following template:

    stately start: :initial_state, attr: :my_state_attr do
      # ...
    end

Your `:initial_state` is a symbol for the object's initial state, when the object is first instantiated (in our example, we used `:processing`). The optional `attr:` defines what the name of the attribut on the object is used to track state (the default is `state`). For instance, if you had an ActiveRecord model called Order, which had a single column called `order_state`, you would need to use `attr: :order_state` above.

Third, define each of your states. They can be simple, like:

    state :invalid

You can also make them complex, like:

    state :completed do
      prevent_from :refunded

      before_transition from: :processing, do: :calculate_total
      after_transition do: :email_receipt

      validate :validates_credit_card
    end

Stately is designed to be intuitive and easy to understand, even by people who aren't otherwise familiar with the gem.

## How to contribute

Stately is still a work in progress. If you're so inclined, there are a number of ways you can get involved in the development of stately:

1. Use it and provide feedback. I built this gem to help solve a problem I've been having, but I want to hear how it has helped you (or how it could be improved). Leave a comment here, file a bug report, and submit pull requests.

2. Refactoring. The gem needs to be cleaned up. I've been working on it, but there is still plenty more to do.

3. Rethink how actions are derived given a state name. Right now, a hard-coded list of action names are used based on the state name, but there is plenty of room for improvement here.

4. Help get the word out. I'd love to get as many opinions on this gem as possible, so the more the better.

Hope you found this helpful. Let me know what you think of the gem and any projects you're using it in.

