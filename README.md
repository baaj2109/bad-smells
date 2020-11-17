

# Code Smell

## Bloaters
Bloaters are code, methods and classes that have increased to such gargantuan proportions that they’re hard to work with. Usually these smells don’t crop up right away, rather they accumulate over time as the program evolves (and especially when nobody makes an effort to eradicate them).
### long method
method longer than ten lines should make you start asking questions.
####Treatment
* To reduce the length of a method body, use "Extract Method".

* If local variables and parameters interfere with extracting a method,  use "Replace Temp with Query"

* try moving the entire method to a separate object via "Replace Method with Method Object".

* Conditional operators and loops are a good clue that code can be moved to a separate method. For conditionals, use "Decompose Conditional".

####Payoff
* Among all types of object-oriented code, classes with short methods live longest. The longer a method or function is, the harder it becomes to understand and maintain it.

* In addition, long methods offer the perfect hiding place for unwanted duplicate code.

### large class
A class contains many fields/methods/lines of code.
####Treatment
* Extract Class helps if part of the behavior of the large class can be spun off into a separate component.

* Extract Subclass helps if part of the behavior of the large class can be implemented in different ways or is used in rare cases.

* Extract Interface helps if it’s necessary to have a list of the operations and behaviors that the client can use.

####Payoff
* Refactoring of these classes spares developers from needing to remember a large number of attributes for a class.

* In many cases, splitting large classes into parts avoids duplication of code and functionality.

### primitive obsession 基本型別偏執
Use of primitives instead of small objects for simple tasks (such as currency, ranges, special strings for phone numbers, etc.)
####Treatment
* If you have a large variety of primitive fields, try Replace Data Value with Object.

* If the values of primitive fields are used in method parameters, go with "Introduce Parameter Object" or "Preserve Whole Object".

* When complicated data is coded in variables, use "Replace Type Code with Class", "Replace Type Code with Subclasses" or "Replace Type Code with State/Strategy".

* If there are arrays among the variables, use Replace Array with Object.

####Payoff
* Code becomes more flexible thanks to use of objects instead of primitives.

* Better understandability and organization of code. Operations on particular data are in the same place, instead of being scattered. No more guessing about the reason for all these strange constants and why they’re in an array.

* Easier finding of duplicate code.

### long parameter list
More than three or four parameters for a method.

####Treatment
* if some of the arguments are just results of method calls of another object, use Replace Parameter with Method Call. 

* Instead of passing a group of data received from another object as parameters, pass the object itself to the method, by using Preserve Whole Object.

* If there are several unrelated data elements, sometimes you can merge them into a single parameter object via Introduce Parameter Object.

####Payoff
* More readable, shorter code.

* Refactoring may reveal previously unnoticed duplicate code.

### data clumps 資料泥團
Sometimes different parts of the code contain identical groups of variables (such as parameters for connecting to a database)
總是一起出現的資料
####Treatment
* If repeating data comprises the fields of a class, use Extract Class

* If the same data clumps are passed in the parameters of methods, use Introduce Parameter Object 

* If some of the data is passed to other methods, think about passing the " to the method instead of just individual fields.

####Payoff
* Improves understanding and organization of code. Operations on particular data are now gathered in a single place, instead of haphazardly throughout the code.

* Reduces code size.


## object-orientation abusers
### switch statements
You have a complex switch operator or sequence of if statements.
####Treatment
* To isolate switch and put it in the right class, you may need Extract Method and then Move Method.

* If a switch is based on type code, such as when the program’s runtime mode is switched, use Replace Type Code with Subclasses or Replace Type Code with State/Strategy.

* If a switch is based on type code, such as when the program’s runtime mode is switched, use Replace Type Code with Subclasses or Replace Type Code with State/Strategy.

* If one of the conditional options is null, use Introduce Null Object.

* If there aren’t too many conditions in the operator and they all call same method with different parameters, polymorphism will be superfluous. If this case, you can break that method into multiple smaller methods with Replace Parameter with Explicit Methods and change the switch accordingly.

####Payoff
* Improved code organization.

####When to Ignore
* When a switch operator performs simple actions, there’s no reason to make code changes.

* Often switch operators are used by factory design patterns (Factory Method or Abstract Factory) to select a created class.

