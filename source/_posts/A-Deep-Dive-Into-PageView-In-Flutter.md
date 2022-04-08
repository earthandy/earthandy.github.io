---
title: A Deep Dive Into PageView In Flutter
date: 2022-01-28 23:01:29
summary: A PageView is a widget which generates scrollable pages on the screen.This can either be a fixed list of pages or a builder function which builds repeating pages.PageView acts similar to a ListView in the sense of constructing elements.
keywords: Flutter,PageView,Widgets,PageView Widgets
categories: Flutter
tags:
  - PageView
---

### Exploring PageViews

A **PageView** is a widget which generates **scrollable** pages on the screen.This can either be a fixed list of pages or a builder function which builds repeating pages.**PageView** acts similar to a **ListView** in the sense of constructing elements.

The types of PageView are:

1. **PageView**
2. **PageView.builder**
3. **PageView.custom**

### PageView (Default constructor)

This type takes a fixed list of children (pages) and makes them scrollable.

```dart
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: PageView(
        children: <Widget>[
          Container(
            color: Colors.pink,
            child: Center(
              child: Text('Page 1',
                  style: TextStyle(
                      fontSize: 35.0,
                      fontWeight: FontWeight.bold,
                      color: Colors.white)),
            ),
          ),
          Container(
            color: Colors.cyan,
            child: Center(
              child: Text('Page 2',
                  style: TextStyle(
                      fontSize: 35.0,
                      fontWeight: FontWeight.bold,
                      color: Colors.white)),
            ),
          ),
          Container(
            color: Colors.deepPurple,
            child: Center(
              child: Text('Page 3',
                  style: TextStyle(
                      fontSize: 35.0,
                      fontWeight: FontWeight.bold,
                      color: Colors.white)),
            ),
          ),
        ],
      ),
    );
  }
```

The above code produces the following result:

### PageView.builder

This constructor takes a **itemBuilder** function and an **itemCount** similar to **ListView.builder**.

```dart
PageView.builder(
 itemBuilder: (context,position){
  return _buildPage();_
 },
 itemCount: listItemCount,// Can be null
)
```

Like a *ListView.builder*,this builds children on demand.

If the *itemCount* is set to null (not set),an infinited list of pages can be generated.

*For example,this code:*

```dart
PageView.builder(
 itemBuilder: (context,position){
  return Container(
   color: position % 2 == 0? Colors.pink:Colors.cyan,
  );
 }
)
```

*Gives an infinite list of pages with alternating pink and cyan colors:*

> Note:*PageView.custom* works the same way as *ListView.custom* (Disussed in earlier)
> Deep Dive) and we will not be discussing it here.

### Orientation

All types of Page Views can have **horizontal** or **vertically** scrolling pages.

```dart
PageView(
 children: <Widget>[
  // Add children here
 ],
 scrollDirection: Axis.vertical,
)
```

The above code gives us:

### PageSnapping

Page Snapping allows us to keep the page at intermediate values,This is done by switching off the **pageSnapping** attribute.In this case the page will not scroll to an integer position and behave like a normal *ListView*.

```dart
PageView(
 children: <Widget>[
  //Add children here
 ],
 pageSnapping: false,
)
```

### ScrollPhysics

