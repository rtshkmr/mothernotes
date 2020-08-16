this document serves to be my personal takeaways from CS2103T. Here's the [overall textbook](https://nus-cs2103-ay2021s1.github.io/website/se-book-adapted/index.html) and here's the [course schedule/timeline](https://nus-cs2103-ay2021s1.github.io/website/schedule/timeline.html). 

- [Week 1: OOP Concepts, Java Collections & Exceptions Handling](#week-1-oop-concepts-java-collections--exceptions-handling)
  - [1.1: Classes & Objects](#11-classes--objects)
    - [*Objects as abstraction mechanisms*](#objects-as-abstraction-mechanisms)
    - [*Objects as an encapsulation of some data*](#objects-as-an-encapsulation-of-some-data)
    - [Class related syntax caveats](#class-related-syntax-caveats)
    - [Instance and Class Methods/Members/GettersAndSetters/](#instance-and-class-methodsmembersgettersandsetters)
    - [java enums](#java-enums)
  - [1.2: OOP Inheritance](#12-oop-inheritance)
  - [1.3: Polymorphism](#13-polymorphism)
    - [Abstract classes and methods](#abstract-classes-and-methods)
    - [Interfaces](#interfaces)
    - [Polymorphism and Substitutability](#polymorphism-and-substitutability)
  - [1.4 Java Collections](#14-java-collections)
  - [1.5: Exception Handling](#15-exception-handling)
- [Week 2: SDLC Processes, Version Control, IDE Basic Features & Automated Testing](#week-2-sdlc-processes-version-control-ide-basic-features--automated-testing)
  - [2.2 SDLC: Software Development Life Cycle (SDLC)](#22-sdlc-software-development-life-cycle-sdlc)
  - [2.3: RCS: Revision History](#23-rcs-revision-history)
  - [2.4: Remote Repos](#24-remote-repos)
  - [2.5: IDE Basic Features](#25-ide-basic-features)
  - [2.6: Automated Testing](#26-automated-testing)

# Week 1: OOP Concepts, Java Collections & Exceptions Handling

## 1.1: Classes & Objects

- three main programming paradigms: OOP, Procedural (e.g. C) , Functional and Logic (prolog) Programming
  - OOP uses objects, an abstraction on simple data structures (as used by Procedural paradigms), so OOP is "higher level" than Procedural
- some languages support multiple paradigms e.g. JS, python

### *Objects as abstraction mechanisms* 
it allows us to abstract away the lower level details and work with bigger granularity entities
- OOP views the world as a **network of interacting objects, w both state and behaviour**
- Objects having **Interfaces** and **Implementations** that support these interfaces
- Obj interactions done via sending messages, with sender objects, receiver objects, responses and state changes due to those..

### *Objects as an encapsulation of some data*
- for packaging of data
- for info hiding, info only accessible to the outside world via the object's interface


### Class related syntax caveats 

- `this` keyword:
  - can be used to refer to a constructor of a calss within the same class: 

    ```java
    // In this example the constructor Time() uses the this keyword to call its own 
    // overloaded constructor Time(int, int, int)
    public Time() {
        this(0, 0, 0); // call the overloaded constructor
    }

    public Time(int hour, int minute, int second) {
        // ...
    }
    ```

### Instance and Class Methods/Members/GettersAndSetters/

Just diff scope. Not everything that has a "total" identifier means it has to be class-level, see if it's truly aggregated data.

* `System.out.println(...)` : 
    * `out` is the class-level public attr of the `System` class 
    * `println` is an instance-level method of the `out` obj

### [java enums](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)
*  use enum types any time you need to represent a fixed set of constants.  Use it when a regular data type such as `int` or `String` would allow invalid values to be assigned to a variable.
*  enum declaration defines a class (called an enum type), only extends `java.lang.Enum`
   *  class body can have other methods and fields
   *  Java requires that the constants be defined first, prior to any fields or methods. Also, when there are fields and methods, the list of enum constants must end with a semicolon.
* [see ex on priority color](https://nus-cs2103-ay2021s1.github.io/website/schedule/week1/topics.html#key-exercise-show-priority-color)



## 1.2: OOP Inheritance

- just follow the usual *is-a* relationship intuitively. diagramatically, the child class points to the parent class to indicate that the child class extends from the parent class

**overloading and method/type signatures:**
* return types and param names are **NOT** part of the method/type signature
* what matters: param type, param order, method name... 

**overriding:**
* done at sub-classes, have same name, type signature and return type as the parent overridden method
* A subclass inherits **all the members** (fields, methods, and *nested classes*) from its superclass. **Constructors are not members**, so they are not inherited by subclasses, but the constructor of the superclass can be *invoked* from the subclass.
* `super` keyword:
  * access overridden method (parent method) 
  * access ***hidden field***:
    * when both the superclass and the subclass use the same variable name, the superclass variables is said to be hidden/shadowed by the subclass variable


* more on [subclass constructors](https://nus-cs2103-ay2021s1.github.io/website/schedule/week1/topics.html#subclass-constructors), somehow I forget this easily 



## 1.3: Polymorphism

> The ability of different objects to respond, each in its own way, to identical messages is called polymorphism.

### Abstract classes and methods

> **Abstract method**: An abstract method is a method signature without a method implementation.

* If a class includes abstract methods, then the class itself must be declared abstract.
  
* Abstract classes can be used as reference type but cannot be instantiated:
  ```java
  // abstact class Account
  Account a;         // OK to use as a type
  a = new Account(); // Compile error!
  ```
* a class that does not have any abstract methods can be declared as an abstract class.

* subclass of an abstract class either has implemention for all the abstract methods indicated in the superclass or the subclass is also an abstract class 

* see the java tut, more on [abstract classes vs interfaces](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html#abstract-classes-compared-to-interfaces) and their use case

### Interfaces 
* an interface is a** reference type**, similar to a class, mainly containing method signatures
  * can contain constants and static methods
  * can have [default method implementation](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and nested types
  * can't be instantiated, can only be implemented by classes
  * can be used as a type
  * interfaces can inherit from other interfaces (using `extends`)
  * *multiple inheritance among interfaces is allowed* (since subsequently a class can validly implement multiple interfaces)


### Polymorphism and Substitutability

> A subclass is an instance of the superclass but not vice-versa
  ```java
  Staff staffMember;
  staffMember = new AcademicStaff(); // valid as AcademicStaff :> Staff
  AcademicStaff acadStaffMember = staffMember; // invalid
  ```


## 1.4 Java Collections

**Collections/Container**: gropus multiple elems into a single unit, used to aggregate things

example of collections: set, list, queue, map...

## 1.5: Exception Handling

> The term exception is shorthand for the phrase "exceptional event." An exception is an event, which occurs during the execution of a program, that disrupts the normal flow of the program's instructions.

e.g. network connection timeout because of a slow server, or a corrupted file that we try to read

* All exceptions are **checked exceptions**, except for `Error`, `RuntimeException`, and their subclasses
  * are subject to ***Catch or Specify Requirement***: 
    * try statement with exception handler is required
    * method (header) must provide a throws claus that lists the exception

* **unchecked exceptions:** branch into `ERRORS` and `RuntimeException`
  * the ***Catch or Specify Requirement*** is not mandatory but can be put in if you want
  * `ERRORS`: exceptions indicated by `ERROR` an its subclasses. e.g. a system malfunction... 
  * **runtime exceptions:** usually indicate programming bugs, logic errors or wrong API use.  e.g. `NullPointerException`


* How things are handled: 
  * exceptional situation is encapsulated into an obj, then that can be raised/thrown so another piece of code can catch it and deal with it
    * Creating an exception object and handing it to the runtime system is called *throwing an exception*.
    * your catch block is your *exceptions handler*

* **finally block**: You can use a `finally` block to specify code that is guaranteed to execute with or without the exception. This is the right place to close files, recover resources, and otherwise clean up after the code enclosed in the try block.



# Week 2: SDLC Processes, Version Control, IDE Basic Features & Automated Testing

## 2.2 SDLC: Software Development Life Cycle (SDLC)

**SDLC:** stages include requirements, analysis, design, implementation and testing

Some SDLC models: 
* sequential/waterfall model: each step produces artifacts for the next
  * useful model when the problem statement that is well-understood and stable
  * major problem with this model is that requirements of a real-world project are rarely well-understood at the beginning and keep changing over time
* iterative/incremental: feeback drives the incremental improvements
  * **depth first iterative**
  * **breadth first iterative**


## 2.3: RCS: Revision History

[overview video](https://www.youtube.com/watch?v=v40b3ExbM0c)

**nb: they suggested using the win10 gui tool called source tree but let's use the cli instead**

* init

* staging and committing: 
   Staging allows us to decide which changes to put in what commit. Helps us organise our commits better.

   `stage` and `add` are synonymous

   ***see commit graph:*** use `gitk`, a rudimentary gui tool, or `git log --all --decorate --oneline --graph` 

* adding a .gitignore

* going thru the history: 
  * use the hash for the commit and checkout to that commit
    * can checkout using commit hash or tagname
      * can also `git chekout HEAD~2`: checks out to 2 commits behind HEAD
    * [***stashing***](https://www.atlassian.com/git/tutorials/saving-changes/git-stash): checking out without stashing uncommited changes will cause overwriting of files when the checkouts happen. so stash the uncommitted changes first so you can unstash it later!
      * stashes are locally stored, won't be pushed to remote location
      * reapply previously stashed changes: 
        * with `git stash pop`: pops it away from the stash
        * `git stash apply`: will retain it in the stash and reapplies the changes 
        * ***nb: git won't stash changes to untracked(unstaged)/ignored files***. can track changes to ignored files by using the `-a` option, so `git stash -a`. use `-u` for stashing untracked file changes
        * can have multiple stashes, use the link above to understand how to use it better
  * looking through changes: 
    * check differences using `diff`
      * no args: will show uncommitted changes since last commit
      * `git diff <hashpoint1>..<hashpoint2>` will show changes between the two points passed
      * `git diff <committed tag name>..HEAD`: from that tag to the most recent commit
    * seeing an indiv commit: `git show <commit hash>`
  * [tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging) your commit!:
    * `git tag -a <tag name>` : creates the tag, `a` flag is for annotation
    * `git tag` : to view all tags
    * `git push origin --tags` **remember to push your tags** 
    * **nb:** tagging can be done on a previous commits, just pass in the hash for that commit after the annotation string

## 2.4: Remote Repos

Git is not the only VCS, others like Mercurial exist. GitHub isn't the only hosting service, BitBucket(supports Mercurial too) and GitLab exist too.

**upstream repo:**
  *  original repo that you clone from is the upstream repo
     *  there can be multiple upstreams if they are cloned from each other
     *  can work w any number of other repos as long as they have a shared history among them (i.e. one was copied from the other at some point)
  *  cloning, pushing and pulling can be done b/w local repos too!
  
**forking**: involves creating a remote copy of another remote repo. Can be used to play around with code on repos for which you don't have write-access

**pull request**: involves requesting someone else to pull your changes onto their repo, most commonly works when you send PRs from a fork to its upstream repo.

**fetch vs pull**: 
  * pull = fetch + merge, so will move the head to the latest commit


## 2.5: IDE Basic Features

Web based IDEs exist, e.g. Amazon's [Cloud9 IDE](https://aws.amazon.com/cloud9/)


* [maiden run of intelliJ](https://www.youtube.com/watch?v=c0efB_CKOYo&feature=youtu.be)
* [intelliJ intro](https://www.youtube.com/watch?v=S764o0mAXhg&feature=youtu.be), see other videos from that channel if needed for things like debugging and stuff 
  

## 2.6: Automated Testing

Executing Test Cases: how to perform a test, at least the input to Software Under Test(SUT) and the expected behaviour. Elaborate test cases (w id, descrpition, objectives, classification info, cleanup instructions...)

Each Test Case: 
  1. feed input into SUT
  2. observe actual o/p
  3. compare actual vs expected o/p: a mismatch is a test case failure, indicates a bug *unless the test case is the error itself*


**Regression**: modifying a system might cause unintended and undesirable changes in other areas. That's why we need to do regression testing where the past test cases also have to be run. This requires testing to be automated!

^ **Test Driven Development (TDD)**

**basic automated testing**:
  * have an `input.txt` and a `expected.txt`
  * run the program like so: `$ java AdressBook < input.txt > output.txt` to store the o/p in a file
  * do a file comparison b/w the o/p and the expected o/p: `$ diff output.txt expected.txt`