title: Flutter Challenge the Twitter App
summary: >-
  Flutter Challenges will attempt to recreate a particular app UI or design in
  Flutter.This challenge will attempt the home screen of the Twitter Android
  app. Note that the focus will be on the UI rather than actually fetching data
  from a backend server.
keywords: 'Flutter,Challenge,Twitter,Twitter App'
categories:
  - Flutter
tags:
  - Twitter
date: 2022-01-29 00:21:00
---

Flutter Challenges will attempt to recreate a particular app UI or design in Flutter.

This challenge will attempt the home screen of the Twitter Android app. Note that the focus will be on the UI rather than actually fetching data from a backend server.

### Understanding the app structure

![app structure](https://s2.loli.net/2022/01/29/xJ36F7ZfvRMwAyN.jpg)

The Twitter app has 4 main pages controlled by a Bottom Navigation Bar.

They are:

1. Home(Displays tweets in your feed)
2. Search(Search people,organisations,etc)
3. Notifications(Notifications and mentions)
4. Messages(Private messages)

The BottomNavigationBar has for tabs to go to each of these pages.

In our app,we’ll have four different pages and we’ll just change the pages when an item on the BottomNavigationBar is tapped.

### Setting up the project

After creating a Flutter project(I’ve named it twitter_ui),clear out the default code in the project until you’re left with this:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge Twitter',
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
      body: Center(),
    );
  }
}
```

The HomePage will have a Scaffold that will hold our main BottomNavigationBar and whatever page is currently active.

### Getting Started

Because the Bottom Navigation Bar is main widget used to navigate,let’s try that first.

Here’s how the BottomNavigationBar looks like:

![twitter_bottom_navigation](https://s2.loli.net/2022/01/29/2DhUmRibuzsJdQP.jpg)Because we don’t have the exact icons used in the app,we’ll go with the [Font Flutter Awesome Package.](https://pub.dartlang.org/packages/font_awesome_flutter)Add the dependency in the pubspec.yaml and add

```dart
import 'package:font_awesome_flutter/font_awesome_flutter.dart';
```

to the main.dart.

The BottomNavigationBar code comes down to:

```dart
bottomNavigationBar: BottomNavigationBar(
        items: [
          BottomNavigationBarItem(
            title: Text(''),
            icon: Icon(FontAwesomeIcons.home,
                color: selectedPageIndex == 0 ? Colors.blue : Colors.blueGrey),
          ),
          BottomNavigationBarItem(
            title: Text(''),
            icon: Icon(FontAwesomeIcons.search,
                color: selectedPageIndex == 1 ? Colors.blue : Colors.blueGrey),
          ),
          BottomNavigationBarItem(
            title: Text(''),
            icon: Icon(FontAwesomeIcons.bell,
                color: selectedPageIndex == 2 ? Colors.blue : Colors.blueGrey),
          ),
          BottomNavigationBarItem(
            title: Text(''),
            icon: Icon(FontAwesomeIcons.envelope,
                color: selectedPageIndex == 3 ? Colors.blue : Colors.blueGrey),
          ),
        ],
        onTap: (index) {
          setState(() {
            selectedPageIndex = index;
          });
        },
        currentIndex: selectedPageIndex,
      ),
```

Add this to the HomePage.

Notice when we set the color of the icons,we check if the icon is selected and then assign colors.In the twitter app,selected icon is blue and let’s set unselected ones to blueGrey.

We define a integer called selectedPageIndex which stores the index of the page selected. In the onTop function,we set the variable to the new index. It is wrapped in a setState() call as we need a refresh of the page to re-render the AppBar.

Here is our Bottom Navigation Bar:

![twitter_appbar](https://s2.loli.net/2022/01/29/eXKnm96vGH3c8fS.png)

### Setting Up The Pages

Let’s set up the four basic pages which will be displayed when the respective icons are clicked.

We set up four pages (in different files) like this:

For the user feed(home) page:

```dart
import 'package:flutter/material.dart';

class UserFeedPage extends StatefulWidget {
  _UserFeedPageState createState() => _UserFeedPageState();
}

class _UserFeedPageState extends State<UserFeedPage> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

Similarly we set up the Search,Notification and Messages Page.

Again in the base page,we import all these pages and set up a list of these pages.

```dart
var pages = [
    UserFeedPage(),
    SearchPage(),
    NotificationPage(),
    MessagesPage(),
  ];
```

And in the Scaffold,we set

```dart
body:pages[selectPageIndex],
```

which sets the body to display the page.

