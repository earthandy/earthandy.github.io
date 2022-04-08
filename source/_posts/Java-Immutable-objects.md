---
title: Java Immutable objects
date: 2022-02-26 17:57:50
keywords: Java,Immutable, Java Object, Java Immutable objects
categories:
- Java
tags:
- Object
---

### Introduce

Immutable objects are simply objects whose state (the object’s data) cannot change after construction. Examples of immutable objects from the JDK include String and Integer.

### Immutable Object Advantage

- are simple to construct, test, and use
- are automatically thread-safe and have no synchronization issues
- don’t need a copy constructor
- don’t need an implementation of clone
- allow hashCode to use lazy initialization, and to cache its return value
- don’t need to be copied defensively when used as a field
- make good Map keys and Set elements (these objects must not change state while in the collection)
- have their class invariant established once upon construction, and it never needs to be checked again
- always have “failure atomicity” (a term used by Joshua Bloch): if an immutable object throws an exception, it’s never left in an undesirable or indeterminate state

### How can I make a Object Immutable

- ensure the class cannot be overridden - make the class final, or use static factories and keep constructors private
- make fields private and final
- force callers to construct an object completely in a single step, instead of using a no-argument constructor combined with subsequent calls to setXXX methods (that is, avoid the Java Beans convention)
- do not provide any methods which can change the state of the object in any way - not just setXXX methods, but any method which can change state
- if the class has any mutable object fields, then they must be defensively copied when they pass between the class and its caller

**It’s interesting to note that BigDecimal is technically not immutable, since it’s not final.**

### Example

```java
import java.util.Date;

/**
* Planet is an immutable class, since there is no way to change
* its state after construction.
*/
public final class Planet {

  public Planet (double aMass, String aName, Date aDateOfDiscovery) {
     fMass = aMass;
     fName = aName;
     //make a private copy of aDateOfDiscovery
     //this is the only way to keep the fDateOfDiscovery
     //field private, and shields this class from any changes that 
     //the caller may make to the original aDateOfDiscovery object
     fDateOfDiscovery = new Date(aDateOfDiscovery.getTime());
  }

  /**
  * Returns a primitive value.
  *
  * The caller can do whatever they want with the return value, without 
  * affecting the internals of this class. Why? Because this is a primitive 
  * value. The caller sees its "own" double that simply has the
  * same value as fMass.
  */
  public double getMass() {
    return fMass;
  }

  /**
  * Returns an immutable object.
  *
  * The caller gets a direct reference to the internal field. But this is not 
  * dangerous, since String is immutable and cannot be changed.
  */
  public String getName() {
    return fName;
  }

//  /**
//  * Returns a mutable object - likely bad style.
//  *
//  * The caller gets a direct reference to the internal field. This is usually dangerous, 
//  * since the Date object state can be changed both by this class and its caller.
//  * That is, this class is no longer in complete control of fDate.
//  */
//  public Date getDateOfDiscovery() {
//    return fDateOfDiscovery;
//  }

  /**
  * Returns a mutable object - good style.
  * 
  * Returns a defensive copy of the field.
  * The caller of this method can do anything they want with the
  * returned Date object, without affecting the internals of this
  * class in any way. Why? Because they do not have a reference to 
  * fDate. Rather, they are playing with a second Date that initially has the 
  * same data as fDate.
  */
  public Date getDateOfDiscovery() {
    return new Date(fDateOfDiscovery.getTime());
  }

  // PRIVATE

  /**
  * Final primitive data is always immutable.
  */
  private final double fMass;

  /**
  * An immutable object field. (String objects never change state.)
  */
  private final String fName;

  /**
  * A mutable object field. In this case, the state of this mutable field
  * is to be changed only by this class. (In other cases, it makes perfect
  * sense to allow the state of a field to be changed outside the native
  * class; this is the case when a field acts as a "pointer" to an object
  * created elsewhere.)
  */
  private final Date fDateOfDiscovery;
}
```