### temporary field
Temporary fields get their values (and thus are needed by objects) only under certain circumstances. Outside of these circumstances, they’re empty.
####Treatment
* Introduce Null Object and integrate it in place of the conditional code which was used to check the temporary field values for existence.
* Temporary fields and all code operating on them can be put in a separate class via Extract Class.

####Payoff
* Better code clarity and organization.

### refuesed bequest 被拒絕的遺贈
If a subclass uses only some of the methods and properties inherited from its parents, the hierarchy is off-kilter. The unneeded methods may simply go unused or be redefined and give off exceptions.
####Treatment
* If inheritance makes no sense and the subclass really does have nothing in common with the superclass, eliminate inheritance in favor of Replace Inheritance with Delegation.

* Extract all fields and methods needed by the subclass from the parent class, put them in a new subclass, and set both classes to inherit from it (Extract Superclass).

####Payoff
* Improves code clarity and organization. You will no longer have to wonder why the Dog class is inherited from the Chair class (even though they both have 4 legs).


### alternative classes with different interfaces
Two classes perform identical functions but have different method names.
####Treatment
* Rename Methods to make them identical in all alternative classes.

* Move Method, Add Parameter and Parameterize Method to make the signature and implementation of methods the same.

* If only part of the functionality of the classes is duplicated, try using Extract Superclass. In this case, the existing classes will become subclasses.
 
####Payoff
* You get rid of unnecessary duplicated code, making the resulting code less bulky.

* Code becomes more readable and understandable (you no longer have to guess the reason for creation of a second class performing the exact same functions as the first one).



## change preventers
### divergent change 發散式修改
many changes are made to a single class.
####Treatment
* Split up the behavior of the class via Extract Class.

* If different classes have the same behavior, you may want to combine the classes through inheritance (Extract Superclass and Extract Subclass).

####Payoff
* Improves code organization.

* Reduces code duplication.

* Simplifies support.

### shotgun surgery
 Shotgun Surgery refers to when a single change is made to multiple classes simultaneously.
####Treatment
* Use Move Method and Move Field to move existing class behaviors into a single class. If there’s no class appropriate for this, create a new one.

* If moving code to the same class leaves the original classes almost empty, try to get rid of these now-redundant classes via Inline Class.
####Payoff
* Better organization.

* Less code duplication.

* Easier maintenance.

### parallel inheritance hierarhies
Whenever you create a subclass for a class, you find yourself needing to create a subclass for another class.

####Treatment
* You may de-duplicate parallel class hierarchies in two steps. First, make instances of one hierarchy refer to instances of another hierarchy. Then, remove the hierarchy in the referred class, by using Move Method and Move Field.
####Payoff
* Reduces code duplication.

* Can improve organization of code.


## dispensables
### comments
A method is filled with explanatory comments.
####Treatment
* If a comment is intended to explain a complex expression, the expression should be split into understandable subexpressions using Extract Variable.
* If a comment explains a section of code, this section can be turned into a separate method via Extract Method. 
* If a method has already been extracted, but comments are still necessary to explain what the method does, give the method a self-explanatory name. Use Rename Method for this.
* If you need to assert rules about a state that’s necessary for the system to work, use Introduce Assertion.

####Payoff
* Code becomes more intuitive and obvious.
* If the same code is found in two subclasses of the same level:


### duplicate code
Two code fragments look almost identical.
####Treatment
* If the same code is found in two or more methods in the same class: use Extract Method and place calls for the new method in both places.
+ If the same code is found in two subclasses of the same level:
	+ Use Extract Method for both classes, followed by Pull Up Field for the fields used in the method that you’re pulling up.
	+ If the duplicate code is inside a constructor, use Pull Up Constructor Body.
	+ If the duplicate code is similar but not completely identical, use Form Template Method.
	+ If two methods do the same thing but use different algorithms, select the best algorithm and apply Substitute Algorithm.
+ If duplicate code is found in two different classes:
	+ If the classes aren’t part of a hierarchy, use Extract Superclass in order to create a single superclass for these classes that maintains all the previous functionality.
	+ If it’s difficult or impossible to create a superclass, use Extract Class in one class and use the new component in the other.

* If a large number of conditional expressions are present and perform the same code (differing only in their conditions), merge these operators into a single condition using Consolidate Conditional Expression and use Extract Method to place the condition in a separate method with an easy-to-understand name.
* If the same code is performed in all branches of a conditional expression: place the identical code outside of the condition tree by using Consolidate Duplicate Conditional Fragments.

