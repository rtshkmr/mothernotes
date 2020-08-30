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
- [Week 3: Branching, FileIO, Code Organisation using Packaging](#week-3-branching-fileio-code-organisation-using-packaging)
  - [3.1: RCS: Branching](#31-rcs-branching)
  - [3.3: RCS: Creating Pull Requests](#33-rcs-creating-pull-requests)
  - [3.3: Javadoc](#33-javadoc)
  - [3.4: File IO in Java](#34-file-io-in-java)
  - [3.5: Packages and Code Organisation](#35-packages-and-code-organisation)
  - [3.6: JAR: archived java files](#36-jar-archived-java-files)
  - [3.7 Good Quality Code & Coding Standards](#37-good-quality-code--coding-standards)
  - [3.8: Developer Testing & Unit Testing](#38-developer-testing--unit-testing)
    - [Test Automation Tools: JUnit](#test-automation-tools-junit)
      - [Basic JUnit Testing](#basic-junit-testing)
      - [Stubs and others](#stubs-and-others)
- [Week 4: Modelling, JavaFX, Varargs, Code Analysis and Review](#week-4-modelling-javafx-varargs-code-analysis-and-review)
  - [4.2: Class/Object Diagams [UML Models]](#42-classobject-diagams-uml-models)
    - [OO Structures and Class Structures](#oo-structures-and-class-structures)
    - [Overview of Class/Object Diagrams:](#overview-of-classobject-diagrams)
    - [Object vs Class Diagrams](#object-vs-class-diagrams)
  - [4.3 Intermediate Features of UML Diagrams](#43-intermediate-features-of-uml-diagrams)
  - [4.4: JavaFX: GUI programming](#44-javafx-gui-programming)

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


**nb:** member is just an umbrella term for anything in a class (but constructors are not members)

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

easy way to do the testing: run your code and see that it will fail after some new feature / change has been added. Just see the change by eye and see if it works. 


# Week 3: Branching, FileIO, Code Organisation using Packaging 

## 3.1: RCS: Branching

Allows us to create isolated situations to safely create new features without affecting the master branch. 

We can work on multiple branches at the same time as well and this helps us work on multiple features simultaneously without their effects affecting one another. 

You can update a branch to the sync with the  merge commits done with the other branches to the master branch by merging the master branch to the branch you're working on. This might create a merge conflict. Now we can merge it back to the master branch when we're done with the current branch that we were working on.

*rmb to switch* to 

**merging**: results in a new commit to represent the commit (this allows a commit that indicates your deconflicting of merge conflicts), representing the changes done in the branch being merged. Git may realise that it can do a **fast-forward mege** by not creating that extra commit and this behaviour is defaulted to, in such cases we need to explicitly ask it to make a merge commit when merging. prevent fastforwarding like so: `git merge --no-ff <branch name>`


nb: when merging,you should checkout to the receiving branch first then do the merging. applies for syncing master to yoru branch as well as merging your own branch to master as well... 


**syncing branches**: e.g. while working on your branch, some urgent bug fix has been done by some other branch. Now we sync our current branch by:

*  merging master to our branch. Just keep syncronising to the latest changes from master branch.
*  doing a `rebase`. i.e. rebases the current branch you're working on and pretend like you're branching it off of the latest changes on the master branch. this is a little more advanced, use with caution. Rebasing will cause new hash commits, might mess up push changes and you'd have to do a force-push 


## 3.3: RCS: Creating Pull Requests 

While making the PR, we have to choose the correct destination and sources. If your repo has multiple upstreams, then the default ones may not be right, so take note. 
  * less common to send PRs using `master` branch, more likely to be done from within a feature branch...
**updating the PR** Note that while a PR review is in process, commiting to the branch that you submitted for PR will automatically update the code in the PR. 

**creating PRs within the same repo**: that's possible, allows other devs to approve the chage via the PR review mechanism.

[see github documentation on PRs](https://help.github.com/articles/creating-a-pull-request/)


## 3.3: Javadoc

just start using javadoc already. 

[short tutorial on javadocs](https://www.tutorialspoint.com/java/java_documentation.htm)
[detailed oracle document](https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html)

## 3.4: File IO in Java

see `java.io.File`. within your java code, even on windows, your file path separator can be `/` as in unix. else can also use `\\` but rem to use 2 slashes.

[file io techniques using other classes](https://www.tutorialspoint.com/java/java_files_io.htm)



* **LOADING AND READING:**  create a `File` object and read lines in it using a `Scanner`, 
* **WRITING FILE**: use `FileWriter` object, which has a `writeToFile` method... 
example: 

  ```java
  import java.io.FileWriter;
  import java.io.IOException;
  import java.io.File;
  import java.io.FileNotFoundException;
  import java.util.Scanner;

  public class FileReadingDemo {

      private static void printFileContents(String filePath) throws FileNotFoundException {
          File f = new File(filePath); // create a File for the given file path
          Scanner s = new Scanner(f); // create a Scanner using the File as the source
          while (s.hasNext()) {
              System.out.println(s.nextLine());
          }
      }

    private static void writeToFile(String filePath, String textToAdd) throws IOException {
        FileWriter fw = new FileWriter(filePath); 
        //^ open in append mode by passing in true as another argument
        fw.write(textToAdd);
        fw.close();
    }

      public static void main(String[] args) {
          try {
              printFileContents("data/fruits.txt");
          } catch (FileNotFoundException e) {
              System.out.println("File not found");
          }

        String file2 = "temp/lines.txt";
          try {
              writeToFile(file2, "first line" + System.lineSeparator() + "second line");
          } catch (IOException e) {
              System.out.println("Something went wrong: " + e.getMessage());
          }
        }

  }

  ```
## 3.5: Packages and Code Organisation

* package classes via a putting a **a single**  package statement at the top of the file. 
  * lowercase (no camelCase) and separate folders via dot separators
* importing packages won't import its subpackages, even if we use a wildcard (wildcard will import all the packages in that package but not subpackages). **packages don't behave as hierarchies**
* *static imports* exist that allows us to import specific members of a type

* running/compiling needs to take the packaging into account 

^ a little unclear on this actually QQ



## 3.6: JAR: archived java files 

delivery method is usually JAR, contains classes and other resources/assets important to the program.

[read this tut when needed](https://se-education.org/guides/tutorials/jar.html)

## 3.7 Good Quality Code & Coding Standards

Many topics under this are pretty subjective and opinionated. 

Be open-minded about code-quality pls.

Be a good follower. 

Just follow the prescribed coding style. 

rmb to config the IDE to be coding-standard compliant.

## 3.8: Developer Testing & Unit Testing

Ref to testing by Devs. Diff from QA engineers testing a system.

Do early testing

**Test Driver:**  drives the SUT for the purpose of testing. 

### Test Automation Tools: JUnit

Unit testing:  testing individual units (methods, classes, subsystems, ...) to ensure each piece works correctly


[three pillars of unit tests](https://blog.aspiresys.pl/technology/three-pillars-of-unit-tests/)

[some apple case study](https://avandeursen.com/2014/02/22/gotofail-security/)

#### Basic JUnit Testing

Create a matching Test Class using JUnit, with the methods you wanna try out.

Naming convention for testcase methods: `whatIsBeingTested_descriptionOfTestInputs_expectedOutcome`

[testing on exception handling](https://howtodoinjava.com/junit5/expected-exception-example/)


[JUnit5 user guide](https://junit.org/junit5/docs/current/user-guide/)

[testing private methods: short desc](http://stackoverflow.com/questions/34571/whats-the-proper-way-to-test-a-class-with-private-methods-using-junit) [testing private methods: longer desc](http://www.artima.com/suiterunner/private.html)


#### Stubs and others 

Stubs isolate an SUT from its dependencies. Allows testing to be done in isolation, so that dependencies are never the reason for the test to be influenced. 

isolating meaning: ` If a Logic class depends on a Storage class, unit testing the Logic class requires isolating the Logic class from the Storage class.`

Stubs mirror the Class it's a stub for, so that it's the same interface as the one you're making the stub for. Aim: so simple that it's unlikely to contain any bugs



# Week 4: Modelling, JavaFX, Varargs, Code Analysis and Review

Modelling out software, the same application has many aspects (classes, structures, behaviours, interactions, various architectures...) and having a 
standardised modelling scheme is useful for this.

Also look at Model-driven development! 

## 4.2: Class/Object Diagams [UML Models]

[UML Modelling Language](https://en.wikipedia.org/wiki/Unified_Modeling_Language)

Class Diagrams will have Object Diagrams

### OO Structures and Class Structures 

 a network of objects interacting with each other. Therefore, it is useful to be able to model how the relevant objects are 'networked' together inside a software

 Class Structures determine how the objects are connected, the OO strutures can change over time 


### Overview of Class/Object Diagrams:
**Class Diagram:**
  - UML class diagrams describe the structure (but not the behavior) of an OOP solution
  - Underlining: 
    - class level attributes and variables underlined
    - underline the object model's title 
  - Comparments: 
    - **Attributes Compartment** and **Operations compartment** <--- may be omitted if doesn't value add
    - all things in these compartments, keep it in the same compartment okay 
    - can show empty compartments
  - fields have data type indicated, for collections just describe as: `routes:Route[]`
    - **either show it as an association to classes, or as an attribute, but not both, preferrable to use lines to show class association instead of putting it as an attribute of the containing class**
  - field visibility: `+` for public and `-` for private


  - **Associations** shown with a line joined to the two (implemented by using instance level attributes): 
    - multiplicility relationship indicated on the line itself, use wildcard, single digit or range of digits to indicate this multiplicity
    - **Association Decorations:**
      - **Roles**: a specific type of association, e.g. longest_route
      - **multiplicity** indication applies to roles also (if multiple routes of the same length, then there will be multiple `longest_route`)
        - `0..1` is optional multiplicity 
        - `1` multiplicity aka compulsary associations
        - **bidirectional associations**: require matching variables in both classes 
        - for other multiplicity associations, use the right Data Structures
      - **Labels**:  tells us which direction to describe the associatio (which direction to read?) 
      - **Navigability**: given a bunch of objects, which classes can navigate to others' (keeping reference off). can be bidirectional, unidirectional, can be transitive, **represented by arrowed lines (arrowhead on your association line)**
        - note that classes linked by association doesn't automatically mean that they know about each other, navigability is a separate concept

- class diagrams represent the relationships forever, a set of rules that exist forever 
**Object Diagram:** An object diagram shows an object structure at a given point of time.
  - `<objectname(optional)>:<Classname>`  <--- underlined, the name is optional 
  - Object Structure associations will change in time, any of these time moments can be represented as a snapshot of sorts
  - have no compartment for methods
  - multiplicities also omitted 
  - a single class diagram can have multiple object diagrams (cuz diff points in time)



**nb:** can have association from one class to the same class: just get the multiplicity part right for the association, e.g. person with role of parents


### Object vs Class Diagrams 

## 4.3 Intermediate Features of UML Diagrams 

**Inheritance**: `triangle arrowhead on the parent`

**Composition**: `filled diamond on the larger one`whole part
  * whole-part relationship
  * implies no cyclical links 
  *  keep track of it for data integrity. when the whole is deleted, the parts must be deleted too (books and chapters)
  * cascading deletion alone isn't enough for something to be considered part of a whole, it needs to be an integral part 

**Aggregation**: `empty diamond on the container`container-containee r/s, it's a little weaker. We are advised not to bring in aggregation 
  * deleting container doesn't mean need to delete containee

**Dependency**: a dependency is a need for one class to depend on another without having a direct association with it. represent with a `dashed dependency arrow`
  - needs to be met for compilation to happen
  - normal association has permanent link: holds reference but dependency, temporary dependency, uses it but doesn't keep references
    - e.g. need to read data in a XX model to do some work every once in a while


**Association classes:** Attaching behaviours into associations: use a separate class that represent the association. `single dashed line pointing to the association` 
  * use it to store data about an association, e.g. `Man` an `Woman` having an association of Marriage, can consider having `Marriage` as a class.

**Abstract Classes**: `{abstract}` keyword with braces 

**Interfaces:** `<<interface>>`

**Interface Implementations:** `dashed version of inheritance`

**Constraints:** put within a note, put it `{within curly braces}` use [OC(Object Constraint Language)](https://en.wikipedia.org/wiki/Object_Constraint_Language)

**Associations as Attributes:** `name: type [multiplicity] = default value`

**Enumrations:** `<<enumeration>>` notation shall be used, treat it as a class of its own

**Model notes**: freestanding or dashed line for attaching it to something 


## 4.4: JavaFX: GUI programming












