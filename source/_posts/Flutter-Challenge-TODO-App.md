---
title: Flutter Challenge TODO App
date: 2022-01-28 23:50:46
top: true
cover: true
summary: This challenge is based on a design on Dribbble by Jae-song,Jeong.Flutter Challenges will attempt to recreate a particular app UI or design in Flutter.
keywords: Flutter,Challenge,TODO,TODO App
categories: Flutter
tags:
  - TODO
---

![todo](https://s2.loli.net/2022/01/28/TxizVj9dLcrOu72.png)

This challenge is based on a design on Dribbble by Jae-song,Jeong.

Link:[ https://dribbble.com/shots/3812962-iPhone-X-Todo-Concept](https://dribbble.com/shots/3812962-iPhone-X-Todo-Concept)

Flutter Challenges will attempt to recreate a particular app UI or design in Flutter.

![todo_ui_final](https://s2.loli.net/2022/01/28/P3ztK5yqwMaZC9L.png)

This challenge will attempt the home screen of the app design.Note that the foucs will be on the UI rather than actually fetching data from a backend.

Before starting,take a look at the design at the link given above or at [this link.](https://cdn.dribbble.com/users/1200964/screenshots/3812962/attachments/861450/todo_concept_iphonex_60fps.mp4)

### Understanding the app structure

The design is a concept for implementing a TODO app. It has three main parts:

1. The AppBar
2. The User Details
3. The Card List
4. The Color Transition when the list is scrolled

### Getting Started

Let’s create a Flutter Project named todo_app_ui and clear the default code until we’re left with this:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge TODO',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('TODO'),
        centerTitle: true,
      ),
      body: Center(),
    );
  }
}
```

We’ll go from top to bottom when recreating thr UI to make it simpler.

So first up,

### The App Bar

This is the AppBar of the design:

![todo_appbar](https://s2.loli.net/2022/01/28/8NOpeduoqXlF53z.png)

It consists of a drawer,a centered title and a search action.

Let’s try to code the appBar:

```dart
appBar: AppBar(
        title: Text(
          'TODO',
          style: TextStyle(fontSize: 16.0),
        ),
        backgroundColor: appColors[cardIndex],
        centerTitle: true,
        actions: <Widget>[
          Padding(
            padding: const EdgeInsets.only(right: 16.0),
            child: Icon(Icons.search),
          ),
        ],
        elevation: 0.0,
      ),
```

My appColors list holds the background colors needed for each card index.
The card index is simply the index of the card in focus,0 for now.

To add a drawer icon,simply add

```dart
drawer:Drawer(),
```

to the Scaffold. This not only adds a drawer icon to your AppBar,but also adds a fully functional drawer.

Also,I’ve set the Scaffold background color to the same color as the AppBar.

```dart
backgroundColor:appColors[cardIndex],
```

Here’s our AppBar:

![ourappbar](https://s2.loli.net/2022/01/28/ltbxHpXgRhKIU9y.png)

So far,so good.

### User Details

Below the AppBar is some text,welcoming them to the app and mentioning the number of things remaining on the list.

![todo_user_details](https://s2.loli.net/2022/01/28/3sjESUMhTaym6kw.png)

The layout is simple enough to understand:A Column of an Icon and 3 text elements.

The code of this part comes down to:

```dart
Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          Row(),
          Padding(
            padding:
                const EdgeInsets.symmetric(horizontal: 64.0, vertical: 32.0),
            child: Container(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: <Widget>[
                  Padding(
                    padding: const EdgeInsets.symmetric(vertical: 8.0),
                    child: Icon(Icons.account_circle,
                        size: 45.0, color: Colors.white),
                  ),
                  Padding(
                    padding: const EdgeInsets.fromLTRB(0.0, 16.0, 0.0, 12.0),
                    child: Text('Hello,Jane.',
                        style: TextStyle(
                          fontSize: 30.0,
                          color: Colors.white,
                        )),
                  ),
                  Text(
                    'Looks like feel good.',
                    style: TextStyle(
                      color: Colors.white,
                    ),
                  ),
                  Text(
                    'You have 3 tasks to do today.',
                    style: TextStyle(color: Colors.white),
                  )
                ],
              ),
            ),
          )
        ],
      ),
