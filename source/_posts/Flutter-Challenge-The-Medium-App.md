---
title: Flutter Challenge The Medium App
date: 2022-01-29 00:00:39
summary: Flutter Challenges will attempt to recreate a particular app UI or design in Flutter. Today we’ll attempt to create the home screen of the Medium app. Note that the focus will be on the UI rather than actually fetching articles.
keywords: Flutter,Challenge,Medium,Medium App
categories: Flutter
tags:
  - Medium
---

![Medium](https://s2.loli.net/2022/01/29/MCo5YvxiU1Irez7.jpg)



Flutter Challenges will attempt to recreate a particular app UI or design in Flutter.

Today we’ll attempt to create the home screen of the Medium app. Note that the focus will be on the UI rather than actually fetching articles.

### Getting Started

Before developing a design,identify the components required to make the screen in any framework.

The app screen has a drawer,a notification and search option,and a list of articles.

Let’s take a look at the article

![medium_article](https://s2.loli.net/2022/01/29/RJxgzZ6sTXfHavr.jpg)

At the very top,the article has a text which gives you relevance as to why it’s there. For example,if it’s based on your reading history or if it’s from a category you follow.

Below that is the title of the article iteself,which is bold and black as to stand out.

Below the title is the author of the article,date of publishing,and how long the article is estimated to take to read.

On the right hand side is the article image and a bookmark button for saving it for later.

### Setting up the app

Create a new Flutter,example:”medium_app_ui”.

Remove the code for the counter app until you only remain with the a Scaffold with an AppBar.

Before we move on to the list,let’s contruct the AppBar according to the Medium design.

As of now,the appBar is blue. We need to change the app bar to black. The actions on the appBar are notifications and search.Doing these changes:

```dart
appBar: AppBar(
        title: Text('Home'),
        backgroundColor: Colors.black,
        actions: <Widget>[
          Icon(
            Icons.notifications_none,
            color: Colors.grey,
          ),
          Icon(
            Icons.search,
            color: Colors.grey,
          )
        ],
      ),
```

After this,we need to adjust a few icons and add a drawer.To add a drawer without and content,simply add.

```dart
drawer: Drawer(),
```

to your Scaffold.

Here is the code and resulting AppBar.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge Medium',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          'Home',
          style: TextStyle(
            fontWeight: FontWeight.w400,
          ),
        ),
        backgroundColor: Colors.black,
        actions: <Widget>[
          Padding(
            padding: const EdgeInsets.only(right: 20.0),
            child: Icon(
              Icons.notifications_none,
              color: Colors.grey,
            ),
          ),
          Padding(
            padding: const EdgeInsets.only(right: 12.0),
            child: Icon(
              Icons.search,
              color: Colors.grey,
            ),
          )
        ],
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[],
        ),
      ),
    );
  }
}
```

![medium_appbar_final](https://s2.loli.net/2022/01/29/hPK217XRMC6DkQb.png)

So far,so good.

Let’s create a class to store articles named NewsArticle.dart

```dart
class NewsArticale {
  String categoryTitle;
  String title;
  String author;
  String date;
  String readTime;
  String imageAssetName;
  NewsArticale(this.categoryTitle, this.title, this.author, this.date,
      this.readTime, this.imageAssetName);
}
```

For now,they’re just hardcoded articles.

### Creating the List

To create a repeating list in Flutter,we use ListView.builder().

In the image,the cards are over a grey background.

Let’s analyse the card components themselves.

![medium_analyse](https://s2.loli.net/2022/01/29/6zaMcvoVA4D7F2C.jpg)

There’s a class named NewsHelper which provides the hard-coded articles.

The final item is build like this:

```dart
Padding(
            padding: const EdgeInsets.fromLTRB(0.0, 0.5, 0.0, 0.5),
            child: Card(
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: <Widget>[
                    Text(
                      article.categoryTitle,
                      style: TextStyle(
                          color: Colors.black38,
                          fontWeight: FontWeight.w500,
                          fontSize: 16.0),
                    ),
                    Padding(
                      padding: const EdgeInsets.fromLTRB(0.0, 12.0, 0.0, 12.0),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: <Widget>[
                          Flexible(
                            child: Text(
                              article.title,
                              style: TextStyle(
                                fontWeight: FontWeight.bold,
                                fontSize: 22.0,
                              ),
                            ),
                            flex: 3,
                          ),
                          Flexible(
                            flex: 1,
                            child: Container(
                              height: 80.0,
                              width: 80.0,
                              child: Image.asset(
                                  'assets/' + article.imageAssetName,
                                  fit: BoxFit.cover),
                            ),
                          )
                        ],
                      ),
                    ),
                    Row(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: <Widget>[
                        Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: <Widget>[
                            Text(
                              article.author,
                              style: TextStyle(
                                fontSize: 18.0,
                              ),
                            ),
                            Text(
                              article.date + '.' + article.readTime,
                              style: TextStyle(
                                color: Colors.black45,
                                fontWeight: FontWeight.w500,
                              ),
                            ),
                          ],
                        ),
                        Icon(Icons.bookmark_border),
                      ],
                    )
                  ],
                ),
              ),
            ),
          )
