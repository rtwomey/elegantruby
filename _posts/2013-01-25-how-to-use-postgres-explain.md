---
layout: post
title: "How to use Postgres EXPLAIN"
description: >
  Slow queries in Rails are automatically run through EXPLAIN, but what does this tell us? Learn
  how to use EXPLAIN effectively and speed up slow queries.
category: Tutorials
tags: [postgres, optimization, rails]
---
{% include JB/setup %}

A key tool in analyzing slow SQL queries is to use the
[Postgres EXPLAIN command](http://www.postgresql.org/docs/8.1/static/sql-explain.html). You may
have noticed the output of this in your Rails log already: if Rails detects a slow SQL query in
development mode, it will automatically re-run the query with EXPLAIN on, and output the results in
your log. This command can be very helpful in understanding exactly what Postgres is doing under
the hood.

Let's take a look at one of the quickest ways to use EXPLAIN to find a missing index. Let's say
you have the following pop up in your log:

    Limit (cost=102403.14..102403.22 rows=30 width=943)
      -> Sort (cost=102403.14..102403.27 rows=49 width=943)
            Sort Key: id
            -> Seq Scan on vehicles (cost=0.00..102401.77 rows=49 width=943)
                  Filter: ((created_at >= '2012-01-04 16:06:25.350568'::timestamp without time zone) AND (created_at < '2013-01-24 16:46:05.787881'::timestamp without time zone) AND (company_id = 1234))

The important thing to note is the 'Seq Scan' part of that EXPLAIN. This indicates that Postgres
has determined it needs to iterate sequentially through a list of results in order to find the
right result (the Filter line is showing you what that sequence consists of). Whenever you see a
'Seq Scan' in your logs, it's usually a good indication of a potential place to put an index.

Having an index on vehicles.company_id will eliminate the need for this costly 'Seq Scan', so let's
add one. Create a migration that looks similar to:

    class AddIndexToVehiclesCompanyId < ActiveRecord::Migration
      def change
        add_index :vehicles, :company_id
      end
    end

I knew that it was on the `vehicles` table because Postgres tells me the 'Seq Scan' is going
through vehicle results, and I knew I needed an indx on the `company_id` because that's in the
WHERE clause of my query and is indicated in the Filter line above (in other words, if Postgres
is iterating through results to find the one result where `company_id = 1234`, we know we need to
add an index so it can immediately find the row with that `company_id`).

After running the migrations, I can confirm that this query has sped up significantly and I'm no
longer getting an EXPLAIN result in my logs. Hooray for fast queries!