So till now,this is the MyHomePage base widget:

```dart
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';
import 'package:twitter_ui/messages_page.dart';
import 'package:twitter_ui/notification_page.dart';
import 'package:twitter_ui/search_page.dart';
import 'package:twitter_ui/user_feed_page.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge Twitter',
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
  var selectedPageIndex = 0;
  var pages = [
    UserFeedPage(),
    SearchPage(),
    NotificationPage(),
    MessagesPage(),
  ];
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(''),
      ),
      body: pages[selectedPageIndex],
      bottomNavigationBar: BottomNavigationBar(
        items: [
          BottomNavigationBarItem(
              title: Text(''),
              icon: Icon(
                FontAwesomeIcons.home,
                color: selectedPageIndex == 0 ? Colors.blue : Colors.blueGrey,
              )),
          BottomNavigationBarItem(
              title: Text(''),
              icon: Icon(
                FontAwesomeIcons.search,
                color: selectedPageIndex == 1 ? Colors.blue : Colors.blueGrey,
              )),
          BottomNavigationBarItem(
              title: Text(''),
              icon: Icon(
                FontAwesomeIcons.bell,
                color: selectedPageIndex == 2 ? Colors.blue : Colors.blueGrey,
              )),
          BottomNavigationBarItem(
              title: Text(''),
              icon: Icon(
                FontAwesomeIcons.envelope,
                color: selectedPageIndex == 3 ? Colors.blue : Colors.blueGrey,
              )),
        ],
        currentIndex: selectedPageIndex,
        onTap: (index) {
          setState(() {
            selectedPageIndex = index;
          });
        },
      ),
    );
  }
}
```

Now,we’ll recreate the pages themselves.

### Creating the User Feed Page

