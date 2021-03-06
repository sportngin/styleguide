---
layout: page
title: Swift
header: Swift Style Guide
group: navigation
---
{% include JB/setup %}

Introduction
-----------

Rather than create our own style guide for Swift, we decided to refer to
[The Official raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide) and to [Github's Swift Style Guide](https://github.com/github/swift-style-guide).  We will attempt to point out the conflicts or ambiguities in the two here.  The rationale is these style guides were compiled from many different sources and are widely accepted in the developer community as the de facto standard.

Developers are expected to read through and familiarize themselves with both style guides.

Conflicts
-----------

#### Access Controllers

The two style guides differ on their opinion of specifying the `internal` access control keyword for top level functions, types and variables.  The Ray Wenderlich style is preferred here.  `internal` is implied and the default access control, so there is no need to specify it.

#### Final Modifier

Github is pretty strict about starting all classes as `final` in order to avoid subclassing.  Ray Wenderlich's approach of only specifying `final` when there is intention to keep a class from being subclassed is preferred.  Although the intention of Github's style guide has a lot of merit, and composition over inheritance is preferred, it's a little much to go marking everything as `final`.  However, marking things 'private' that are not needed outside the class or type is strongly encouraged and comes in the same vein.


Cliff Notes
-----------

A few important things to make sure you have a good understanding of and also some interesting points.

#### Code Organization

[using protocol conformance](https://github.com/raywenderlich/swift-style-guide#code-organization
  ) from Wenderlich's style guide.  This is an interesting way to group code into logical blocks of functionality.  

#### Memory Management

Make sure [this section](https://github.com/raywenderlich/swift-style-guide#memory-management) makes sense and is understood.  

#### Golden Path

Proper use of [guard](https://github.com/raywenderlich/swift-style-guide#golden-path) really helps code readability.  

#### Type declaration

[Type Annotation for Empty Arrays and Dictionaries
.](https://github.com/raywenderlich/swift-style-guide#type-annotation-for-empty-arrays-and-dictionaries)  
