---
title: A Deep Dive Into the Draggable&DragTarget Widget in Flutter
date: 2022-01-28 22:30:59
summary: Draggable and DragTarget allows us drag a widget across screen.First we will look at the basics of Draggables and DragTargets then dive into details of customizing them.
keywords: Flutter,Draggable,Widgets,DragTarget Widgets
categories: Flutter
tags:
  - Draggable
---

### Exploring Draggable and DragTargets

**Draggable** and **DragTarget** allows us drag a widget across screen.First we
will look at the basics of **Draggables** and **DragTargets** then dive into details
of customizing them.

![dragger_demo](https://s2.loli.net/2022/01/28/2HWGpLJr1hweUuE.gif)

### Draggable

A “Draggable” makes a widget movable around the screen.Let’s use a chess
knight as our example.

![dragger_games](https://s2.loli.net/2022/01/28/wWzj6AnXlsY4agC.gif)

The code for this is relatively straightforward:

```dart
Draggable(
 child:WhiteKnight(
  size:90.0,
 ),
 feedback:WhiteKnight(
  size:90.0,
 ),
 childWhenDragging:Container(),
)
```

There are three main parts to the **Draggable** widget:

1. **Child:** The child parameter is the widget that will be displayed when the
   draggable is stationary.
2. **Feedback:** The feedback is the widget that will be displayed when the
   widget is being dragged.
3. **Child When Dragging:** The childWhenDragging parameter takes the widget
   **to display in the original place of child** when the widget is being dragged.

In the example above,we have a white knight as the child.When the knight is being
dragged,we show an empty *Container* in its place.

#### Let’s customize the widget

First,let’s change the **feedback** parameter.

In the example below,we change the feedback parameter to a white bishop. Now,when the widget is not being dragged,a white knight is shown however as you start dragging,our bishop is shown.

```dart
Draggable(
 child:WhiteKnight(
  size:90.0,
 ),
 feedback:WhiteBishop(
  size:90.0,
 ),
 childWhenDragging:Container()
)
```

Result of the above code:
![draggable_feedback](https://s2.loli.net/2022/01/28/cU1vAyl2CGJBQYe.gif)

Now let’s move on to the third parameter,**childWhenDragging**.This takes a widget to be displayed in the original position of child as the widget is being dragged.

We’ll display a white rook when the widget is being dragged:

```dart
Draggable(
 child:WhiteKnight(
  size:90.0,
 ),
 feedback:WhiteBishop(
  size:90.0,
 ),
 childWhenDragging:WhiteRook(
  size:90.0,
 )
)
```

This becomes:

![draggable_childwhendragging](https://s2.loli.net/2022/01/28/MGEkBuhPjfT1Ivm.gif)

#### Restricting motion for Draggables

Setting the **axis** parameter helps us restrict motion in one dimension only. **Axis.horizontal** makes the feedback widget only move in the horizontal axis.

```dart
Draggable(
 axis: Axis.horizontal,
 child: WhiteKnight(
  size:90.0,
 ),
 feedback: WhiteBishop(
  size:90.0,
 ),
 childWhenDragging:Container()
)
```

![draggable_axis](https://s2.loli.net/2022/01/28/4lOLS2TojEpUNfG.gif)

Similarly,we also have **Axis.vertical** for vertical movement.

#### Adding data to Draggable

Data can be attached to a *Draggable*.This is useful to us since we use *Draggables* and *DragTargets*.

Considering our example,if we were to have multiple chess pieces,each piece would need to have unique data associated with it.Pieces would need properties such as color(White,Black) and type(ie:Rook,Bishop,Knight).

Below shows an example of how data can be attached to a *Draggable* for use with *DragTarget*.

We will take a look at this more in-depth in the **DragTarget** section.

```dart
Draggable(
 child:WhiteKnight(
  size:90.0,
 ),
 feedback:WhiteKnight(
  size:90.0,
 ),
 childWhenDragging:Container(),
 data:[
  'White',
  'Knight'
 
 ]
)
```

#### Callbacks

The *Draggable* widget supplies callbacks for actions on the widget.

These callbacks are:
**onDragStarted:** This is called when a drag is started on a widget.
**onDragEnd:** This is called when a drag is ended on a widget.
**onDragCompleted:** When a *Draggable* is dragged to a *DragTarget* and
accepted,this callback is called.We will look at *DragTarget* is in the next section.
**onDragCancelled:** When a *Draggable* does not reach a *DragTarget* or is rejected, this callback is fired.

### DragTarget

While a *Draggable* allows a widget to be dragged,a **DragTarget provides a destination for the draggable**.

For example,in chess,**a chess piece is a draggable** whereas **a square box on the chessboard is a drag target**.

![dragtarget_chess](https://s2.loli.net/2022/01/28/BIQkgFsKiOy3UNA.gif)

Here,the knight is accepted by the *DragTarget*

The code for this example goes:

```dart
bool accepted = false;

DargTarget(
  builder:(context,List<String> candidateData,rejectedData){
    return accepted? WhiteKnight(size:90.0) : Container();
  },
  onWillAccept: (data){
   return true;
  },
  onAccept:(data){
   accepted = true;
  }
)
```

**NB: The DragTarget is surrounded by a black colored container which is not shown in the code for brevity.**

Let’s look at the parameters of the **DragTarget** more detail.

**Note:** The “data” in the following sections refers to the **data** parameter of the *Draggable*.

#### Builder

The builder builds the actual widget inside the *DragTarget*.This function is called every time a *Draggable* is hovered over the *DragTarget* or is dropped onto it.

The function has three parameters,**context,candidateData** and **rejectedData**.

**candidateData** is the data of a *Draggable* whilst it is hovering over the *DragTarget*, ready for acceptance by *DragTarget*. **Note:** It’s List **Generics** must same the
Draggable’s *data* parameter’s data type.

**rejectedData** is the data of a *Draggable* hovering over a *DragTarget* at moment it is
not accepted.

**Note that both of there are dealing with Draggables which are hovering over the DragTarget. At this point,they are not dropped onto it yet.**

Now,how does the *DragTarget* know what to accept?

#### onWillAccept

**onWillAccept** is a function which gives us the *Draggable* data for us to decide whether to accept or reject it.

This is where the *data* we attached onto the *Draggable* becomes important. We use the data passed to accept or reject specific *Draggables*.

For example if our Draggable is:

```dart
Draggable(
 data:'Knight',
 //...
)
```

Then we can do:

```dart
DragTarget(
 //...
 onWillAccept:(data){
  return data == 'Knight';
 }
)
```

This will only accept the *Draggable* if the data is “Knight”.

This function is called when the *Draggable* is first hovered ovet the *DragTarget*.

#### onAccept

If the *Draggable* is dropped onto the *DragTarget* and *onWillAccept* returns true, then *onAccept* is called.

*onAccept* also gives us the data of the *Draggable* and is usually used for storing the *Draggable* dropped over *DragTarget*.

#### onLeave

This was not implemented in the example.*onLeave* is called when a *Draggable* is hovered over the *DragTarget* and leaves without being dropped.

### A Demo

Let’s create a simple demo app where the user is given a number and they are
required to sort it as either “even” or “odd”.Depending on the choice,we will
then display a message stating whether the choice is correct or wrong.

Source code:

```dart
import 'package:flutter/material.dart';
import 'dart:math' as math;

class DeepDiveHome extends StatefulWidget {
  _DeepDiveHomeState createState() => _DeepDiveHomeState();
}

class _DeepDiveHomeState extends State<DeepDiveHome>
    with TickerProviderStateMixin {
  GlobalKey<ScaffoldState> scaffoldKey = GlobalKey();
  int value;

  @override
  void initState() {
    value = math.Random().nextInt(10);
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      key: scaffoldKey,
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: <Widget>[
            Draggable(
              child: Container(
                width: 150,
                height: 150,
                color: Colors.pinkAccent,
                child: Center(
                  child: Text('$value',
                      style: TextStyle(
                          fontSize: 35,
                          color: Colors.white,
                          fontWeight: FontWeight.bold)),
                ),
              ),
              feedback: Container(
                width: 150,
                height: 150,
                color: Colors.pinkAccent,
                child: Center(
                  child: Text('$value',
                      style: TextStyle(
                          fontSize: 35,
                          color: Colors.white,
                          fontWeight: FontWeight.bold)),
                ),
              ),
              childWhenDragging: Container(
                width: 150,
                height: 150,
                color: Colors.pinkAccent,
                child: Center(
                  child: Text('$value',
                      style: TextStyle(
                          fontSize: 35,
                          color: Colors.white,
                          fontWeight: FontWeight.bold)),
                ),
              ),
              data: value,
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: <Widget>[
                Container(
                  height: 150,
                  width: 150,
                  color: Colors.green,
                  child: DragTarget(
                    builder: (BuildContext context, List<int> candidateData,
                        List rejectedData) {
                      return Center(
                        child: Text(
                          'Even',
                          style: TextStyle(
                              fontSize: 35,
                              fontWeight: FontWeight.bold,
                              color: Colors.white),
                        ),
                      );
                    },
                    onWillAccept: (data) {
                      return true;
                    },
                    onAccept: (data) {
                      if (data % 2 == 0) {
                        scaffoldKey.currentState.showSnackBar(SnackBar(
                          content: Text('Correct!'),
                        ));
                      } else {
                        scaffoldKey.currentState.showSnackBar(SnackBar(
                          content: Text('Wrong!'),
                        ));
                      }
                    },
                  ),
                ),
                Container(
                  height: 150,
                  width: 150,
                  color: Colors.deepPurpleAccent,
                  child: DragTarget(
                    builder: (BuildContext context, List<int> candidateData,
                        List rejectedData) {
                      return Center(
                        child: Text(
                          'Odd',
                          style: TextStyle(
                              fontSize: 35,
                              fontWeight: FontWeight.bold,
                              color: Colors.white),
                        ),
                      );
                    },
                    onWillAccept: (data) {
                      return true;
                    },
                    onAccept: (data) {
                      if (data % 2 != 0) {
                        scaffoldKey.currentState.showSnackBar(SnackBar(
                          content: Text('Correct!'),
                        ));
                      } else {
                        scaffoldKey.currentState.showSnackBar(SnackBar(
                          content: Text('Wrong!'),
                        ));
                      }
                    },
                  ),
                )
              ],
            )
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton.extended(
        icon: Icon(Icons.refresh),
        label: Text('Refresh Number'),
        onPressed: () {
          setState(() {
            value = math.Random().nextInt(10);
          });
        },
      ),
    );
  }
}
```

### A few more things

#### Draggable customizations

*Draggable* gives us a few more customizations.

One such customization is **maxSimultaneousDrags**.Multi-touch devicesare able to drag the same *Draggable* with two or more pointers and hence we can have multiple feedback on screen.For instance if the user is using a second finger to drag a child while one drag is already progress,we receive two feedback widgets at the same time.

**maxSimulataneousDrags** lets us decide the maximum number of times a *Draggable* can be dragged at one time.

The **feedbackOffset** property also lets us set the offset of the feedback widget that displays on the screen.

### Common error:DragTarget not accepting data

In the builder function,consider giving a type to the parameter **candidateData**.It returns a **List** by default,so if the data you’re passing is a string,the type would be **List**.

```dart
builder:(context,List<String> candidateData,rejectedData){ //...}
```

This solves a few type errors that occasionally comes up when passing data.

### A more complete real-world example using Draggable and DragTarget

Somebody wrote a complete chessboard package using *Draggables* and *DragTarget*.

For the complete source code,you can check it out [here](https://github.com/deven98/flutter_chess_board).
