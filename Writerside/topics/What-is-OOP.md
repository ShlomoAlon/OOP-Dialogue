# OOP Dialog
The following is a dialogue between a student and a teacher. 
The student has some experience with both functional programming and OOP.
But has read lots of online content that has given him a negative view of OOP.
And has convinced him that OOP sucks and that functional programming is OP.

In a sense, this is a conversation between my past self and my current self.

Oliva Object: What brings you to my office today?

Felix Lambda: I'm having trouble with OOP. 
It all looks so complicated and contrived.
Why am I creating all these classes and objects?
What is the meaning of life? And why aren't we learning the
pure beauty of functional programming instead? After all, does
not functional programming make all bugs go away by virtue of
making all data immutable?

Olivia Object: Ok wow, that's a lot of question
The meaning of life is simple, it's 42, it's always been 42. The other
questions are will take a bit longer to answer. But I'll try my best.

The first question we need to answer is what is an object? Or to be more precise,
what is the difference between an object and a struct? or an object and a record?
C had structs, and haskell has records, and noone considers C to be an OOP language.
So what's the difference?

Felix Lambda: Honestly, that's part of why I'm so confused. 
I understand struct's, there so simple and intuitive. I have a bunch
of data, and I want to bundle it together because I pass them around 
alot together. So I put it in a struct.
Classes seem similar to struct's but differ in some subtle way.
But all these differences seem to just make things more complicated 
and unintuitive. Some languages have some of those differences, some have others.
So I'm not sure exactly what the difference is.

Olivia Object: That's an excellent starting point. Let's pretend for a bit
all our language has are structs and type aliases. Imagine you
are building a checkers solving engine. How would you go about storing
the state of a checkers' game?

Felix Lambda: Well, a checkers' game is just a 2d array of squares with at 
most one piece on each square.
The simplest way to represent that is as a type alias of a 2d array of characters.
By keeping it as a type alias, I keep all the functionality of a 2d array. 

```Rust
type CheckersState = [[char; 8]; 8];
let initial_state = [
    ['r', ' ', 'r', ' ', 'r', ' ', 'r', ' '],
    [' ', 'r', ' ', 'r', ' ', 'r', ' ', 'r'],
    ['r', ' ', 'r', ' ', 'r', ' ', 'r', ' '],
    [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
    [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
    [' ', 'b', ' ', 'b', ' ', 'b', ' ', 'b'],
    ['b', ' ', 'b', ' ', 'b', ' ', 'b', ' '],
    [' ', 'b', ' ', 'b', ' ', 'b', ' ', 'b'],
];
```

Olivia Object: That's a good start, but wouldn't it suck
if someone accidentally created an invalid board, state then
passed it to your code? For example, what if someone did this

```Rust
// someone passes in the v's instead of r's and b's
let invalid_state = [
    ['v', ' ', 'v', ' ', 'v', ' ', 'v', ' '],
    [' ', 'v', ' ', 'v', ' ', 'v', ' ', 'v'],
    ['v', ' ', 'v', ' ', 'v', ' ', 'v', ' '],
    [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
    [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
    [' ', 'v', ' ', 'v', ' ', 'v', ' ', 'v'],
    ['v', ' ', 'v', ' ', 'v', ' ', 'v', ' '],
    [' ', 'v', ' ', 'v', ' ', 'v', ' ', 'v'],
];
```

Now when you pass that state around in your code, it will create
some really wacky behavior. You
 could check in every function that takes a board state to
make sure nobody did something foolish like that. But that would be
very tedious and error-prone. 

Felix Lambda: YESSS, I learned all about this in my functional programming class.
Make invalid states unrepresentable. Functional programming is amazing at that. Here's
an easy way to fix the problem.
```Rust
enum Piece {
    Red,
    RedKing,
    Black,
    BlackKing,
    Empty,
}

type CheckersState = [[Piece; 8]; 8];
```

Now we can't create an invalid state.

Olivia Object: That's a excellent idea. But do you really think that's
the only way to create an invalid state? Are there perhaps any squares that
your engine might assume are empty? What happens if someone puts a piece on 
a dead square? Like this
```Rust
let initial_state = [
['r', 'r', 'r', ' ', 'r', ' ', 'r', ' '],
[' ', 'r', ' ', 'r', ' ', 'r', ' ', 'r'],
['r', ' ', 'r', ' ', 'r', ' ', 'r', ' '],
[' ', ' ', 'r', ' ', ' ', ' ', ' ', ' '],
[' ', ' ', ' ', 'b', ' ', ' ', ' ', ' '],
[' ', 'b', ' ', 'b', ' ', 'b', ' ', 'b'],
['b', ' ', 'b', ' ', 'b', ' ', 'b', ' '],
[' ', 'b', ' ', 'b', ' ', 'b', ' ', 'b'],
];
```

