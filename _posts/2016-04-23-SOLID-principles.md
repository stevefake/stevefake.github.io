---
layout: post
title:  "The SOLID principles"
date:   2016-04-23
categories:
category :
tagline:
tags : [SOLID]

---

Thoughbot has a [piece](https://robots.thoughtbot.com/back-to-basics-solid) by
Britt Ballard on the SOLID principles. The acronym provides five guideposts for
object-oriented programmers to write code that is easy to maintain and update.

One of the two creators of the SOLID principles, [Robert Martin](https://en.wikipedia.org/wiki/Robert_Cecil_Martin)
(aka Uncle Bob), is also the coauthor of the Agile Manifesto.

The acronym can be broken down as follows (borrowed from [Wikipedia](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design))):

S - Single Responsibility Principle (SRP)
  a class should have only a single responsibility (i.e. only one potential change in the software's specification should be able to affect the specification of the class)
O - Open/Closed Principle (OCP)
  “software entities … should be open for extension, but closed for modification.”
L - Liskov Substitution Principle (LSP)
  “objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.” See also design by contract.
I - Interface Segregation Principle (ISP)
  “many client-specific interfaces are better than one general-purpose interface.”
D - Dependency Inversion Principle (DIP)
  one should “Depend upon Abstractions. Do not depend upon concretions.”

To illustrate the Single Responsibility Principle, Ballard presents some example
code with a single class, DealProcessor. I will only excerpt the relevant
portions here (for the full example visit the Thoughbot link above). There is a:
``def process
    @deals.each do |deal|
      Commission.create(deal: deal, amount: calculate_commission)
      mark_deal_processed
    end
  end``

And then a private method:
``  def calculate_commission
    @deal.dollar_amount * 0.05
  end``

To make this code easier to maintain, given the very real likelihood of needing
to update the commission percentage, Ballard refactors to create an additional
class, CommissionCalculator. Now the process looks like this:
``  def process
    @deals.each do |deal|
      mark_deal_processed
      CommissionCalculator.new.create_commission(deal)
    end
  end``

And the commission percentage is set in the new class:
``class CommissionCalculator
  def create_commission(deal)
    Commission.create(deal: deal, amount: deal.dollar_amount * 0.05)
  end
end``

I - Interface Segregation Principle
  -duck typing
    -- refers not to keyboard entry but to types of programming objects or classes
      https://en.wikipedia.org/wiki/Duck_typing
      http://rubylearning.com/satishtalim/duck_typing.html
