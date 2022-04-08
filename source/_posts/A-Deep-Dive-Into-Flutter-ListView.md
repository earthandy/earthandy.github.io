---
title: A Deep Dive Into Flutter ListView
date: 2021-06-22 22:41:46
summary: We’ll start with looking at the types of ListViews and later look at the other features and neat modifications for it.
keywords: Flutter,ListView
categories: Flutter
tags:
  - ListView
---

> A ListView in Flutter is a linear list of scrollable items. We can use it to make a list of items scrollable or make a list of repeating items.

### Exploring the types of ListView
We’ll start with looking at the types of ListViews and later look at the other features and neat modifications for it.

Let’s look at the types of ListViews there are:

1. ListView
2. ListView.builder
3. ListView.separated
4. ListView.custom

Let’s go around exploring these types one by one:

#### ListView
This is the default constructor of the ListView class. A ListView simply takes a list of children and makes it scrollable.

A List contructed using the default constructor:

The general format of the code is:

```dart
ListView(
 children:<Widget>[
  ItemOne(),
  ItemTwo(),
  ItemThree(),
 ],
)
```

Usually this should be used with a small number of children as the List will also construct the invisible elements in the list and a large amount of elements may render this inefficient.

#### ListView.builder()
The builder() constructor constructs a repeating list of items. The constructor takes two main parameters:An itemCount for the number of items in the list and an itemBuilder for constructed each list item.

A List contructed using the builder() constructor:

The general format of the code is:

```dart
ListView.builder(
 itemCount:itemCount,
 itemBuilder:(context,position){
  return listItem();
 }
)
```

The list items are constructed lazily,meaning only a specific number of list items are constructed and when a user scrolls ahead,the earlier ones are destroyed.

Neat trick: Since the elements are loaded lazily and only the needed number of elements are loaded,we don’t really need an itemCount as a compulsory parameter and the list can be infinite.

```dart
ListView.builder(
 itemBuilder:(context,position){
  return Card(
   child:Padding(
    padding:const EdgeInsets.all(16.0),
    child: Text(position.toString(),style:TextStyle(fontSize:22.0))
   ),
  );
 }
)
```

A ListView without the itemCount parameter:

#### ListView.separated()
In the separated() constructor,we generate a list and we can specify the separator between each item.

A ListView constructed using the ListView.separated() constructor:

In essence,we construct two interweaved lists: one as the main list,one as the separator list.

**Note That: the infinite count discussed in the earlier constructor cannot be used here and this constructor enforces an itemCount.**

The code for this type goes as:

```dart
ListView.separated(
          itemBuilder: (context, position) => Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Text(
                    'main list $position',
                    style:
                        TextStyle(fontSize: 22.0, fontWeight: FontWeight.bold),
                  ),
                ),
              ),
          separatorBuilder: (context, position) {
            if (position % 2 == 0 && position <= 4) {
              return Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Text(
                    'Separator list $position',
                    style: TextStyle(fontSize: 22.0, color: Colors.redAccent),
                  ),
                ),
              );
            } else {
              return Container();
            }
          },
          itemCount: 10,
        ));
```

This type of list lets you dynamically define separators,have different types of separators for different types of items,add or remove separators when need,etc.

This implementation can also be used for inserting other types of elements (advertisements for example) easily and without any modification to the main list in the middle of the list items.

**Note: The separator list length is 1 less than the item list as a separator does not exist after the last element.**

#### ListView.custom()
The custom() constructor as its name suggests,lets you build ListViews with custom functionality for how the children of the list are build. The main parameter required for this is a SliverChildDelegate which builds the items.
The types of SliverChildDelegtes are:

1. SliverChildListDelegate
2. SliverChildBuilderDelegate

SliverChildListDelegate accepts a direct list of children whereas SliverChildBuilderDelegate accepts an IndexedWidgetBuilder(The builder function we use).

You can either use or subclass these for building your own delegates

> ListView.builder is essentially a ListView.custom with a SliverChildBuilderDelegate
> The ListView default constructor behaves like a ListView.custom with a SliverChildListDelegate

Now that we’re done with the types of ListViews,let’s take a look at ScrollPhysics.

### Exploring ScrollPhysics
To control the way scrolling takes place,we set the physics parameter in the ListView constructor. The different types of physics are:

#### NeverScrollablePhysics
NeverScrollablePhysics renders the list non-scrollable.Use this to disable scrolling of the ListView completely.

#### BouncingScrollPhysics
BouncingScrollPhysics bound back the list when the list ends. A similar effect is used on IOS.

#### ClampingScrollPhysics
This is the default scrolling physics used on Android. The list stops at the end and gives an effect indicating it.

#### FixedExtentScrollPhysics
This is slightly different than the other ones in this list in the sense that is only works with FixedExtendScrollControllers and lists that use them. For an example we will take a ListWheelScrollView which makes a wheel-like list.

FixedExtentScrollPhysics Only scrolls to items instead of any offset in between.

The code for this example is incredibly simple:

```dart
import 'package:flutter/material.dart';

class DeepDiveHome extends StatefulWidget {
  _DeepDiveHomeState createState() => _DeepDiveHomeState();
}

class _DeepDiveHomeState extends State<DeepDiveHome> {
  FixedExtentScrollController fixedExtentScrollController =
      new FixedExtentScrollController();

  final mothsOfTheYear = <String>[
    "January",
    "February",
    "March",
    "April",
    "May",
    "June",
    "July",
    "August",
    "September",
    "October",
    "November",
    "December"
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Deep Dive Flutter'),
      ),
      body: ListWheelScrollView(
        controller: fixedExtentScrollController,
        physics: FixedExtentScrollPhysics(),
        children: mothsOfTheYear.map((month) {
          return Card(
            child: Row(
              children: <Widget>[
                Expanded(
                  child: Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: Text(
                      month,
                      style: TextStyle(fontSize: 18.0),
                    ),
                  ),
                )
              ],
            ),
          );
        }).toList(),
        itemExtent: 60.0,
      ),
    );
  }
}
```

### A few more things to know
#### How to keep elements that get destroy alive in a list?
Flutter provides a KeepAlive() widget which keeps an item alive which would have otherwise be destroyed. In a list,elements are wrapped by default in a AutomaticKeepAlive widget.

AutomaticKeepAlives can be disabled by setting the addAutomaticKeepAlives field to false. This is useful in cases where the elements don’t need to be kept alive or for a custom implementation of KeepAlive.

#### Why does my ListView have space between the list and the outer Widget?
By default,a ListView has padding between it and the outer widget,to remove it,set padding to EdgeInsets.all(0.0).