![User Feed Page](https://s2.loli.net/2022/01/29/cBPEJlmnF5RDqp3.jpg)

There are two elements in the page:The AppBar and the list of tweets.

First let’s make the AppBar,It has a user profile picture and a black title with a white background.

```dart
appBar: AppBar(
        backgroundColor: Colors.white,
        title: Text(
          'Home',
          style: TextStyle(color: Colors.black),
        ),
        leading: Icon(Icons.account_circle, color: Colors.grey, size: 35.0),
      ),
```

We’ll use an icon instead of a profile picture.

Now,we need to create the list of tweets.For this we use a ListView.builder().

Let’s take a look at the list item.

![check_list_view_item](https://s2.loli.net/2022/01/29/paZkK4fCVImis9v.jpg)

First,we’ll have a column with a row and a divider you see at the bottom.

Inside the row,we’ll have the user icon and another column.

The column will hold a row for tweet information,a text with the tweet itself,an image and another row for actions to take on tweet(like,comment,etc.).

For brevity,we’ll exclude the image for now,but adding it is as simple as adding an image in a row.

```dart
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';

class UserFeedPage extends StatefulWidget {
  _UserFeedPageState createState() => _UserFeedPageState();
}

class _UserFeedPageState extends State<UserFeedPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.white,
        title: Text(
          'Home',
          style: TextStyle(color: Colors.black),
        ),
        leading: Icon(Icons.account_circle, color: Colors.grey, size: 35.0),
      ),
      body: ListView.builder(
        itemCount: 4,
        itemBuilder: (context, position) {
          TweetItemModel tweet = TweetHelper.getTweet(position);
          return Column(
            children: <Widget>[
              Padding(
                padding: const EdgeInsets.all(4.0),
                child: Row(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: <Widget>[
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Icon(
                        Icons.account_circle,
                        size: 60.0,
                        color: Colors.grey,
                      ),
                    ),
                    Expanded(
                      child: Column(
                        mainAxisAlignment: MainAxisAlignment.start,
                        children: <Widget>[
                          Padding(
                            padding: const EdgeInsets.only(top: 4.0),
                            child: Row(
                              mainAxisAlignment: MainAxisAlignment.spaceBetween,
                              children: <Widget>[
                                Expanded(
                                  child: Container(
                                    child: RichText(
                                      text: TextSpan(children: [
                                        TextSpan(
                                            text: tweet.username,
                                            style: TextStyle(
                                                fontWeight: FontWeight.w600,
                                                fontSize: 18.0,
                                                color: Colors.black)),
                                        TextSpan(
                                            text: ' ' + tweet.twitterHandle,
                                            style: TextStyle(
                                                fontSize: 16.0,
                                                color: Colors.grey)),
                                        TextSpan(
                                            text: '${tweet.time}',
                                            style: TextStyle(
                                                fontSize: 16.0,
                                                color: Colors.grey)),
                                      ]),
                                      overflow: TextOverflow.ellipsis,
                                    ),
                                  ),
                                  flex: 5,
                                ),
                                Expanded(
                                  flex: 1,
                                  child: Padding(
                                    padding: const EdgeInsets.only(right: 4.0),
                                    child: Icon(Icons.expand_more,
                                        color: Colors.grey),
                                  ),
                                )
                              ],
                            ),
                          ),
                          Padding(
                            padding: const EdgeInsets.symmetric(vertical: 4.0),
                            child: Text(tweet.tweet,
                                style: TextStyle(fontSize: 18.0)),
                          ),
                          Padding(
                            padding: const EdgeInsets.all(8.0),
                            child: Row(
                              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                              children: <Widget>[
                                Icon(FontAwesomeIcons.comment,
                                    color: Colors.grey),
                                Icon(FontAwesomeIcons.retweet,
                                    color: Colors.grey),
                                Icon(FontAwesomeIcons.heart,
                                    color: Colors.grey),
                                Icon(FontAwesomeIcons.shareAlt,
                                    color: Colors.grey),
                              ],
                            ),
                          )
                        ],
                      ),
                    )
                  ],
                ),
              ),
              Divider(),
            ],
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: Icon(FontAwesomeIcons.featherAlt),
      ),
    );
  }
}

class TweetItemModel {
  String tweet;
  String username;
  String time;
  String twitterHandle;

  TweetItemModel(this.tweet, this.username, this.time, this.twitterHandle);
}

class TweetHelper {
  static var tweets = [
    TweetItemModel(
        "Inviting computer science students to register for the latest event in computer technology.",
        "Google Devs",
        "3m",
        "@GoogleDevsIndia"),
    TweetItemModel(
        "Developing a large, complex app that needs a microservice architecture? @crichardson. Read on to learn how to  decompose an application into services ",
        "Java",
        "5m",
        "@java"),
    TweetItemModel(
        "The Samsung Galaxy S9 is in the record books now, but it's not likely that Samsung will be celebrating this particular milestone. https://www.androidauthority.com/samsung-galaxy-s9-history-887809/ … #samsung #samsunggalaxys9 by: C. Scott Brown",
        "Android Authority",
        "30m",
        "@AndroidAuth"),
    TweetItemModel(
        "Inviting computer science students to register for the latest event in computer technology.",
        "Google Devs India",
        "3m",
        "@GoogleDevsIndia"),
  ];

  static TweetItemModel getTweet(int position) {
    return tweets[position];
  }
}
```

![list](https://s2.loli.net/2022/01/29/smJuvCbcL4ESNPM.png)

This is the recreated page for the Twitter user feed. The fact that recreating andy UI is fast and easy in Flutter is a testament to its development speed and customisability at the same time.These are two things that rarely go together.

The main.dart code is follow:

```dart
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';
import 'package:twitter_ui/messages_page.dart';
import 'package:twitter_ui/notification_page.dart';
import 'package:twitter_ui/search_page.dart';
import 'package:twitter_ui/user_feed_page.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge Twitter',
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

class _MyHomePageState extends State<MyHomePage> {
  var selectedPageIndex = 0;
  var pages = [
    UserFeedPage(),
    SearchPage(),
    NotificationPage(),
    MessagesPage(),
  ];
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: pages[selectedPageIndex],
      bottomNavigationBar: BottomNavigationBar(
        items: [
          BottomNavigationBarItem(
              title: Text(''),
              icon: Icon(
                FontAwesomeIcons.home,
                color: selectedPageIndex == 0 ? Colors.blue : Colors.blueGrey,
              )),
          BottomNavigationBarItem(
              title: Text(''),
              icon: Icon(
                FontAwesomeIcons.search,
                color: selectedPageIndex == 1 ? Colors.blue : Colors.blueGrey,
              )),
          BottomNavigationBarItem(
              title: Text(''),
              icon: Icon(
                FontAwesomeIcons.bell,
                color: selectedPageIndex == 2 ? Colors.blue : Colors.blueGrey,
              )),
          BottomNavigationBarItem(
              title: Text(''),
              icon: Icon(
                FontAwesomeIcons.envelope,
                color: selectedPageIndex == 3 ? Colors.blue : Colors.blueGrey,
              )),
        ],
        currentIndex: selectedPageIndex,
        onTap: (index) {
          setState(() {
            selectedPageIndex = index;
          });
        },
      ),
    );
  }
}
```