```

Note:That Row() was simply add to expand the Column to the full with of the phone.It has no other relevance to the layout.

Recreated user details and text:

![user_detail](https://s2.loli.net/2022/01/28/MP8SB9HqhT6tZIf.png)

Note:We’ll just combine the data text with the card list in a container,so we’ll add it later.

Moving on to the main part:

### The Card List

The Card List is a list of cards which denote the category of the notes(Person,Work, Home).To make this list,we’ll use a ListView.builder().

![card_list](https://s2.loli.net/2022/01/28/6FyBhlTzjIk2Pe4.png)

Let’s analyse the Card elements.

The Card has a Column with:

1. A Row for the icons
2. Two Texts for displaying number of tasks and category
3. Progress Bar for displaying the progress of the category of tasks

Also,in the design,the list does not scroll normally,rather it only scrolls on swipe and goes one position ahead or behind.So what we’ll do is,we’ll disable scrolling and write a GestureRecognizer to recognise swipes on cards and then go to the next card index.

Trying to recreate it:

```dart
ListView.builder(
      physics: NeverScrollableScrollPhysics(),
      itemCount: 3,
      controller: scrollController,
      itemBuilder: (context, position) {
        return GestureDetector(
          child: Padding(
            padding: const EdgeInsets.all(8.0),
            child: Card(
              child: Container(
                width: 250.0,
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: <Widget>[
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: <Widget>[
                          Icon(
                            cardsList[position].icon,
                            color: appColors[position],
                          ),
                          Icon(
                            Icons.more_vert,
                            color: Colors.grey,
                          ),
                        ],
                      ),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: <Widget>[
                          Padding(
                            padding: const EdgeInsets.symmetric(
                                horizontal: 8.0, vertical: 4.0),
                            child: Text(
                                '${cardsList[position].tasksRemaining} Tasks',
                                style: TextStyle(color: Colors.grey)),
                          ),
                          Padding(
                            padding: const EdgeInsets.symmetric(
                                horizontal: 8.0, vertical: 4.0),
                            child: Text(
                              '${cardsList[position].cardTitle}',
                              style: TextStyle(
                                fontSize: 28.0,
                              ),
                            ),
                          ),
                          Padding(
                            padding: const EdgeInsets.all(8.0),
                            child: LinearProgressIndicator(
                              value: cardsList[position].taskCompletion,
                            ),
                          )
                        ],
                      ),
                    )
                  ],
                ),
              ),
              shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(10.0)),
            ),
          ),
          onHorizontalDragEnd: (details) {
            if (details.velocity.pixelsPerSecond.dx > 0) {
              if (cardIndex > 0) {
                cardIndex--;
              } else {
                if (cardIndex < 2) {
                  cardIndex++;
                }
              }
            
            setState(() {
              scrollController.animateTo((cardIndex) * 256.0,
                  duration: Duration(milliseconds: 500),
                  curve: Curves.fastOutSlowIn);
            });
          }
        },
        );
      },
    )
```

We also have a scroll controller to animate the list which we initialise like this:

```dart
ScrollController scrollController;