```

### Designing the Drawer

This is how the drawer looks on the Medium app:

![medium_drawer](https://s2.loli.net/2022/01/29/OkC8M7JFP2EYgvX.jpg)

The Drawer is simply a Column with a list of elements and an Image at the top. The top white portion can be represented by one container and the bottom half by another.

Here is my drawer code:

```dart
Drawer(
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          Container(
            child: Padding(
              padding: const EdgeInsets.fromLTRB(32.0, 64.0, 32.0, 16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: <Widget>[
                  Icon(Icons.account_circle, size: 90.0),
                  Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: Text('John Doe',
                        style: TextStyle(
                          fontSize: 20.0,
                        )),
                  ),
                  Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: Text('See profile',
                        style: TextStyle(
                          color: Colors.black45,
                        )),
                  )
                ],
              ),
            ),
          ),
          Expanded(
            child: Container(
              color: Colors.black12,
              child: Padding(
                padding: const EdgeInsets.fromLTRB(40.0, 16.0, 40.0, 20.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: <Widget>[
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Home', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Audio', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child:
                          Text('Bookmarks', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child:
                          Text('Interests', style: TextStyle(fontSize: 18.0)),
                    ),
                    Divider(),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Become a member',
                          style: TextStyle(fontSize: 18.0, color: Colors.teal)),
                    ),
                    Divider(),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child:
                          Text('New Story', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Stats', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Drafts', style: TextStyle(fontSize: 18.0)),
                    ),
                    Expanded(
                      child: Container(
                        alignment: Alignment.bottomLeft,
                        child: Row(
                          mainAxisAlignment: MainAxisAlignment.spaceBetween,
                          children: <Widget>[
                            Icon(
                              Icons.account_box,
                              size: 64.0,
                            ),
                            Text(
                              'Setting',
                              style: TextStyle(fontSize: 18.0),
                            ),
                            Text('Help', style: TextStyle(fontSize: 18.0))
                          ],
                        ),
                      ),
                    )
                  ],
                ),
              ),
            ),
          )
        ],
      ),
    )