Felix Lambda: The engine would never terminate since we can end in a state where
no player could possibly win. There might be other bugs as well since my code
might rely on the fact that those squares are empty.
This isn't the end of the world though, if someone creates an invalid state like
this, then it's their fault. I can't protect against everything.

Olivia Object: Wouldn't it be nice if we could make it impossible to create an invalid state?

Felix Lambda: That would be nice, but I don't see how we could do that
without dependant types. And those look way to complicated for me right now.
Even if I could figure out how to use them, I'm not sure if they are worth the
added complexity.

Olivia Object: Haha, I can't understand them either. I only passed my type theory class
because my professor himself didn't fully understand them.
I'm not convinced that anyone truly understands them.

Felix Lambda: WAIT, I have an idea. What if we just make the board smaller? There are only
32 squares on a checker's board, so we could just make the board 8 by 4. Then we wouldn't have
any dead squares. This would also make the engine more space efficient since we would only need
to store 32 squares instead of 64. The calculations might be a bit more complex, but I think
that is worth it for the added safety.

Olivia Object: That's a really good idea. 
But can you see how this won't generalize to other problems?
This is a clever solution in this instance, but it's not a general solution
to the problem.


Felix Lambda: 
You're right, but I'm still not convinced that there isn't always
a clever solution like this. In fact I think it's telling that the 
first example you gave me I was able to solve with a clever solution.

Olivia Object: Don't worry, we will work through a couple more examples later.
The reason I used this example is that it's easy to understand.
Most data structures are far more complex than this. And there exists no clever
solution for them due to the nature of their complexity.
Trust me when I say this. For the vast majority of data, 
there is no way to make invalid states unrepresentable at the type level.
And even if there was, the sheer complexity of the code would make it
completely unworkable.

Felix Lambda: Ok, I'll take your word for it. But I still don't see how
this relates to OOP.

Olivia Object: Don't worry we will get there. 
But first, another question. In the interest of speed, you decide to maintain 
both a board state and a list of pieces. This way you can quickly iterate
over all the pieces as well as directly query the piece on a given square.

```rust
type Location = (int, int)
struct BoardState {
    board: [[Piece; 8]; 4],
    pieces: [(location, Piece); 24],
}
```

Felix Lambda:
There are so many ways to create an invalid state here.
What if the board and the pieces are out of sync?
This would be a nightmare to debug and completely destroy the engine.
It's so inelegant. I would never do this.
You should only store data once, so it can't get out of sync. If you need to iterate over the pieces
then iterate over the board. It might be a bit slower but storing state twice is never 
the solution. 

Olivia Object: I mostly agree with you on this. Storing state twice
is usually a bad idea. But the reason is precisely because
when you store state twice, 
you can potentially create invalid states. But storing state twice 
is sometimes necessary for speed. In fact, 
the entire tech stack you are using, from your operating system to your compiler
rely on storing state twice. And when it is necessary, you need to
be able to do it in a way that doesn't create invalid states.
Also, storing state twice isn't the only way to create invalid states.
I'm just using it as an example to show you that there are many ways to create invalid states.
And that it's not always possible to make invalid states unrepresentable at the type level.
Moving on.

What if you finish your engine, you release it to the public; it becomes the
de facto checkers engine for the entire world. And then two years later, you
read about bitboards, and that the best way to represent a checkers' state is
like this.

```Rust
type CheckersState = (u64, u64, u64, u64);
```

Felix Lambda: I would never do that. I don't care at all about speed. Especially,
not linear speedups like that.
I only care about correctness. And that CheckersState is 
less type safe than the other one. There are so many ways to create
invalid states in this representation. I would never do that.

Olivia Object: (Smacks forehead) I'm sorry, I keep forgetting that you are a functional programmer.
But remember, the faster your engine is, the more problems you can actually solve. 
Speed is an incredibly important part of a checker's engine. Stockfish beats humans
because it can search many more positions than a human can. And it can do that because
someone who cared about speed wrote it.