@override
void initState(){
 super.initState();
 scrollController = ScrollController();
}
```

If you notice,the list has “physics” set to NeverScrollablePhysics. This inhibits scrolling of the list itself as we don’t want it to scroll. There is also a GestureDetector wrapped around the list recognise when the user swipes on the list. Take a look at the “horizontalDragEnd:” component.When the drag ends,we check if the swipe was a left swipe or right swipe and update the index accordingly.We then animate the list to scroll to that index in 500ms.I also have a cardList[] which stores the information about the cards and sets it.

Now let’s deal with animating the backgroundcolors when the card is swiped.

### The Color Transition

For a color transition we need three things:

1. An AnimationController to trigger an animation.
2. A CurvedAnimation to defined how the values change.
3. A ColorTween to define start and end colors.

```dart
AnimationController animationController;
ColorTween colorTween;
CurvedAnimation curvedAnimation;
```

Declare these variables above the initState() method.

Second,we declare a “currentColor” variable to store the current color of the background and also set the appBar and scaffoldBackground to currenColor.

The main change comes in the onHorizontalDragEnd method.When the horizontal swipe ends,we have to trigger the color change animation.

```dart
onHorizontalDragEnd:(details) {
 animationController =
        AnimationController(vsync: this, duration: Duration(milliseconds: 500));
    curvedAnimation = CurvedAnimation(
        parent: animationController, curve: Curves.fastOutSlowIn);
    animationController.addListener(() {
      setState(() {
        currentColor = colorTween.evaluate(curvedAnimation);
      });
    });
    if (details.velocity.pixelsPerSecond.dx > 0) {
      if (cardIndex > 0) {
        cardIndex--;
        colorTween = ColorTween(begin: currentColor, end: appColors[cardIndex]);
      }
    } else {
      if (cardIndex < 2) {
        cardIndex++;
        colorTween = ColorTween(begin: currentColor, end: appColors[cardIndex]);
      }
    }
    setState(() {
      scrollController.animateTo((cardIndex) * 256.0,
          duration: Duration(milliseconds: 500), curve: Curves.fastOutSlowIn);
    });
    colorTween.evaluate(curvedAnimation);
    animationController.forward();
}
```

We first initialise our animation controller,the animation itself and the color tween based on whether it was swiped left or right(If swiped left,we want to go from current to next color,whereas if it was swiped right we want to go from
current to the color before).

When the animation is running,we set the currentColor to the values of the animation.

The whole code is here:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge TODO',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      debugShowCheckedModeBanner: false,
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> with TickerProviderStateMixin {
  var appColors = [
    Color.fromRGBO(231, 129, 109, 1.0),
    Color.fromRGBO(99, 138, 223, 1.0),
    Color.fromRGBO(111, 194, 173, 1.0)
  ];
  var cardIndex = 0;
  ScrollController scrollController;
  var currentColor = Color.fromRGBO(231, 129, 109, 1.0);

  var cardsList = [
    CardItemModel("Personal", Icons.account_circle, 9, 0.83),
    CardItemModel("Work", Icons.work, 12, 0.24),
    CardItemModel("Home", Icons.home, 7, 0.32)
  ];

  AnimationController animationController;
  ColorTween colorTween;
  CurvedAnimation curvedAnimation;
  @override
  void initState() {
    super.initState();
    scrollController = ScrollController();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: currentColor,
      appBar: AppBar(
        title: Text(
          'TODO',
          style: TextStyle(fontSize: 16.0),
        ),
        backgroundColor: currentColor,
        centerTitle: true,
        actions: <Widget>[
          Padding(
            padding: const EdgeInsets.only(right: 16.0),
            child: Icon(Icons.search),
          ),
        ],
        elevation: 0.0,
      ),
      body: Center(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[Row(), buildHeader(), buildTaskCard()],
        ),
      ),
      drawer: Drawer(),
    );
  }

  Padding buildHeader() {
    return Padding(
      padding: const EdgeInsets.symmetric(horizontal: 64.0, vertical: 32.0),
      child: Container(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            Padding(
              padding: const EdgeInsets.symmetric(vertical: 8.0),
              child:
                  Icon(Icons.account_circle, size: 45.0, color: Colors.white),
            ),
            Padding(
              padding: const EdgeInsets.fromLTRB(0.0, 16.0, 0.0, 12.0),
              child: Text('Hello,Jane.',
                  style: TextStyle(
                    fontSize: 30.0,
                    color: Colors.white,
                  )),
            ),
            Text(
              'Looks like feel good.',
              style: TextStyle(
                color: Colors.white,
              ),
            ),
            Text(
              'You have 3 tasks to do today.',
              style: TextStyle(color: Colors.white),
            )
          ],
        ),
      ),
    );
  }

  Column buildTaskCard() {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        Padding(
          padding: const EdgeInsets.symmetric(horizontal: 64.0, vertical: 16.0),
          child:
              Text('TODAY: JUL 21,2018', style: TextStyle(color: Colors.white)),
        ),
        Container(
          height: 350,
          child: ListView.builder(
            physics: NeverScrollableScrollPhysics(),
            itemCount: 3,
            controller: scrollController,
            scrollDirection: Axis.horizontal,
            itemBuilder: (context, position) {
              return GestureDetector(
                child: Padding(
                  padding: const EdgeInsets.all(8.0),
                  child: Card(
                    child: Container(
                      width: 250.0,
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: <Widget>[
                          Padding(
                            padding: const EdgeInsets.all(8.0),
                            child: Row(
                              mainAxisAlignment: MainAxisAlignment.spaceBetween,
                              children: <Widget>[
                                Icon(
                                  cardsList[position].icon,
                                  color: appColors[position],
                                ),
                                Icon(
                                  Icons.more_vert,
                                  color: Colors.grey,
                                ),
                              ],
                            ),
                          ),
                          Padding(
                            padding: const EdgeInsets.all(8.0),
                            child: Column(
                              crossAxisAlignment: CrossAxisAlignment.start,
                              children: <Widget>[
                                Padding(
                                  padding: const EdgeInsets.symmetric(
                                      horizontal: 8.0, vertical: 4.0),
                                  child: Text(
                                      '${cardsList[position].tasksRemaining} Tasks',
                                      style: TextStyle(color: Colors.grey)),
                                ),
                                Padding(
                                  padding: const EdgeInsets.symmetric(
                                      horizontal: 8.0, vertical: 4.0),
                                  child: Text(
                                    '${cardsList[position].cardTitle}',
                                    style: TextStyle(
                                      fontSize: 28.0,
                                    ),
                                  ),
                                ),
                                Padding(
                                  padding: const EdgeInsets.all(8.0),
                                  child: LinearProgressIndicator(
                                    value: cardsList[position].taskCompletion,
                                  ),
                                )
                              ],
                            ),
                          )
                        ],
                      ),
                    ),
                    shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(10.0)),
                  ),
                ),
                onHorizontalDragEnd: handleDrag,
                onTap: () {
                  print(cardIndex);
                },
              );
            },
          ),
        )
      ],
    );
  }

  void handleDrag(details) {
    animationController =
        AnimationController(vsync: this, duration: Duration(milliseconds: 500));
    curvedAnimation = CurvedAnimation(
        parent: animationController, curve: Curves.fastOutSlowIn);
    animationController.addListener(() {
      setState(() {
        currentColor = colorTween.evaluate(curvedAnimation);
      });
    });
    if (details.velocity.pixelsPerSecond.dx > 0) {
      if (cardIndex > 0) {
        cardIndex--;
        colorTween = ColorTween(begin: currentColor, end: appColors[cardIndex]);
      }
    } else {
      if (cardIndex < 2) {
        cardIndex++;
        colorTween = ColorTween(begin: currentColor, end: appColors[cardIndex]);
      }
    }
    setState(() {
      scrollController.animateTo((cardIndex) * 256.0,
          duration: Duration(milliseconds: 500), curve: Curves.fastOutSlowIn);
    });
    colorTween.evaluate(curvedAnimation);
    animationController.forward();
  }
}

class CardItemModel {
  String cardTitle;
  IconData icon;
  int tasksRemaining;
  double taskCompletion;

  CardItemModel(
      this.cardTitle, this.icon, this.tasksRemaining, this.taskCompletion);
}
```