####Payoff
* Merging duplicate code simplifies the structure of your code and makes it shorter.

* Simplification + shortness = code that’s easier to simplify and cheaper to support.


### lazy class
Understanding and maintaining classes always costs time and money. So if a class doesn’t do enough to earn your attention, it should be deleted.

####Treatment
* Components that are near-useless should be given the Inline Class treatment.
* For subclasses with few functions, try Collapse Hierarchy.

####Payoff
* Reduced code size.

* Easier maintenance.


### dataclass
A data class refers to a class that contains only fields and crude methods for accessing them (getters and setters). T
####Treatment
* If a class contains public fields, use Encapsulate Field to hide them from direct access and require that access be performed via getters and setters only.

* Use Encapsulate Collection for data stored in collections (such as arrays).

* Review the client code that uses the class. In it, you may find functionality that would be better located in the data class itself. If this is the case, use Move Method and Extract Method to migrate this functionality to the data class.

* After the class has been filled with well thought-out methods, you may want to get rid of old methods for data access that give overly broad access to the class data. For this, Remove Setting Method and Hide Method may be helpful.


####Payoff
* Improves understanding and organization of code. Operations on particular data are now gathered in a single place, instead of haphazardly throughout the code.

* Helps you to spot duplication of client code.

### dead code
A variable, parameter, field, method or class is no longer used (usually because it’s obsolete).

####Treatment
* Delete unused code and unneeded files.

* In the case of an unnecessary class, Inline Class or Collapse Hierarchy can be applied if a subclass or superclass is used.

* To remove unneeded parameters, use Remove Parameter.

####Payoff
* Reduced code size.

* Simpler support.

### speculative generality
There’s an unused class, method, field or parameter.

####Treatment
* For removing unused abstract classes, try Collapse Hierarchy.

* Unnecessary delegation of functionality to another class can be eliminated via Inline Class.

* Unused methods? Use Inline Method to get rid of them.

* Methods with unused parameters should be given a look with the help of Remove Parameter.

* Unused fields can be simply deleted.



####Payoff
* Slimmer code.

* Easier support.

## couplers
### feature envy
A method accesses the data of another object more than its own data.


####Treatment
* If a method clearly should be moved to another place, use Move Method.

* If only part of a method accesses the data of another object, use Extract Method to move the part in question.

* If a method uses functions from several other classes, first determine which class contains most of the data used. Then place the method in this class along with the other data. Alternatively, use Extract Method to split the method into several parts that can be placed in different places in different classes.

####Payoff
* Less code duplication (if the data handling code is put in a central place).

* Better code organization (methods for handling data are next to the actual data).

### inappropriate intimacy
One class uses the internal fields and methods of another class.


####Treatment
* The simplest solution is to use Move Method and Move Field to move parts of one class to the class in which those parts are used. But this works only if the first class truly doesn’t need these parts.
* Another solution is to use Extract Class and Hide Delegate on the class to make the code relations “official”.

* If the classes are mutually interdependent, you should use Change Bidirectional Association to Unidirectional.

* If this “intimacy” is between a subclass and the superclass, consider Replace Delegation with Inheritance.

####Payoff
* Improves code organization.

* Simplifies support and code reuse.


### message chains
In code you see a series of calls resembling $a->b()->c()->d()


####Treatment
* To delete a message chain, use Hide Delegate.

* Sometimes it’s better to think of why the end object is being used. Perhaps it would make sense to use Extract Method for this functionality and move it to the beginning of the chain, by using Move Method.


####Payoff
* Reduces dependencies between classes of a chain.

* Reduces the amount of bloated code.


### middle man
If a class performs only one action, delegating work to another class, why does it exist at all?


####Treatment
* If most of a method’s classes delegate to another class, Remove Middle Man is in order.

####Payoff
* Less bulky code.



## other smells
### Incomplete Library Class
Sooner or later, libraries stop meeting user needs. The only solution to the problem—changing the library—is often impossible since the library is read-only.





####Treatment
* To introduce a few methods to a library class, use Introduce Foreign Method.

* For big changes in a class library, use Introduce Local Extension.

####Payoff
* Reduces code duplication (instead of creating your own library from scratch, you can still piggy-back off an existing one).
