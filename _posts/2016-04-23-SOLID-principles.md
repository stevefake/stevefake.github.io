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

* **S - Single Responsibility Principle (SRP)**
..*  a class should have only a single responsibility (i.e. only one potential change in the software's specification should be able to affect the specification of the class)
* **O - Open/Closed Principle (OCP)**
..*  “software entities … should be open for extension, but closed for modification.”
* **L - Liskov Substitution Principle (LSP)**
..*  “objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.” See also design by contract.
* **I - Interface Segregation Principle (ISP)**
..*  “many client-specific interfaces are better than one general-purpose interface.”
* **D - Dependency Inversion Principle (DIP)**
..*  one should “Depend upon Abstractions. Do not depend upon concretions.”

To illustrate the **Single Responsibility Principle**, Ballard presents some example
code with a single class, DealProcessor. I will only excerpt the relevant
portions here (for the full example visit the Thoughbot link above). There is a:

```
  def process
    @deals.each do |deal|
      Commission.create(deal: deal, amount: calculate_commission)
      mark_deal_processed
    end
  end
```

And then a private method:

```  
  def calculate_commission
    @deal.dollar_amount * 0.05
  end
```

To make this code easier to maintain, given the very real likelihood of needing
to update the commission percentage, Ballard refactors to create an additional
class, CommissionCalculator. Now the process looks like this:

```
  def process
    @deals.each do |deal|
      mark_deal_processed
      CommissionCalculator.new.create_commission(deal)
    end
  end
```

And the commission percentage is set in the new class:

```
class CommissionCalculator
  def create_commission(deal)
    Commission.create(deal: deal, amount: deal.dollar_amount * 0.05)
  end
end
```

To illustrate the **Open/Closed Principle**, Ballard gives another example of a
violation of the OCP and then refactors it so that new `parse` methods could be
added without the need to update any of the surrounding code.

The **Liskov Substitution Principle** argues that parent and children super and
sub classes should be able to substitute for one another. To ensure this mutable
methods should not override parent methods, as it may introduce unpredictable
behavior. Ballard gives the example of two classes, a square and a rectangle
class, in which an unsuspecting programmer confronting legacy code violating the
**LSP** would be surprised to find that calling set_height on an instance of the
Rectangle class could modify the width of the object. The surprise arrises from
the Square class written in violation of the **LSP**.

The **Interface Segregation Principle** says that housing multiple APIs under a
single interface will cause confusion and that they should instead be broken out.
Wikipedia gives us the [origin](https://en.wikipedia.org/wiki/Interface_segregation_principle)
 of this principle.

Robert Martin (Uncle Bob again) was consulting for Xerox on a new, multi-task printer
system. The growing software system for the printer was becoming unwieldy to update.
I'll let Wikipedia take it away:

"The design problem was that a single Job class was used by almost all of the tasks. Whenever a print job or a stapling job needed to be performed, a call was made to the Job class. This resulted in a 'fat' class with multitudes of methods specific to a variety of different clients. Because of this design, a staple job would know about all the methods of the print job, even though there was no use for them.

"The solution suggested by Martin utilized what is called the Interface Segregation Principle today. Applied to the Xerox software, an interface layer between the Job class and its clients was added using the Dependency Inversion Principle. Instead of having one large Job class, a Staple Job interface or a Print Job interface was created that would be used by the Staple or Print classes, respectively, calling methods of the Job class. Therefore, one interface was created for each job type, which were all implemented by the Job class."

As an aside, Ballard references duck typing in discussing **ISP**. [Duck typing](https://en.wikipedia.org/wiki/Duck_typing) refers not to keyboard entry
but to types of programming objects (i.e. categorization). In the Ruby language,
the focus is more on what an object's capabilities are than on its class. To [quote](http://rubylearning.com/satishtalim/duck_typing.html), "Duck Typing
means an object type is defined by what it can do, not by what it is."

The **Dependency Inversion Principle** states that in object-oriented design,
[both](https://en.wikipedia.org/wiki/Dependency_inversion_principle) high and
low-level modules should depend on the same abstractions. Ballard returns to the
parser example and refactors it to meet the **DIP**. "This decouples our
high-level functionality from low-level implementation details and allows us to
easily modify what those low-level implementation details are. Having to write a
separate usage file parser per file type would require lots of unnecessary duplication."
