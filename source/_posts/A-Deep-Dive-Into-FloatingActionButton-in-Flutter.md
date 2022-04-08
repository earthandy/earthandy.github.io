---
title: A Deep Dive Into FloatingActionButton in Flutter
date: 2021-06-23 18:57:38
summary: A FloatingActionButton in material design is a button on a screen that is tied to an obvious action which a user would usually do on that specific screen. This button floats above the content of the screen and usually resides on one corner of the screen.
keywords: Flutter,FloatingActionButton
categories: Flutter
tags:
  - FloatingActionButton
---

### Introduction to FloatingActionButton
A FloatingActionButton in material design is a button on a screen that is tied to an obvious action which a user would usually do on that specific screen. This button floats above the content of the screen and usually resides on one corner of the screen.

For example,in Gmail,the main screen has a FloatingActionButton to compose email. The purpose of the FAB must be something the user might frquently want or needs to do.

### Types of FloatingActionButton in Flutter
Flutter offers two types of FloatingActionButtons out-of-box.

1. FloatingActionButton
2. FloatingActionButton.extended

#### FloatingActionButton
The default constructor creates a simple circular FAB with a child widget inside it. It takes an onPressed method to react to taps and a child (not compulsory)to display a widget inside the FAB.

The code is relatively straightforward and is most often used with the floatingActionButton parameter of the Scaffold widget.

```dart
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: Container(),
      floatingActionButton: FloatingActionButton(
        onPressed: () => {},
        child: Icon(Icons.add),
      ),
    );
  }
```

#### FloatingActionButton.extended
FloatingActionButton.extended offers a wide FAB,usually with an icon and a label inside it.

Instead of a child parameter,we now hava label and icon paramters.

```dart
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Deep Dive Flutter'),
        ),
        body: Container(),
        floatingActionButton: FloatingActionButton.extended(
            onPressed: () {}, icon: Icon(Icons.save), label: Text('Save')));
  }
```

### Dealing with Colors in a FloatingActionButton

The FAB offers the foregroundColor and backgroundColor parameters to color the elements inside it.

**The foregroundColor colors the child inside the FAB**

```dart
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Deep Dive Flutter'),
        ),
        body: Container(),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(Icons.add),
          foregroundColor: Colors.red,
        ));
  }
```

**The backgroundColor colors the background of the button**

```dart
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Deep Dive Flutter'),
        ),
        body: Container(),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(Icons.add),
          backgroundColor: Colors.pink,
        ));
  }
```

**Note: Individual element colors will override the foregroundColor attribute.**

If the icon inside has a user defined color and a foregroundColor at the same time,the icon color will override the foregroundColor. 

```dart
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Deep Dive Flutter'),
        ),
        body: Container(),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(
            Icons.add,
            color: Colors.yellow,
          ),
          foregroundColor: Colors.pink,
        ));
  }
```

The “+” icon turns yellow even with foregroundColor specifying pink.

### Changing the Look of the FloatingActionButton
The FAB gives us multiple options to resize and change the look of the button.

#### Switching to a mini button
In material design,the FAB has two sizes:default and mini. To switch to a mini size,set mini in the constructor to true.

```dart
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Deep Dive Flutter'),
        ),
        body: Container(),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(
            Icons.add,
          ),
          mini: true,
        ));
  }
```

#### Changing the shape of the FAB
To change the shape of the button,we set the shape parameter in the constructor. The default value of the shape is a CircleBorder().

Let’s make a square button instead of a round one.

```dart
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Deep Dive Flutter'),
        ),
        body: Container(),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(
            Icons.add,
          ),
          shape: RoundedRectangleBorder(),
        ));
  }
```

If you notice,it says RoundedRectangleBorder but it behaves like a square. This is because we haven’t supplied a boardRadius.

Supplying a border radius:

```dart
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Deep Dive Flutter'),
        ),
        body: Container(),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(
            Icons.add,
          ),
          shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.all(Radius.circular(16.0))),
        ));
  }
```

RoundedRectangleBorder and other border classes also have another field called side which let’s us edit the color and width of the border.

```dart
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Deep Dive Flutter'),
        ),
        body: Container(),
        floatingActionButton: FloatingActionButton(
            onPressed: () {},
            child: Icon(
              Icons.add,
            ),
            shape: RoundedRectangleBorder(
              side: BorderSide(color: Colors.pink, width: 4.0),
            )));
  }
```

