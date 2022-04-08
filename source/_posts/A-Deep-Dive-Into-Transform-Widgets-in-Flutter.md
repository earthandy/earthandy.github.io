---
title: A Deep Dive Into Transform Widgets in Flutter
date: 2021-06-24 00:49:55
summary: A Transform widget “transforms“ (i.e. chanages the shape,size,position,and orientation) its child widget before painting it.Let’s start with the types of Transform widgets.
keywords: Flutter,Transform,Widgets,Transform Widgets
categories: Flutter
tags:
  - Transform
---

### Introduction to the Transform widget
A Transform widget “transforms“ (i.e. chanages the shape,size,position,and orientation) its child widget before painting it.

Distorting the shape of the container using Transform: This is extrememly useful for custom shapes and many different kinds of
animations in Flutter. This can be used to transform any widget and distort it to any shape we like or move it around as well.

Let’s start with the types of Transform widgets.

### Exploring the types of Transform widget
The Transform widget gives us a few constructors to help simplify the certain of transformations. Common operations such as scaling,rotation or translation are all provided via constructors.

The types of Transform widgets are:

1. Transform(default constructor)
2. Transform.rotate
3. Transform.scale
4. Transform.translate

We will look at the single-operation constructors first.

#### Transform.rotate
As the name suggests,Transform.rotate simply rotates the child by an angle. Here the child is a square container.

```dart
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: Transform.rotate(
        angle: 1.0,
        child: Center(
          child: Container(
            height: 200.0,
            width: 200.0,
            color: Colors.pink,
          ),
        ),
      ),
    );
  }
```

The angle parameter let’s set an angle(in radians) the child will be rotated by. The widget also allows us specify an origin for the rotation of our widget. We specify the origin using the origin parameter. This takes an Offset. The Offset notes the distance of the origin in relation to the center of the child itself. When we don’t need explicitly set the offset however,the child rotates around its own center.

I made an animation to better visualize where the center of rotation lies.

<img src="transform_rotate_origin_default.gif" width="50%"/>

If the child is rotated form the lower right point of the square,it would look like this:

The code for the above rotation is:

```dart
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: Transform.rotate(
        angle: 1.0,
        origin: Offset(50.0, 50.0),
        child: Center(
          child: Container(
            height: 100.0,
            width: 100.0,
            color: Colors.blue,
          ),
        ),
      ),
    );
  }
```

The square has sides of 100.0.This code rotates the container about the lower right vertex of the square.Since the offset is 50.0 to the right and 50.0 below,the center of the child is defined by the offset(50.0.,50.0).

#### Transform.scale
The scale constructor scales the child by the given scale parameter. The container when scaled to half its original size:

```dart
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: Transform.scale(
        scale: 0.5,
        child: Center(
          child: Container(
            height: 100.0,
            width: 100.0,
            color: Colors.yellow,
          ),
        ),
      ),
    );
  }
```

Here we set the scale to 0.5 to reduce the size of the container by half. Similar to the rotation transform,we can also set an origin in scaling. When the origin is left blank,the center of the widget is taken. When scaling, the changes on each side are exactly the on each. When no origin is provided for scaling:

Like the last example,let’s now set the origin to the lower right corner. If we now reload our app,the widget will scale differently compared to the default (the center).

This is achieved bu providing the origin(50.0,50.0).This offset corresponds to the lower right corner.

```dart
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: Transform.scale(
        scale: 0.5,
        origin: Offset(50.0, 50.0),
        child: Center(
          child: Container(
            height: 100.0,
            width: 100.0,
            color: Colors.blue,
          ),
        ),
      ),
    );
  }
```

#### Transform.translate
Transform.translate translates the child of the transform widget by a specified amount in the X and Y direction. We supply an Offset which has the amount by which we want to move the child by in the X and Y directions.

Container offset by 100.0 in the X axis:

```dart
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: Transform.translate(
        offset: Offset(100.0, 0.0),
        child: Center(
          child: Container(
            height: 100.0,
            width: 100.0,
            color: Colors.blue,
          ),
        ),
      ),
    );
  }
```

Here we supply an Offset which moves the containers 100.0 in the X direction. Any changes in the second parameter would move it in the Y direction.

We cannot set an origin in translation since translation is not affected by origin. Since we give an offset and not coordinates,the origin doesn’t make a difference.

#### Transform(Default constructor)
Unlike the other constructors in the list,the default constructor allows us to do multiple operations at once. It is the most powerful constructor from the list.

Instead of taking a specific parameter like an angle or scaling,this constructor takes a 4D Matrix directly in the transform parameter. This allows us to carry out multiple operations.

An example of this is:

```dart
Transform(
        transform: Matrix4.skewY(0.3)..rotateZ(3.14 / 12.0),
        origin: Offset(50.0, 50.0),
        child: Container(
          height: 100.0,
          width: 100.0,
          color: Colors.blue,
        ),
      ),
```

The above code gives us a skew around the Y axis of 0.3 and a rotation of π/12 radians arounf the Z axis.

A Skew is essentially titling the child in one direction while keeping the sides parallel.

**SkewX**

**SkewY**

Matrix4 also lets us set rotation and translation. We use Matrix4.rotationX(),rotationY() and rotationZ() for rotation and Matrix4.translation() or Matrix4.translationValues() for translation.

We can also set an origin in this constructor using the origin parameter like we did in the first two constructors.

### Now let’s apply pointless transformations to normal Flutter widgets

> “Ok Flutter,do a barrel roll“

```dart
import 'package:flutter/material.dart';
import 'dart:math' as math;

class DeepDiveHome extends StatefulWidget {
  _DeepDiveHomeState createState() => _DeepDiveHomeState();
}

class _DeepDiveHomeState extends State<DeepDiveHome>
    with SingleTickerProviderStateMixin {
  Animation<double> animation;
  AnimationController controller;

  @override
  void initState() {
    super.initState();
    controller =
        AnimationController(duration: Duration(seconds: 5), vsync: this);
    animation = Tween<double>(begin: 0.0, end: 2 * math.pi).animate(controller)
      ..addListener(() {
        setState(() {});
      });
    controller.forward();
  }

  @override
  Widget build(BuildContext context) {
    return Transform.rotate(
      angle: animation.value,
      child: Scaffold(
        appBar: AppBar(
          title: Text('Deep Dive Flutter'),
        ),
        body: Center(
          child: Text('Transform Demo', style: TextStyle(fontSize: 30)),
        ),
      ),
    );
  }

  @override
  void dispose() {
    controller.dispose();
    super.dispose();
  }
}
```

### Extra large FAB
For some reason,this looks better than the normal FloatingActionButton.

```dart
import 'package:flutter/material.dart';

class DeepDiveHome extends StatefulWidget {
  _DeepDiveHomeState createState() => _DeepDiveHomeState();
}

class _DeepDiveHomeState extends State<DeepDiveHome> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: Center(
        child: Text('Transform Demo', style: TextStyle(fontSize: 30)),
      ),
      floatingActionButton: Transform.scale(
        scale: 4,
        child: FloatingActionButton(
          child: Icon(
            Icons.add,
          ),
          onPressed: () {},
        ),
      ),
    );
  }
}
```