How's about this. Pretend your name is John, and you are normal.
What would you do?

Felix Lambda: (thinks for a bit) I don't know.
Hundreds of people are relying on my engine. Changing the data structure
would likely break all of their code. I would probably just leave it as is.
And release a new version of the engine that uses bitboards.

I guess the most important thing is to plan ahead. When you
define a data structure, you need to realize that it will likely
be impossible to change it later. So you need to make sure that 
you nail it the first time.

Olivia Object: What if you put a note in the documentation that says
don't rely on the way the data is stored. And that it might change in the future.

Felix Lambda: That could work. But we both know that no one reads the documentation.
Also, I want them to rely on the way the data is stored. I want them to be
able to inspect the different branches of the search tree. 
Besides, how would they even pass in the data? The engine takes a CheckersState
as an argument and returns checkers state's as an output. 

Olivia Object: You can keep that part of the API the same. You just, need to
convert the data from the old format to the new format.

Felix Lambda: It still feels weird to have two different types of CheckersState.

Olivia Object: Would you roughly agree to the following problem
statement. 

**Whenever you represent something in code, whether it be a checkers state,
a list, or an email address, there is rarely a one to one mapping between the data
and the way it is stored. And when there isn't, there is almost always the
possibility of creating an invalid state or of needing to change the way
the data is stored.**

Felix Lambda: I can agree to that. I see what you mean by email as well.
The following does not work.
```Rust
type Email = String;
```
since there are many strings that are not valid emails (though my intuition 
is that there is a complex way of representing too enforce valid state). I'm not sure
what you mean by a list though.


Olivia Object:
Have you ever accidentally created an invalid list? Perhaps
an invalid String? An int?
Maybe an invalid Bool? Or an invalid Float?
What about an invalid dictionary? Or a set?

Has this ever happened to you? 

Felix Lambda: I'm not sure what you mean. Of course I've never done that.
It's impossible. These are all primitive types. They can't be invalid.
I've gotten some weird behavior's before, like once when my
integer overflowed.

Olivia Object: What do you mean they are all primitive types?
Under the hood, they are just represented as bytes. And bytes can be anything.
And there are so many ways to create an invalid byte. At least for
lists, strings, sets, and dictionaries. Integers and floats are
impossible to create an invalid state. But for lists, strings, sets, and dictionaries, 
the vast majority of states are invalid. It wouldn't surprise me if 99 percent
of all byte representations of these types are invalid. Not just are they invalid,
the consequences of using them are undefined and incredibly buggy.
Any C programmer will tell you of the pain of using an invalid primitive type.

So how is it possible that you have never created an invalid list, string, set, or dictionary?

Felix Lambda: This is silly. I never touch the byte representation of these types.
In fact, as far as I'm aware, most of the languages I use don't even expose the byte representation.
In fact, in rust using the byte representation of these items in your code is considered unsafe.
One of the big differences between rust and C is that rust doesn't let you access the byte representation in
regular safe code. You have to use unsafe rust to do that.

Olivia Object: But rust let's you trivially access the byte representation of an integer? 
What's the difference?

Felix Lambda: Like you said above, integers can't be invalid. So it's safe to access the byte representation.

Olivia Object: Would you say that there is a one to one mapping between the way integers and the way they are stored?
But that there is not a one to one mapping between the way lists, strings, sets, and dictionaries and
the way they are stored? And when there isn't a one to one mapping, then 
you shouldn't be allowed to access the way they are stored since the 
bytes might be in an invalid state?

Felix Lambda: That is partially True. 
There is a very big difference between byte representations and larger data structures though.
When the byte representation is invalid, you get undefined behavior.
When the larger data structure is invalid, you get buggy behavior.

Olivia Object: While I agree that there is a difference between undefined behavior and buggy behavior.
Wouldn't you agree that we should do our best to prevent both?

Felix Lambda: I agree. But I'm not sure how to do that. You're the teacher, you tell me.

Olivia Object: I'm glad you asked. Though one final point. Did you know that
rust documentation says that you should never rely on
the byte representation of a struct even in unsafe rust 
since it might change in the future? Does this remind
you of anything?

Felix Lambda: It reminds me of the checkers' engine when you suggested I put a note in the documentation
that the data structure might change in the future and programmers shouldn't rely on it.

