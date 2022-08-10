---
Title: How Java Works
---

# What is code?

Programmers have to read code. The very act of
debugging, for example, is reading your code and 
looking for a defect to fix. However, the way a
programmer reads code is very different from how
a computer reads code.

When humans read code, we:

* misunderstand things
* skim over uninteresting or simple things
* make incorrect assumptions.

Obviously, computers can't do this. Computers need
code that it can translate into instructions. This
code must be:

* unambiguous
* syntactically correct
* logically possible (such as no null pointers)

The earliest programming involved carefully defining every
machine operation, one by one, often with mechanic or
physical parts use to communicate instructions to the
computer. Often, these instructions were written in a
form of binary. The thing is, computers fundamentally work
the same way; we've just replaced the mechanical parts
with small, faster electronic parts.

From the software side, we found that writing code in
binary was cumbersome, time-consuming, and error prone.
So human readable languages started being developed.

Early languages, like Fortran, added human-readable
identifiers, like "IF", "PRINT", etc. You can see
such structure in this [Greatest Common Divisor code] (https://en.wikibooks.org/wiki/Fortran/Fortran_examples#Greatest_common_divisor)
for Fortran 77.

# Compiling

It's important to understand that, when you write source code,
your computer actually can't run your source code. Well, not directly. Typically,
we write code, and compile that code into something that 
can either be run or used by the computer. The below diagram is
an over simplification of this relationship:

<img alt="alt" src="{{site.baseurl}}/modules/java/images/3/compiler1.png"/>

## Compiling in C

Before talking about Java, let's talk about how C works,
and how it C code is turned into an executable program.

```c 
    #include <stdio.h>
    int main() {
        printf("Hello, World!");
        return 0;
    }
```

I can write the following code in any plain-text editor,
like Notepad (though not "document editors" like Word),
and save the file as ```helloWorld.c```.

<img alt="alt" src="{{site.baseurl}}/modules/java/images/3/helloWorldC.png"/>

Now, if I save this file to a folder called "code",
I can open a terminal (Mac/Linux) or Powershell[^1] and navigate to that folder.
If I use the "ls" (list) command, I can see the file
in my directory.

<img alt="alt" src="{{site.baseurl}}/modules/java/images/3/shell1.png"/>

However, that's all I can do with it. If I try to run
the file, my Windows operating system thinks I'm trying
to "open" the file.

<img alt="alt" src="{{site.baseurl}}/modules/java/images/3/shell2.png"/>

That's because **source code isn't an executable program.** Source
code is meant for humans. We write the source code using a
programming language because it's easier to read than those
low-level machine instructions. If we want to run our program, we must
**compile** it.

## Why not just write low-level machine code?

**Compiling** is turning human readable source code
into computer readable instructions. Anyways, we can
compile c files with a program like GCC. So, I compile
the code into a program called ```helloWorld.exe```, and
then open in Notepad and:

<img alt="alt" src="{{site.baseurl}}/modules/java/images/3/executable.png"/>

What is going on here?!?! Well, what is happening is that the .exe
program contains the low-level machine instructions that tells my
operating system how to run the program. This machine code
is stored in bytes which do not properly translate to human-readable
text in a format that Notepad knows how to display. However, it's quite all right that this code isn't
human-readable because it doesn't need to be human-readable. If
we want to change the program, we can change the c code and recompile.

We never have to work directly with the contents of the .exe file, 
which, as you can see above, *is a definite bonus*, because even
in the best of circumstances, working with that machine code
is cumbersome, difficult, and very limiting.

## Advantages and Disadvantages

The advantage is that I can run this program as is! I don't
need any special software! In fact, anyone with a modern Windows
OS could run this same .exe file, meaning I can share the
runnable program without sharing the source code! Anyone 
running this code doesn't even need to install C or a c compiler!
This is why  when you download a program, it often starts by downloading
an executable first, and you often have to pick the executable for
your operating system.

The disadvantage, *which we will come back to when talking about Java*,
is that this executable only works on my operating system, Windows.
Linux and Mac executable files are fundamentally different, and
so even a simple executable like helloWorld.c (which only prints "Hello, World!")
is not compatible with any other operating system. Now,
there are ways to do "cross-compiling", that is, compile for
a different environment than the one you are programming in,
like compiling a Linux executable in Windows, but now you have
to consider if your code will work on a given operating system.
In fact, you may at times see C code like this:

```c
#ifdef linux
   //do something with relation to linux
#elif _WIN32
   //do something for Windows 32
...

```

This limitation, that we can't make portable executables
is something Java was built to address.

## Interpreters

Some languages, like Python[^2], are typically *interpreted* 
rather than compiled. This is fundamentally the same idea as compiling,
only rather than turning the entire program into machine code first, 
and then running second, when code
is interpreted, we do both at the same time with the help of another program.

In Python, as you execute the program, the python interpreter translates
each line into machine instructions as you come to it. This means you don't have a static
compiled file typically.

## Translation Metaphor

> "Hooray for metaphors"
> - Sterling Archer, __Archer__, "Skytanic" Season 1 Episode 7 

The difference between compiling and interpreting is like this: imagine
you are a United Nations ambassador who is only fluent in English, and
a speaker is giving a speech in Spanish. There are two solutions:
* Wait for someone to publish a translated transcript (compiled)
* Use headphones to listen to a live translator who is translating the speech as
it is being made

The advantage of the first approach is that you have an easily
redistributable transcript of the speech that anyone who can read English
can use, regardless of whether or not they have a translator of their own. 
The disadvantage is you have a costly "compilation" process
to produce the translation, and you can't do anything until it is done.

The second approach has the advantage of translation on the fly, but
it's only possible with the help of a live translator actively working
in the background (python's interpreter). However, anyone without a translator
cannot do this approach.

Note that the above is an imperfect metaphor. **Do not make any assumptions about
which process is more efficient** from this metaphor; the point is only to explain
the difference between compiling and interpreting. It's worth noting that, in practice, 
interpreted languages tend to be a fair amount less efficient than compiled languages.
Specifically, C is routinely consider the fastest/most efficient language, which
is why it's so useful for low-level applications and embedded systems, while Python
is actually *dramatically* less efficient than C. Despite what you may have heard,
Java is actually quite efficient.


This is why you *don't* need to install c to run a program written in C, then compiled into an executable.
However, you *do* need to install Python to run Python programs. 

## How Java works (finally)


### JDK
Much like C, Java is a compiled language. When you compile a Java file, 
the Java Development Kit (JDK) compiles your code, producing a
.class file. The .class file which is the bytecode that specifies the machine instructions. Much
like the C executable, the compiled file is not, nor intended to be, human-readable.

<img alt="alt" src="{{site.baseurl}}/modules/java/images/3/compiler2.png"/>

However, unlike C, the Java .class file is **portable**. **Portable**, in the context
of software, means that you can move software easily from one environment to another
with minimal effort. In C, to move a program from Windows to Linux, you have to recompile
the program. In Java, you just send the .class file, no recompiling needed!

How does it do this? Well...first I need to acknowledge that the last figure is a lie.
A more accurate figure is this:

<img alt="alt" src="{{site.baseurl}}/modules/java/images/3/compiler3.png"/>

Specifically, the .class files don't run on any particular hardware. Rather, than
run in a specifically Java Runtime Environment (JRE).

### JRE

A **JRE** is an environment used to run Java Programs. The **JDK**, by contrast, is
use for *compiling*, not *running* Java programs. When you download a JDK, it will include
a compatible JRE. However, users can download a JRE to be able to run Java programs
without installing a JDK (non-developers wanting to run Java programs will often do this).


### JVM
Each JRE contains a **Java Virtual Machine (JVM)**. A *virtual machine* is software resource
that acts like a computer; virtual machines, functionally, have their own operating system, 
memory, processor, instruction set (machine language), etc.
However, each of these resources is effectively borrowed from the host (physical) machine.

What this means is that the JVM handles the direct interactions with the actual underlying hardware
(computer CPU, physical memory, disk, monitor, etc.), but from the perspective of the JRE, the
underlying physical hardware is irrelevant. 

It's worth noting that the JVM is actually an *interpreter*, that interprets the code it is
given at runtime. However, that code *is not java source code*, but rather .class *byte-code*
compiled by the JDK.

### Restaurant metaphor

> "Hooray for metaphors"
> - Sterling Archer, __Archer__, "Skytanic" Season 1 Episode 7

I like to think of the JRE as a waiter, and the JVM as the chef. A .class file looking
to be run walks into the restaurant, sits down, and starts asking the JRE to cook up a 
nice machine instruction for them. The JRE takes the order back to the chef (aka JVM), the JVM
uses the physical resources (oven, stove, fryer, etc.) to actually complete the order.

As far as the .class file is concerned, they don't care whether the restaurant is using
an electric or electric appliances (*think Mac or PC*), because the *chef* (aka JVM) handles
that. The JRE has a standardized interface (it doesn't matter who the waiter is, you
interact with them the same way), so the class file only needs to worry about interacting
with the JVM


### JIT

Obviously, one of the most important interactions the JVM has is with the processor, the
"brain" of the computer that actually handles executing instructions. As we mentioned before,
the JVM interprets Java byte-code. However, as we mentioned before, interpreted code is
inherently slower than directly executed machine instructions.

Making the process of interacting with the processor/memory/etc. as efficient as possible can lead to significant performance gains. 
*Enter the Just In Time compiler (JIT)* The JIT is part of the JVM, specifically the part that compiles the JVM byte code instructions
into machine code instructions for the underlying hardware. This natively compiled machine code,
machine code specifically compatible with the underlying physical hardware, is much more efficient than
interpreted Java byte code. The JIT can have numerous optimizations to increase performance, reduce
memory consumption, etc.

## Why Java is built this way

A key advantage of Java is portability. That is, an application built and compiled with a JDK on
a Windows computer will predictably behave the same way on a Linux and Mac computer
provided that both have a compatible JRE installed.

This allows us to share executables, without sharing underlying source code, of
apps. The JRE provides enough features to build meaningful and complex applications
that can be run on an hardware that has a compatible JVM.

The JIT provides several optimizations between a JVM and a specific 
underlying hardware architecture.

All of this together gives us convenience, distributability, portability, and performance.


## JVM: It's not just for Java anymore

The JVM can run any compatible byte code, and does not require a JDK, nor does it interact with it
directly. This fact allows for other programming languages to interact with the JVM, provided
they can compile correct .class byte-code. Two languages that do this are **Kotlin** and **Groovy**. 

**Kotlin** has been gaining rapid popularity in the Android community (as Android is a JVM
base system). The growth accelerated dramatically when, in 2019, [Google announced
that Kotlin was now its preferred language for Android app development](https://techcrunch.com/2019/05/07/kotlin-is-now-googles-preferred-language-for-android-app-development/).
I personally enjoy the language, and it's worth looking at (we will *very* briefly later on
in this class), as it cuts down on Java's length, removes some annoying features like checked exceptions,
and adds some useful features like null-safety.

**Groovy** similarly works on the JVM, and we will use a small amount of Groovy when working with
*Gradle* later on. However, beyond its use in Gradle as a domain-specific language (DSL), it never
gained the general-use popularity that Kotlin is now seeing.


# Key takeaways

This was a longer article, so I wanted to summarize the key takeaways:

* Compiling is taking a source code resource, and producing a bytecode resource
designed to run on a particular machine
* Interpreting is similar to compiling, but it done dynamically in real-time
* Java uses a JDK for compiling in bytecode that runs on a virtual machine
* The JRE runs java programs via a JVM
* The JVM interprets java bytecode into machine instructions of the underlying hardware
* The JIT is used to compile the java bytecode into machine instructions for the underlying hardware
at runtime, and boosts optimization and efficiency.


[^1]: Note for Windows users, I strongly recommend using Powershell rather than command-prompt, as Powershell uses pretty much the same commands as Mac and Linux, and helps standardize everything. Powershell comes installed with Windows 8 and later.


[^2]: A note that while Python is often referred to as an interpreted language, the truth is a little murkier. Python does, actually, "compile" code, though there is actually a compilation process in Python to translate instructions into bytecode