---
layout: post
title: "The Infuriating NoMethod Error in Rails"
date: 2018-05-31 08:45:00 -0400
categories: rails
tags: nomethod
---

Usually I hit this because of a NULL value returned from the database.  Sheesh, says I, didn't this problem get solved years ago?  Bring back SQLMOD and a NULL indicator array!

But if all seems well, and there aren't any uninstanciated objects being referred to in the controller or the view, check the validates in the model.  You can only validate attributes named in the model's definition, that is, columns in the database table.

You forgot about the validates in the model, didn't you?  I know I did.