```

![medium_my_drawer](https://s2.loli.net/2022/01/29/dgj5S7vHtLyBQM1.png)

Here is the final code:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge Medium',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: buildAppBar(),
      body: buildBody(),
      drawer: buildDrawer(),
    );
  }

  Drawer buildDrawer() {
    return Drawer(
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          Container(
            child: Padding(
              padding: const EdgeInsets.fromLTRB(32.0, 64.0, 32.0, 16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: <Widget>[
                  Icon(Icons.account_circle, size: 90.0),
                  Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: Text('John Doe',
                        style: TextStyle(
                          fontSize: 20.0,
                        )),
                  ),
                  Padding(
                    padding: const EdgeInsets.all(8.0),
                    child: Text('See profile',
                        style: TextStyle(
                          color: Colors.black45,
                        )),
                  )
                ],
              ),
            ),
          ),
          Expanded(
            child: Container(
              color: Colors.black12,
              child: Padding(
                padding: const EdgeInsets.fromLTRB(40.0, 16.0, 40.0, 20.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: <Widget>[
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Home', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Audio', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child:
                          Text('Bookmarks', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child:
                          Text('Interests', style: TextStyle(fontSize: 18.0)),
                    ),
                    Divider(),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Become a member',
                          style: TextStyle(fontSize: 18.0, color: Colors.teal)),
                    ),
                    Divider(),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child:
                          Text('New Story', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Stats', style: TextStyle(fontSize: 18.0)),
                    ),
                    Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Text('Drafts', style: TextStyle(fontSize: 18.0)),
                    ),
                    Expanded(
                      child: Container(
                        alignment: Alignment.bottomLeft,
                        child: Row(
                          mainAxisAlignment: MainAxisAlignment.spaceBetween,
                          children: <Widget>[
                            Icon(
                              Icons.account_box,
                              size: 64.0,
                            ),
                            Text(
                              'Setting',
                              style: TextStyle(fontSize: 18.0),
                            ),
                            Text('Help', style: TextStyle(fontSize: 18.0))
                          ],
                        ),
                      ),
                    )
                  ],
                ),
              ),
            ),
          )
        ],
      ),
    );
  }

  Center buildBody() {
    return Center(
        child: ListView.builder(
      itemCount: NewsHelper.articleCount,
      itemBuilder: (contex, position) {
        NewsArticle article = NewsHelper.getArticle(position);
        return Padding(
          padding: const EdgeInsets.fromLTRB(0.0, 0.5, 0.0, 0.5),
          child: Card(
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: <Widget>[
                  Text(
                    article.categoryTitle,
                    style: TextStyle(
                        color: Colors.black38,
                        fontWeight: FontWeight.w500,
                        fontSize: 16.0),
                  ),
                  Padding(
                    padding: const EdgeInsets.fromLTRB(0.0, 12.0, 0.0, 12.0),
                    child: Row(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: <Widget>[
                        Flexible(
                          child: Text(
                            article.title,
                            style: TextStyle(
                              fontWeight: FontWeight.bold,
                              fontSize: 22.0,
                            ),
                          ),
                          flex: 3,
                        ),
                        Flexible(
                          flex: 1,
                          child: Container(
                            height: 80.0,
                            width: 80.0,
                            child: Image.asset(
                                'assets/' + article.imageAssetName,
                                fit: BoxFit.cover),
                          ),
                        )
                      ],
                    ),
                  ),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: <Widget>[
                      Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: <Widget>[
                          Text(
                            article.author,
                            style: TextStyle(
                              fontSize: 18.0,
                            ),
                          ),
                          Text(
                            article.date + '.' + article.readTime,
                            style: TextStyle(
                              color: Colors.black45,
                              fontWeight: FontWeight.w500,
                            ),
                          ),
                        ],
                      ),
                      Icon(Icons.bookmark_border),
                    ],
                  )
                ],
              ),
            ),
          ),
        );
      },
    ));
  }

  AppBar buildAppBar() {
    return AppBar(
      title: Text(
        'Home',
        style: TextStyle(
          fontWeight: FontWeight.w400,
        ),
      ),
      backgroundColor: Colors.black,
      actions: <Widget>[
        Padding(
          padding: const EdgeInsets.only(right: 20.0),
          child: Icon(
            Icons.notifications_none,
            color: Colors.grey,
          ),
        ),
        Padding(
          padding: const EdgeInsets.only(right: 12.0),
          child: Icon(
            Icons.search,
            color: Colors.grey,
          ),
        )
      ],
    );
  }
}

class NewsArticle {
  String categoryTitle;
  String title;
  String author;
  String date;
  String readTime;
  String imageAssetName;
  NewsArticle(this.categoryTitle, this.title, this.author, this.date,
      this.readTime, this.imageAssetName);
}

class NewsHelper {
  static var articleCount = 4;

  static var categoryTitles = [
    "SPACE",
    "FROM YOUR NETWORK",
    "BASED ON YOUR READING HISTORY",
    "DATA SCIENCE"
  ];
  static var titles = [
    "Sorry, Methane and 'Organics' On Mars Are Not Evidence For Life",
    "A crash course on Serverless APIs with Express and MongoDB",
    "What happened Gmail?",
    "A year as a Data Scientist right after college: An honest review"
  ];
  static var authorNames = [
    "Ethan Siegal",
    "Adnan Rahic",
    "Avi Ashkenazi",
    "Abhishek Parkbhakar"
  ];
  static var date = ["15 Jun", "15 hours ago", "27 Apr", "14 Jun"];
  static var readTimes = [
    "7 min read",
    "14 min read",
    "8 min read",
    "8 min read"
  ];
  static var imageAssetName = [
    "mars.jpeg",
    "cars.jpeg",
    "gmail.jpeg",
    "umbrella.jpeg"
  ];

  static getArticle(int position) {
    return NewsArticle(
        categoryTitles[position],
        titles[position],
        authorNames[position],
        date[position],
        readTimes[position],
        imageAssetName[position]);
  }
}
```
