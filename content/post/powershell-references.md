+++
categories = ["Scripting", "Programming"]
title = "PowerShell Reference Types And Value Types"
description = "Breaking down PowerShell's reference types and value types."
date = "2015-06-26T15:29:00Z"
keywords = ["PowerShell, reference, value, type"]

+++
A coworker and I ran into something of an oddity yesterday that started the snowball rolling on some research digging into the guts of how PowerShell operates as a langauge. Let me say up front that, with as much as Microsoft is working to make PowerShell the de facto scripting language on Windows, it's both surprising and frustrating how little is published in the way of official documentation. While there's a lot of good stuff people are writing it's always in blogs of Stack Overflow discussions. Microsoft doesn't, at least at the time of this writing, have any official documentation like what I'd reference for [Ruby](http://ruby-doc.org/core-2.2.1/) or [Perl](http://perldoc.perl.org/perlapi.html).

The behavior which kicked off our confusion was how PowerShell handles passing parameters to functions. Take this very simple example:

    function Test
    {
        param($Value)
        $Value *= 2
    }
    
    $number = 3
    Test -Value $number
    $number

The output of this is just 3. The reason is that when I pass **$number** to the **Test** function, it's a *copy* of the value. Thus **$number** never changes. The behavior is different, however, when I pass an object. Take this example:

    function Test
    {
        param($Value)
        $Value.Add("Hello!")
    }
    
    $something = New-Object System.Collections.ArrayList
    Test -Value $something
    $something

The value printed to the screen here is "Hello!". The difference is that **$something** is being modified eventhough it's being passed to the function. That clearly *doesn't* happen when passing a non-object. What gives?

After quite a bit of digging, I failed miserably at finding anything of substance about this topic as it relates to PowerShell. That being said, I was able to find some very *good* information on the topic as it relates to C#, the language used to create PowerShell. In particular this [Stack Overflow](http://stackoverflow.com/questions/23041297/why-are-objects-automatically-passed-by-reference) thread and this [article](http://www.yoda.arachsys.com/csharp/parameters.html) really helped to clear things up for me.

If you want the super detailed explanation, read the content at those links. If you want the short version, however, read on. In C# there are two main sorts of variables: value types and reference types. For value type variables, the value of the variable is the data itself. In my first example, **$number** is a value type, and that means the value of it is 3. Reference types are different. For anything which is a reference type, the value of the variable is a *reference* to the data. So in my second example, **$something** has a value which is simply a reference to where the ArrayList lives. That's why passing **$something** to a function changes what **$something** actually contains; it's a reference that is being passed and the original data is being changed!

To simplify this a bit I can take functions out of the equation entirely. Consider the following example:

    $first = New-Object System.Collections.ArrayList
    $second = $first
    $second.Add("Hello!")
    $first

The output of this is "Hello!". When I set **$second** equal to **$first**, I'm setting **$second** equal to the reference to where the data of **$first** lives. Since **$first** and **$second** both reference the *same thing*, adding new data to **$second** means that **$first** is now different as well!

After realizing that this is how C# functions, it makes *much* more sense why PowerShell behaves the way it does. As a further test, I wanted to see if I could emulate the same behavior in PowerShell by using the `ref` keyword that C# leverages to pass primitives as references. Sure enough, that's possible as well:

    function Test
    {
        param([ref]$Value)
        $Value.Value *= 2
    }

    $number = 3
    Test -Value ([ref]$number)
    $number

Executing this gives me a value of 6. By using the `ref` keyword, I'm able to pass a reference to the function just like I would in C#. Thus the actual data stored by what **$number** references is now changed. Note that when performing the operation inside of my function, however, I can't just do that against **$Value**. The reason for this is that **$Value** is *only a reference*. If I want to do something against its data, I need to act on it's **Value** property.

Hopefully someone else stumbles across this post and is saved from having to spend a bit of time hunting for an explanation if they notice the same behavior in PowerShell that we did.