I think I'm starting to see a pattern here. Whenever there is a one-to-one mapping between
data and the way it is stored, then you can rely on the way it is stored. But whenever there
isn't a one to one mapping, you have to deal with the possibility of invalid states and the
possibility of the way the data is stored changing in the future.


Olivia Object: That's exactly right. And the solution
is really, really simple.
Just like Python doesn't expose the byte representation of lists, strings, sets, and dictionaries.
And they make it private so that you can't access it. We can do the same thing with structs.
We make the struct representation private.

Felix Lambda: How do I do that. As far as I'm aware, my favourite language Haskell doesn't have private data.

Olivia Object: You can do what programmers have been doing for decades. You just don't use Haskell.
It's a really simple trick. It will save you hundreds of hours of your life. 

Felix Lambda: Ok I just googled why
haskell doesn't have private data.
They say that it's because immutable data does not need to be private.
Since it can't be modified to an invalid state.

Olivia Object: Immutable data solves one very specific problem. It 
prevents you from modifying already valid data to an invalid state.
This is actually pretty useful, it saves us from having to define getters and setters 
(which we will cover later).
But Just because you can't modify the data to an invalid state doesn't mean that you can't
construct it in an invalid state.
Also, even if you could prevent invalid states, you still
might want to change the way the data is stored. And then everyone who relied on the way the data
is stored will be screwed. In other words, **just because
the data is immutable doesn't mean that the representation of the data is immutable.**
Functional programmers treat immutability as a silver bullet. It isn't, private data 
representation is a must have for any language that wants to be anything more than an academic toy.

Felix Lambda: I can kind of see how private data representation would prevent people
from relying on the way the data is stored. But I don't see how it would prevent invalid states.

Olivia Object: The solution is subtle but simple.
All functions that have access to the private
data representation or private functions,
must be written in such a way that they can't create any invalid state.

Felix Lambda: So your solution is git good?
I fail to see how this actually solves the problem.
The problem was that there is no way to prevent invalid states. And your solution is too just
to tell people to not create invalid states. That's not a solution.
Invalid states are still possible at the type level. 
Making it private doesn't change that.

Olivia Object: Let's go back to rust unsafe for a second.
Do you know why unsafe rust code has to be written in unsafe blocks?

Felix Lambda: The state reason is that if something goes wrong, then
we know where to look. Or to state it differently, we want to
minimize the surface area of code that we need to audit consistently.

Olivia Object: That's exactly right. And we do the exact 
same thing with private data. We do our best to minimize the
surface area of the code that can possibly create invalid state.
Even further, that code can itself rely on the fact
that the struct is already in a valid state. Because all functions
that have access to the private data representation only ever result 
in a private state. This means that your code doesn't need to do 
any validation. It can just assume that the data is already valid. 
Since no other public function is ever allowed to create an invalid state.

This of course isn't always followed. But this is the ideal.
Part of the reason why I love rust is because
there is a very strong culture of doing this.
When you use a library that has a struct,
you can be reasonably sure that the struct is always in a valid state
regardless of what you do to it. And that the library will never 
create a breaking change to the struct representation
since anything that could possibly change will be private.

Do you know how Files are represented under the hood?

Felix Lambda: Based on my brief and painful time coding in C, I'm going to take 
a wild guess and say that it's an integer.

Olivia Object: That's actually correct. When you open a file, 
you tell the operating system to open a file, and it returns a positive integer.
Then, when you want to read or write to that file, you pass in
that integer to the read and write functions (that both take in an integer). Then, 
when you no longer need it, you tell the operating system to close the file.
Another wrinkle is that when you close the file, the operating system might
re-use that integer for another file. 

Now you're already an old hand at this. So what is the problem with the following
type definition?

```Rust
type File = int;
```

Felix Lambda: What if someone passes in a negative integer?

Olivia Object: That's a good point but we can fix that by just using 
an unsigned integer. What else?

Felix Lambda: What if someone tries to read from a file that they 
already closed? Or even worse,
what if someone closed a file and in the meantime, they opened another file
and that file got the same integer as the first file. Then, when they try to write
from the first file, they actually write from the second file. 
This sounds like a legendary bug that I would be telling my grandchildren about.

Olivia Object: So how do we fix that?

Felix Lambda: based on what you said above, we need to make the integer private.
So, we define it like this

```Rust
pub struct File {
    private file: int,
}
```

