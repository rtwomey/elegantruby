---
layout: post
title: "Adding a counter cache for fun and profit"
description: >
  Learn how adding a counter cache in Rails reduces the number of database hits for situations
  where you might be displaying a count of elements many times on a single page.
category: Tutorials
tags: [optimization, rails]
---
{% include JB/setup %}

Let's say you visit one of your Rails app's pages in your browser and, in glancing at your logs,
you happen to notice a whole bunch of these:

     (0.3ms) SELECT COUNT(*) FROM "wheels" WHERE "wheels"."vehicle_id" = 1
     (0.3ms) SELECT COUNT(*) FROM "wheels" WHERE "wheels"."cohort_id" = 2
     (0.2ms) SELECT COUNT(*) FROM "wheels" WHERE "wheels"."cohort_id" = 3
     (1.1ms) SELECT COUNT(*) FROM "wheels" WHERE "wheels"."cohort_id" = 4
     (1.0ms) SELECT COUNT(*) FROM "wheels" WHERE "wheels"."cohort_id" = 5
     (1.1ms) SELECT COUNT(*) FROM "wheels" WHERE "wheels"."cohort_id" = 6
     (0.5ms) SELECT COUNT(*) FROM "wheels" WHERE "wheels"."cohort_id" = 7
     (0.3ms) SELECT COUNT(*) FROM "wheels" WHERE "wheels"."cohort_id" = 8

This is a sure sign that it's time to add a counter_cache. A counter cache in Rails is just an
additional column that tracks the number of associated models. For instance, here's how you'd
setup your two models:

    class Wheel < ActiveRecord::Base
      belongs_to :vehicle, counter_cache: true
    end

    class Vehicle < ActiveRecord::Base
      has_many :wheels
    end

Then, we need to add a migration:

    class AddWheelsCountToVehicles < ActiveRecord::Migration
      def up
        add_column :vehicles, :wheels_count, :integer, default: 0, null: false

        Vehicle.find_each(select: 'id') do |result|
          Vehicle.reset_counters(result.id, :wheels)
        end
      end

      def down
        remove_column :vehicles, :wheels_count
      end
    end

This migration will add a column, named `wheels_count` to the `vehicles` table, and set the default
to 0. It then proceeds to iterate through all Vehicles in the database, one-at-a-time selecting just
the vehicle ID. Next, the Rails `reset_counters` method is called on Vehicle. Given an ID of a
Vehicle and the counter_cache column to update, Rails will count the number of associated Wheel
objects and update the `wheels_count` column.

Of course, if you have a lot of Vehicles in your database, that migration could take some time to
run, so plan ahead for how you'll deploy this to your app.
