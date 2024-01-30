# The problem with inheritance
Felix Lambda: So you've just explained to me why non ad-hoc polymorphism is
an amazing thing to have in a programming language. Then
why do you people constantly say *do not use inheritance*?

Olivia Object: We just spend a lot of time explaining why
interfaces are awesome. So why do we need inheritance? Or more specifically, what
can inheritance do that interfaces can't?

Felix Lambda: Well, it would seem to me that the main advantages of inheritance
is it's ability to store data. Interfaces can't store data.

Olivia Object: Why can't interfaces store data? Even in java, all you
have to do is declare a getter and setter method for the data you want your interface 
to store. And in more modern programming languages like Scala and Kotlin this is significantly
easier.

Felix Lambda: Maybe, doing it in a class is less verbose and easier to reason about?
Additionally, classes have private variables, which interfaces don't have. 

Olivia Object: That's actually really true. Even when you inherit from a class
you are still forced to reimplement the constructor. And if you are already
reimplementing the constructor, in Kotlin and Scala, you basically get
the getters and setters for free. And we could have added
private data to interfaces as well if we wanted too.
So, why did object-oriented programming rely
so heavily on inheritance over interfaces?


Felix Lambda: Maybe it's because you need to explicitly plan for an interface. If 
you want to extend a class, and they haven't created an interface for it, you
can't extend it as an interface. But you can extend it as a class.

Olivia Object: You are starting on the right track. But still in this case, why
do we have scenarios where people explicitly intend for other people to extend their classes?
For example, why is equals part of the Object class in Java? Why isn't it an interface?
Why is toString part of the Object class in Java? Why isn't it an interface? In more modern programming
languages like Rust, Equals and ToString are interfaces because not every object 
should implement them. But in Java, every object implements them. Why?

Felix Lambda: You still haven't explained why interfaces are better then inheritance.
Maybe the entire reason why object-oriented programming relied so heavily on inheritance
is because it's just as good as interfaces?

Olivia Object: Don't worry we will get there. But for now the simple reason
why Object-oriented programming relied so heavily on inheritance over
interfaces is because full-powered interfaces were only introduced in Java 8. 
Until then interfaces were basically useless. And even in Java 8, interfaces
are still not as powerful as they are in Kotlin and Scala. 
So for the most part, the reliance on inheritance was just a historical accident.
So when people say *do not use inheritance* they
are really just saying *interfaces are better then inheritance*.

Felix Lambda: So why are interfaces better then inheritance?

Olivia Object: Well, let's start with the obvious. Interfaces are explicit.
When you declare an interface, you are explicitly saying *override this method*.
When you extend a class, you have no idea if the person who wrote the class
intended for you to extend it. In fact Swift gives you the ability to 
only choose which methods you want the person to be able to override.
So, I can explicitly say *override this method* and *do not override this method*.

Felix Lambda: Isn't this a great argument for inheritance? You can still
extend a class even if the person who wrote the class didn't intend for you to extend it.