You can also define your own border class to do custom borders.

You can do something like a DiamondBorder class and simple set the border to it. This example is taken from the Flutter Gallery application.

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
        body: Container(),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(
            Icons.add,
          ),
          shape: _DiamondBorder(),
        ));
  }
}

class _DiamondBorder extends ShapeBorder {
  const _DiamondBorder();

  @override
  EdgeInsetsGeometry get dimensions => const EdgeInsets.only();

  @override
  Path getInnerPath(Rect rect, {TextDirection textDirection}) {
    return getOuterPath(rect, textDirection: textDirection);
  }

  @override
  Path getOuterPath(Rect rect, {TextDirection textDirection}) {
    return Path()
      ..moveTo(rect.left + rect.width / 2.0, rect.top)
      ..lineTo(rect.right, rect.top + rect.height / 2.0)
      ..lineTo(rect.left + rect.width / 2.0, rect.bottom)
      ..lineTo(rect.left, rect.top + rect.height / 2.0)
      ..close();
  }

  @override
  void paint(Canvas canvas, Rect rect, {TextDirection textDirection}) {}

  @override
  ShapeBorder scale(double t) {
    return null;
  }
}

```

Or you can just go crazy with it and do a random shape like this one. All you need is to define a new type of ShapeBorder.

#### Changing the elevation of FAB
We can change the effect of elevation of the FAB above the screen by setting the elevation and highlightElevation property.Elevation works at all times while highlightElevation is the elevation of button when tapped.

Let’s set elevation to 0.0:

And then 20.0:

You can see the shadow changing around the button indicating an inrease in distance in the z-axis.

### Animating the FloatingActionButton
This section is meant to cover some animations related to FloatingActionButtons.

#### Defalut FAB animation in page transitions
Before we go into something we can implement,I want to mention the default animation Flutter has when navigating between pages which both have a FloatingActionButton. When transitioning form one page to the other,Flutter will automatically animate the FAB change instead of switching instantly. Because the FAB is a Hero.  This requires no code on the user side.

IMPOETANT: Hero Animations with FloatingActionButtons OR Why can’t I have two FloatingActionButtons on the same pages?

This upper animation is possible because the FloatingActionButton has a Hero widget weapped around it. A Hero is an element that flies to the next page when a transition occurs.

A **Hero** widget requires a tag that correlates two separate elements on two different pages and then makes one transition to the other.

The Hero tag has to be unique. Since each FAB is a Hero,defining two of them on the same page is not allowed without explicitly channging the hero tag of one of them.

Hence,defining two FABs on one page is not allowed. If we want to have two or more FABs,we need to set the heroTag parameter to another value.

```dart
Widget build(BuildContext context) {
   return Scaffold(
       appBar: AppBar(
         title: Text('Deep Dive Flutter'),
       ),
       body: Center(
         child: Container(
           child: FloatingActionButton(
             onPressed: () {},
             child: Icon(Icons.save),
           ),
         ),
       ),
       floatingActionButton: FloatingActionButton(
         onPressed: () {},
         child: Icon(
           Icons.add,
         ),
         heroTag: 'demoValue',
       ));
 }
```

Now,we can hava another FAB on the same page as well.

Since we just said each FAB is a Hero,we can use the FAB to create Hero animations.

The code FAB’s heroTag should be same the second page widget’s which also warp with a Hero widget. The code would be:

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
        body: Container(),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            Navigator.push(context,
                MaterialPageRoute(builder: (context) => SomeOtherClass()));
          },
          child: Icon(
            Icons.add,
          ),
          heroTag: 'demoTag',
        ));
  }
}

class SomeOtherClass extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Hero FAB'),
      ),
      body: Center(
        child: Hero(
          child: Icon(
            Icons.add,
            size: 100.0,
          ),
          tag: 'demoTag',
        ),
      ),
    );
  }
}
```

This makes the button transition to the icon.Note that now,a FAB on the second page would not affect us as we have changed the default Hero tag. So even if we had a FAB on the second page,the defalut transition would not take place.

There are a few other animations we can do with a FAB but those fall within the realm of Scaffold and we’ll cover those when we get to the Scaffold deep dive.
