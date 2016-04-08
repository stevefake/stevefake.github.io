---
layout: post
category : lessons
tagline: "All the little things"
tags : [intro, beginner, sandi metz]
---
{% include JB/setup %}

We watched a 2014 RailsConf [talk](https://www.youtube.com/watch?v=8bZh5LMaSmE) by Sandi Metz
after class yesterday. After spending so much time in the weeds trying to grasp all
of the fundamentals of Ruby, it was an interesting change of perspective to see
someone talk about the broader concerns of an experienced craftsman.

Metz's talk was about writing better objected code: "make smaller things" (classes,
methods, etc).

She described her approach to the Gilded Rose kata, which included a massive 43 line
if statement. Metz, who deprecatingly described herself as "boolean impaired", wanted
to make this unwieldy conditional more digestible. Do a "squint test" - look for changes
in shape (nested conditionals indented way from the left margin mean hard to follow
reasoning) and color.

The existing pattern failed, Metz noted in trying to make a small edit to an existing
unwieldy pattern. (Other words of wisdom from her: "tests are the wall at your back".)
So she reorganized the code to refactor the broken pattern.

She was trying to write code that was "not smart or clever" but comprehensible.
Well written code makes patterns readily apparent.

She argued that "duplication is far cheaper than the wrong abstraction," meaning
it is far easier to eliminate duplicative code later than it is to course correct
a wrong turn down a path with the wrong organization of the code. You can always
flag dups for yourself if you like to try to fix later.

Another mantra: "small methods are simple". The look of the overall code should
reflect that the shape is flat and the colors are clustered. _However_ intermediate
refactorings can get more complex first before arriving at the simpler end-point.
That's okay if you know where you're going.

Ruby code should accord With OO programming principles (basically a style guide
for arranging code): open for extension, closed for modification. Ideally you should
be able to add new behavior without adding code.

We want to write code that can adapt to the future. In other words, "make the change
easy, [although] this might be hard." And "refactor through complexity."
