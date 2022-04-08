---
title: A Deep Dive Into Hero Widgets in Flutter
date: 2021-06-24 00:14:14
summary: A Hero animation in one sentence is simply an element of one screen “flying” to the next when the app goes to the next page.
keywords: Flutter,Hero,Widgets,Hero Widgets
categories: Flutter
tags:
  - Hero
---

### Introduction to Hero Animations
A Hero animation in one sentence is simply an element of one screen “flying” to the next when the app goes to the next page.

<img src="hero_animation.gif" width="50%"/>

Hero Animations take an element like an icon which is now called a “Hero“ and once the page transition is triggered,usually by clicking on the icon,the hero “files“ to the next page. When the user navigates back to the earlier page,the animation goes in the other direction and the icon goes back to its place.

We’ll dicuss not only basic hero animations but things we can customize about it. Let’s see the basics first.

### Creating a Basic Hero Animation
Hero animations are probably one of the easiest animations to do in Flutter and don’t require much setup. Taking a look at the example above,we can see that the same app icon widget exists on both pages,All we need is a way to tell Flutter that both of them are linked.

We do this by wrapping an element like an icon in a Hero widget.

```dart
Hero(
 tag:'DemoTag',
 child:Icon(
  Icons.add,
  size:70.0,
 )
)
```

We supply it a tag to give this specific hero a name. This is necessary because if we have multiple heros on the same page,each hero knows where to fly to. Now the app knows that there is a hero widget that wants to fly to the next page. Now all we need is a place to fly to. All we need is a Hero widget on the second page with the same tag.

```dart
Hero(
 tag:'DemoTag',
 child: Icon(
  Icons.add,
  size:150.0,
 )
)
```

And this results in:

<img src="hero_basic_animation.gif" width="50%" />

### Customizing Hero Animations
The Hero widget allows us to customize aspects of the animation. Let’s explore a few possibilities.

#### Adding placeholders
After the widget flies off the place it used to be in and before the widget arrives at the destination,there is empty space at the destination. We can add a placeholder to this location. Let’s use a CircularProgressIndicator as a placeholder for now.

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
      body: Container(
        child: Hero(
          tag: 'DemoTag',
          child: Icon(Icons.add, size: 100),
        ),
      ),
      floatingActionButton: FloatingActionButton.extended(
        icon: Icon(Icons.forward),
        label: Text('Hero'),
        onPressed: () {
          Navigator.push(context,
              MaterialPageRoute(builder: (context) => SomeOtherClass()));
        },
      ),
    );
  }
}

class SomeOtherClass extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Hero'),
      ),
      body: Center(
        child: Hero(
          child: Icon(
            Icons.add,
            size: 100.0,
          ),
          tag: 'DemoTag',
          placeholderBuilder: (context, widget) {
            return Container(
              width: 150.0,
              height: 150.0,
              child: CircularProgressIndicator(),
            );
          },
        ),
      ),
    );
  }
}
```

We use the placeholderBuilder to contruct the placeholder and return the widget we would like to have as the placeholder.

Using a placeholder:

<img src="hero_placeholder.gif" width="50%"/>

### Changing the Hero widget
Flutter allows us to change the widget which actually files from one page to the other without changing the widgets on the two pages.

Let’s use a rocket icon instead of a “+” icon as the hero without changing the children of the hero widgets. We do this using the flightShuttleBuilder parameter(Hence,the rocket example).

```dart
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';

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
      body: Container(
        child: Hero(
          tag: 'DemoTag',
          child: Icon(Icons.add, size: 100),
        ),
      ),
      floatingActionButton: FloatingActionButton.extended(
        icon: Icon(Icons.forward),
        label: Text('Hero'),
        onPressed: () {
          Navigator.push(context,
              MaterialPageRoute(builder: (context) => SomeOtherClass()));
        },
      ),
    );
  }
}

class SomeOtherClass extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Hero'),
      ),
      body: Center(
        child: Hero(
          child: Icon(
            Icons.add,
            size: 100.0,
          ),
          tag: 'DemoTag',
          flightShuttleBuilder:
              (flightContext, animation, direction, fromContext, toContext) {
            return Icon(
              FontAwesomeIcons.rocket,
              size: 150.0,
            );
          },
        ),
      ),
    );
  }
}
```

<img src="hero_rocket.gif" width="50%" />

The flightShuttleBuilder method has 5 parameters and gives us the animation as well as the direction of the animation.

For now,the rocket icon size stays at 150.0 for both directions. We can have different configurations for each direction by using the direction parameter of the method.

```dart
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';

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
      body: Container(
        child: Hero(
          tag: 'DemoTag',
          child: Icon(Icons.add, size: 100),
        ),
      ),
      floatingActionButton: FloatingActionButton.extended(
        icon: Icon(Icons.forward),
        label: Text('Hero'),
        onPressed: () {
          Navigator.push(context,
              MaterialPageRoute(builder: (context) => SomeOtherClass()));
        },
      ),
    );
  }
}

class SomeOtherClass extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Hero'),
      ),
      body: Center(
        child: Hero(
          child: Icon(
            Icons.add,
            size: 100.0,
          ),
          tag: 'DemoTag',
          flightShuttleBuilder:
              (flightContext, animation, direction, fromContext, toContext) {
            if (direction == HeroFlightDirection.push) {
              return Icon(
                FontAwesomeIcons.rocket,
                size: 150.0,
              );
            } else if (direction == HeroFlightDirection.pop) {
              return Icon(
                FontAwesomeIcons.rocket,
                size: 70.0,
              );
            }
          },
        ),
      ),
    );
  }
}
```

<img src="hero_direction.gif" width="50%" />

#### Making Hero animations work with IOS back swipe gestures
By default,hero animations work when the back button is pressed on IOS but they don’t work on back swipe. To solve this,simply set transitionOnUserGestures to true on both Hero widgets.

```dart
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';

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
      body: Container(
        child: Hero(
          tag: 'DemoTag',
          child: Icon(Icons.add, size: 100),
          transitionOnUserGestures: true,
        ),
      ),
      floatingActionButton: FloatingActionButton.extended(
        icon: Icon(Icons.forward),
        label: Text('Hero'),
        onPressed: () {
          Navigator.push(context,
              MaterialPageRoute(builder: (context) => SomeOtherClass()));
        },
      ),
    );
  }
}

class SomeOtherClass extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Hero'),
      ),
      body: Center(
        child: Hero(
          child: Icon(
            Icons.add,
            size: 100.0,
          ),
          tag: 'DemoTag',
          transitionOnUserGestures: true,
          flightShuttleBuilder:
              (flightContext, animation, direction, fromContext, toContext) {
            if (direction == HeroFlightDirection.push) {
              return Icon(
                FontAwesomeIcons.rocket,
                size: 150.0,
              );
            } else if (direction == HeroFlightDirection.pop) {
              return Icon(
                FontAwesomeIcons.rocket,
                size: 70.0,
              );
            }
          },
        ),
      ),
    );
  }
}
```

And this will trigger hero animations on back swipe as well.

<img src="hero_swipe.gif" width="50%" />