A *PageView* can have custom scrolling behavior in the same way as *ListViews*.
We will not repeat different types of [ScrollPhysics](https://api.flutter.dev/flutter/widgets/ScrollPhysics-class.html) since it is discussed in the ListView Deep Dive.

*ScrollPhysics* can be changed using the pyhsics parameter:

```dart
PageView(
 children: <Widget>[
  // Add children here
 ],
 physics: BoundcingScrollPhysics(),
)
```

### Controlling a PageView

A PageView can be programmatically controlled by attaching a **PageController**.

```dart
// OutSide build method
PageController controller = PageController();

// Inside builder method
PageView(
 controller: controller,
 children: <Widget>[
  // Add children here
 ]
)
```

The scroll position,current page,etc can be checked using the controller.

> **Note:**The *controller.currentPage* returns a double value.For example,when
> the page is being swiped the value goes from 1 to 2 gradually and does not
> instantly jump to 2.

### Add Custom Transitions to PageViews

Let’s discuss adding a few custom transition to the pages using **Transform** + **PageView**. This part will use the **Transform** widget extensively.

#### Transition 1

**The setup**

We first use a basic *PageView.builder*

```dart
PageView.builder(
 controller: controller,
 itemBuilder: (context,position){
 
 
 },
 itemCount: 10,
)
```

Let’s have 10 items for now.

We use a **PageController** and a variable which holds the value of the **currentPage**.

**Defining the PageController and variables:**

```dart
PageController controller = PageController();
var currentPageValue = 0.0;
```

**Updating the variable when the PageView is scrolled.**

```dart
controller.addListener((){
 setState((){
  currentPageValue = controller.page;
 })
}
)
```

Finally,we construct the **PageView**.

**Now,let’s check for three conditions:**

1. If the page is the page being swiped from
2. If the page is the page being swiped to
3. If the page is a page off screen

```dart
import 'package:flutter/material.dart';

class DeepDiveHome extends StatefulWidget {
  _DeepDiveHomeState createState() => _DeepDiveHomeState();
}

class _DeepDiveHomeState extends State<DeepDiveHome> {
  PageController controller = PageController();
  var currentPageValue = 0.0;

  @override
  Widget build(BuildContext context) {
    controller.addListener(() {
      setState(() {
        currentPageValue = controller.page;
      });
    });
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: PageView.builder(
        controller: controller,
        itemBuilder: (context, position) {
          return Container(
            color: position % 2 == 0 ? Colors.blue : Colors.pink,
            child: Center(
              child: Text(
                "Page",
                style: TextStyle(color: Colors.white, fontSize: 22.0),
              ),
            ),
          );
        },
        itemCount: 10,
      ),
    );
  }
}
```

Now we return the same page but wrapped in a **Transform** widget to transform our pages when we swipe it.

```dart
import 'package:flutter/material.dart';

class DeepDiveHome extends StatefulWidget {
  _DeepDiveHomeState createState() => _DeepDiveHomeState();
}

class _DeepDiveHomeState extends State<DeepDiveHome> {
  PageController controller = PageController();
  var currentPageValue = 0.0;

  @override
  Widget build(BuildContext context) {
    controller.addListener(() {
      setState(() {
        currentPageValue = controller.page;
      });
    });
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: PageView.builder(
        controller: controller,
        itemBuilder: (context, position) {
          if (position == currentPageValue.floor() ||
              position == currentPageValue.floor() + 1) {
            return Transform(
              transform: Matrix4.identity()
                ..rotateX(currentPageValue - position),
              child: Container(
                color: position % 2 == 0 ? Colors.blue : Colors.pink,
                child: Center(
                  child: Text(
                    "Page $position",
                    style: TextStyle(color: Colors.white, fontSize: 22.0),
                  ),
                ),
              ),
            );
          } else {
            return Container(
              color: position % 2 == 0 ? Colors.blue : Colors.pink,
              child: Center(
                child: Text(
                  "Page $position",
                  style: TextStyle(color: Colors.white, fontSize: 22.0),
                ),
              ),
            );
          }
        },
        itemCount: 10,
      ),
    );
  }
}
```

Here,we transform the page **being swipe from** and the **page being swiped to**.

**currentPageValue.floor()** gives us the page on the left and
**currentPageValue.floor()+1** gives us the page on the right

In this example,we rotate the page on the **X** direction as it is swiped by a value of **currentPageValue** minus the **index** in radiants.You can amplify the effect by multiplying this value.

We can tweak this transform and alignment of transform to give us multiple types
of new page transitions.

#### Transition 2

Similar code structure,just with a different transformation:

```dart
import 'package:flutter/material.dart';

class DeepDiveHome extends StatefulWidget {
  _DeepDiveHomeState createState() => _DeepDiveHomeState();
}

class _DeepDiveHomeState extends State<DeepDiveHome> {
  PageController controller = PageController();
  var currentPageValue = 0.0;

  @override
  Widget build(BuildContext context) {
    controller.addListener(() {
      setState(() {
        currentPageValue = controller.page;
      });
    });
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: PageView.builder(
        controller: controller,
        itemBuilder: (context, position) {
          if (position == currentPageValue.floor() ||
              position == currentPageValue.floor() + 1) {
            return Transform(
              transform: Matrix4.identity()
                ..rotateY(currentPageValue - position)
                ..rotateZ(currentPageValue - position),
              child: Container(
                color: position % 2 == 0 ? Colors.blue : Colors.pink,
                child: Center(
                  child: Text(
                    "Page $position",
                    style: TextStyle(color: Colors.white, fontSize: 22.0),
                  ),
                ),
              ),
            );
          } else {
            return Container(
              color: position % 2 == 0 ? Colors.blue : Colors.pink,
              child: Center(
                child: Text(
                  "Page $position",
                  style: TextStyle(color: Colors.white, fontSize: 22.0),
                ),
              ),
            );
          }
        },
        itemCount: 10,
      ),
    );
  }
}
```

Here,we rotate around both **Y** and **Z** axes.

#### Transition 3

This is a similar type of transition last time but with a 3-D effect added.

```dart
import 'package:flutter/material.dart';

class DeepDiveHome extends StatefulWidget {
  _DeepDiveHomeState createState() => _DeepDiveHomeState();
}

class _DeepDiveHomeState extends State<DeepDiveHome> {
  PageController controller = PageController();
  var currentPageValue = 0.0;

  @override
  Widget build(BuildContext context) {
    controller.addListener(() {
      setState(() {
        currentPageValue = controller.page;
      });
    });
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: PageView.builder(
        controller: controller,
        itemBuilder: (context, position) {
          if (position == currentPageValue.floor() ||
              position == currentPageValue.floor() + 1) {
            return Transform(
              transform: Matrix4.identity()
                ..setEntry(3, 2, 0.004)
                ..rotateY(currentPageValue - position)
                ..rotateZ(currentPageValue - position),
              child: Container(
                color: position % 2 == 0 ? Colors.blue : Colors.pink,
                child: Center(
                  child: Text(
                    "Page $position",
                    style: TextStyle(color: Colors.white, fontSize: 22.0),
                  ),
                ),
              ),
            );
          } else {
            return Container(
              color: position % 2 == 0 ? Colors.blue : Colors.pink,
              child: Center(
                child: Text(
                  "Page $position",
                  style: TextStyle(color: Colors.white, fontSize: 22.0),
                ),
              ),
            );
          }
        },
        itemCount: 10,
      ),
    );
  }
}
```

This line

```dart
..setEntry(3,2,0.004)
```

gives the pages a 3-D like effect.

```dart
import 'package:flutter/material.dart';

class DeepDiveHome extends StatefulWidget {
  _DeepDiveHomeState createState() => _DeepDiveHomeState();
}

class _DeepDiveHomeState extends State<DeepDiveHome> {
  PageController controller = PageController();
  var currentPageValue = 0.0;

  @override
  Widget build(BuildContext context) {
    controller.addListener(() {
      setState(() {
        currentPageValue = controller.page;
      });
    });
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: PageView.builder(
        controller: controller,
        itemBuilder: (context, position) {
          if (position == currentPageValue.floor() ||
              position == currentPageValue.floor() + 1) {
            return Transform(
              alignment: Alignment.center,
              transform: Matrix4.identity()
                ..setEntry(3, 2, 0.001)
                ..rotateY(currentPageValue - position)
                ..rotateZ(currentPageValue - position),
              child: Container(
                color: position % 2 == 0 ? Colors.blue : Colors.pink,
                child: Center(
                  child: Text(
                    "Page $position",
                    style: TextStyle(color: Colors.white, fontSize: 22.0),
                  ),
                ),
              ),
            );
          } else {
            return Container(
              color: position % 2 == 0 ? Colors.blue : Colors.pink,
              child: Center(
                child: Text(
                  "Page $position",
                  style: TextStyle(color: Colors.white, fontSize: 22.0),
                ),
              ),
            );
          }
        },
        itemCount: 10,
      ),
    );
  }
}
```

You can get it,Only let Transform add **alignment** parameter and change the **setEntry’s** value.A lot types can be created by simply changing rotation angles,axes,alignments and translations.

### Demo App using PageView

To demonstrate a simple app using **PageView** in Flutter,Somebody created an example app to study words for the GRE.This app displays and lets the user save the hardest words using SQLite to save them.It also has Text-To-Speech for pronouncing the word itself.

You can find this app [here](https://github.com/deven98/FlutterGREWords).
