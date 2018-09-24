---
layout: post
title:  "Rails Left Outer Joins Don't Always"
date:   2018-04-10 12:45:00 -0400
categories: ruby, rails, ActiveRecord
---
So RoR 5.0 brings us a left outer join ActiveRecord method.  What the docs (and examples) don't tell you is that the default is to *not* actually join the tables.  Unless you explicitly ask for columns from the joined table, you get ... none.  Yup, none.

Sheesh.

But if you run the SQL (as given in the development server log) in say the Sqlite3 console, you get the merged tables.

What's annoying is that you don't hit this obstacle until the view throws an exception, typically NoMethod for *nil*, where *nil* is a column from the joined table.