Now we need to create the file using a function that returns a File. And 
ensure that that function always returns a valid file. We can do that like this

```Rust
pub fn open_file(path: &str) -> File {
    let int = OS_open(path);
    if int < 0 {
          panic!("Failed to open file");
    }
    return File { file: int };
}

pub fn read_file(file: &File) -> String {
    let mut buffer = String::new();
    let int = file.file;
    OS_read(int, &mut buffer);
    return buffer;
}

pub fn write_file(file: &File, data: &str) {
    let int = file.file;
    OS_write(int, data);
}

pub fn close_file(file: &File) {
    let int = file.file;
    OS_close(int);
}
```

Olivia Object: That's really good for a first try. But
can you spot a way to create an invalid state?

Felix Lambda: What if someone calls close_file then calls read_file?
Once you called close_file, the integer stored in the file struct is no longer valid.
Best case scenario you fail to read the file. Worst case scenario you read from the wrong file.

Olivia Object: That's exactly right. So how do we fix that?

Felix Lambda: What if we completely remove the close_file function?
I get that it's inefficient to keep the file open even after you are done with it.
But in the name of correctness, it's worth it.

Olivia Object: (Smacks forehead) I'm sorry, I keep forgetting that you are a functional programmer.
But remember if you open enough files, you will run out of file handles and linux will crash your program.
We can do better than that. Later, we will implement a far better solution.
But for now we will do the following.

```Rust
pub struct File {
    private file: int,
    private is_open: bool,
}
pub fn open_file(path: &str) -> File {
    let int = OS_open(path);
    if int < 0 {
          panic!("Failed to open file");
    }
    return File { file: int 
           is_open: true };
}

pub fn read_file(file: &File) -> String {
    if !file.is_open {
        panic!("File is closed");
    }
    let mut buffer = String::new();
    let int = file.file;
    OS_read(int, &mut buffer);
    return buffer;
}

pub fn write_file(file: &File, data: &str) {
    if !file.is_open {
        panic!("File is closed");
    }
    let int = file.file;
    OS_write(int, data);
}

pub fn close_file(file: &File) {
    if !file.is_open {
        panic!("File is closed");
    }
    let int = file.file;
    OS_close(int);
    file.is_open = false;
}
```

Felix Lambda: (Scrunches face in disgust) This is so ugly. Your code is littered with
data validation. It's so inelegant.

Oliva Object: I agree. In fact, that's part of the 
reason we do our best to minimize the surface area of code that interacts with the private data representation.
The private data representation is often hacky and inelegant. But it's a necessary evil.
But like I said, we will get to a better solution later.

Felix Lambda: So the entire point of OOP is to make fields in structs private?
That's it? That's the big secret? What about inheritance? What about polymorphism?
What about encapsulation? What about constructors? What about all the other things that OOP is famous for?

Olivia Object: Don't worry, we will get to all of that. But we have yet to answer the
question that we started with. What is an object? And what do we mean when we say that 
code is object-oriented?

Felix Lambda: I'm still not sure. You've explained to me that the fundamental difference between
and object is that an object has a private data representation. But I'm still not sure what actually make 
something an object?

Olivia Object: Where going to use a very non-standard definition of an object.
** An object is a user created primitive-type that is so well-defined that it is as if
you added another primitive type to the standard library**.
Just as you rely on the List, String, Set, and Dictionary types to be rock solid. 
To never create an invalid state. To never create a breaking change in user land. So too an
object is a user-created type that you can rely on to be rock solid. To never create an invalid state.
To never create a breaking change in user land.

Object-Oriented programming is a two-part philosophy. The first and non-controversial part is that
all API's around a data structure should be defined as objects. The second is that
even in your own code, all data structures you use should be defined as objects.
Since you yourself are a user of your own code.

Most of the remaining OOP concepts are just ways to make it easier to create rock-solid objects.

Felix Lambda: What if my entire API is just a series of functions that take in other types?
Like for example, I'm creating a library that does some math or I'm creating a sorting library.
Why should I define my API as an object?

Olivia Object: You shouldn't. In fact, this was the biggest mistake that OOP made.
Object's should only exist when you are wrapping data together. However, a large majority of API's
do wrap data together. Take for example the top rust crates. I would say that around 30 percent of them
are data structures. However this vastly underestimates things since even the crates that aren't data structures
still likely have a data structure somewhere in their API.

<tldr>
hello world
</tldr>






















