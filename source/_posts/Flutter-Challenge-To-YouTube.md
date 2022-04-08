---
title: Flutter Challenge The YouTube App
date: 2022-01-28 23:41:23
top: true
cover: true
summary: Flutter Challenges will attempt to receate a particular app UI or design in Flutter. This challenge will attempt the Home screen and the Video Detail screen (The screen where the video actually plays) of the Youtube app including the animation.
keywords: Flutter,Challenge,YouTube,YouTube App
categories: Flutter
tags:
  - YouTube
---

![youtobe](https://s2.loli.net/2022/01/29/D2a79b6t4BoxEZu.jpg)

Flutter Challenges will attempt to receate a particular app UI or design in Flutter.

This challenge will attempt the Home screen and the Video Detail screen (The screen where the video actually plays) of the Youtube app including the animation.

This challenge will be slightly more complex than my previous ones but the result is that much better for the same.

### Getting Started

The YouTube app comprises of:

a) The Home Screen which contains:

1. The AppBar with 3 actions
2. The User Video Feed
3. The Bottom Navigation Bar

b) The Video Detail Screen which contains:

1. The main video player which can shrink to let users look at their feed (PIP)
2. Recommendations for users based on the current video

### Setting up the project

Let’s make a Flutter project and remove all the default code leaving just a blank screen with the default app bar.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge YouTube',
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
          title: Text(''),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[],
          ),
        ));
  }
}
```

### Making the AppBar

The AppBar has the YouTube logo and name on the left and three actions,i.e. record,search and open profile on the right.

The AppBar can be recreated as:

```dart
appBar: AppBar(
          backgroundColor: Colors.white,
          title: Row(
            mainAxisSize: MainAxisSize.min,
            children: <Widget>[
              Icon(
                FontAwesomeIcons.youtube,
                color: Colors.red,
              ),
              Padding(
                padding: const EdgeInsets.only(left: 8.0),
                child: Text(
                  'YouTube',
                  style: TextStyle(
                    color: Colors.black,
                    letterSpacing: -1.0,
                    fontWeight: FontWeight.w700,
                  ),
                ),
              )
            ],
          ),
          actions: <Widget>[
            Padding(
              padding: const EdgeInsets.symmetric(horizontal: 12.0),
              child: Icon(
                Icons.videocam,
                color: Colors.black54,
              ),
            ),
            Padding(
              padding: const EdgeInsets.symmetric(horizontal: 12.0),
              child: Icon(Icons.search, color: Colors.black54),
            ),
            Padding(
              padding: const EdgeInsets.symmetric(horizontal: 12.0),
              child: Icon(Icons.account_circle, color: Colors.black54),
            )
          ],
        ),
```

This is what the recreated AppBar looks like:

![youtube_appbar](https://s2.loli.net/2022/01/28/KZUcRx8uLgMVr4l.png)

**Note:** For the YouTube logo,I’ve used the FontFlutterAwesome icons [available as a package on Dart pub.](https://pub.dartlang.org/packages/font_awesome_flutter#-readme-tab-)

Let’s move on to the Bottom Navigation Bar,

### Creating the BottomNavigationBar

The bottom navigation bar has 5 items in it and is pretty straightforward to recreate in Flutter.We use the bottomNavigationBar parameter of the Scaffold.

```dart
bottomNavigationBar: BottomNavigationBar(
        items: [
          BottomNavigationBarItem(
            icon: Icon(
              Icons.home,
              color: Colors.black54,
            ),
            title: Text("Home",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              FontAwesomeIcons.fire,
              color: Colors.black54,
            ),
            title: Text("Trending",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.subscriptions,
              color: Colors.black54,
            ),
            title: Text("Subscriptions",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.email,
              color: Colors.black54,
            ),
            title: Text("Inbox",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.folder,
              color: Colors.black54,
            ),
            title: Text("Library",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
        ],
        type: BottomNavigationBarType.fixed,
      ),
```

**Note:** We need to specify a fixed BottomNavigationBarType because for 4+ items,the default type is shifting to avoid overcrowding.

The result is:

![youtube_navigation_bar](https://s2.loli.net/2022/01/28/tbrTi7SdzasBLCm.png)

### The User Video Feed

The User Video Feed is a list of items consisting of recommended videos, Let’s take a look at the list item:

![youtube_user_video_feed](https://s2.loli.net/2022/01/28/ouIpVg8vZNfiG1L.png)

The list item consists of a Column with an image and a Row of information about the video.The Row consists of an image,a Column of the title and pubisher and a menu button.

To create a list in Flutter,we use a ListView.builder().Recreating the list items, we come to:

```dart
import 'package:flutter/material.dart';
import 'package:flutterchallenge/VideoItemModel.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge YouTube',
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
  var videos = [
    VideoItemModel(
        "Gordon Ramsay Cooked For Vladimir Putin",
        "The Late Show with Stephen Colbert\n1.1M views.2 weeks ago",
        "assets/images/youtube/youtube_one.jpg"),
    VideoItemModel(
        "Hailee Steinfeld, Alesso - Let Me Go",
        "Hailee Steinfeld\n57M views.8 months ago",
        "assets/images/youtube/youtube_two.jpg"),
    VideoItemModel(
        "Charlie Puth - Look At Me Now",
        "Lyricwood\n4.7M views.4 months ago",
        "assets/images/youtube/youtube_three.jpg")
  ];
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.white,
        title: Row(
          mainAxisSize: MainAxisSize.min,
          children: <Widget>[
            Icon(
              FontAwesomeIcons.youtube,
              color: Colors.red,
            ),
            Padding(
              padding: const EdgeInsets.only(left: 8.0),
              child: Text(
                'YouTube',
                style: TextStyle(
                  color: Colors.black,
                  letterSpacing: -1.0,
                  fontWeight: FontWeight.w700,
                ),
              ),
            )
          ],
        ),
        actions: <Widget>[
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 12.0),
            child: Icon(
              Icons.videocam,
              color: Colors.black54,
            ),
          ),
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 12.0),
            child: Icon(Icons.search, color: Colors.black54),
          ),
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 12.0),
            child: Icon(Icons.account_circle, color: Colors.black54),
          )
        ],
      ),
      body: ListView.builder(
        itemCount: 3,
        itemBuilder: (context, position) {
          return Column(
            children: <Widget>[
              Row(
                children: <Widget>[
                  Expanded(
                    child: Image.asset(
                      videos[position].imagePath,
                      fit: BoxFit.cover,
                    ),
                  )
                ],
              ),
              Padding(
                padding: const EdgeInsets.all(12.0),
                child: Row(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: <Widget>[
                    Expanded(
                      child: Icon(
                        Icons.account_circle,
                        size: 40.0,
                      ),
                      flex: 2,
                    ),
                    Expanded(
                      child: Column(
                        children: <Widget>[
                          Padding(
                            padding: const EdgeInsets.only(bottom: 4.0),
                            child: Text(
                              videos[position].title,
                              style: TextStyle(fontSize: 18.0),
                            ),
                          ),
                          Text(
                            videos[position].publisher,
                            style: TextStyle(color: Colors.black54),
                          ),
                        ],
                        crossAxisAlignment: CrossAxisAlignment.start,
                      ),
                      flex: 9,
                    ),
                    Expanded(
                      flex: 1,
                      child: Icon(Icons.more_vert),
                    ),
                  ],
                ),
              )
            ],
          );
        },
      ),
      bottomNavigationBar: BottomNavigationBar(
        items: [
          BottomNavigationBarItem(
            icon: Icon(
              Icons.home,
              color: Colors.black54,
            ),
            title: Text("Home",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              FontAwesomeIcons.fire,
              color: Colors.black54,
            ),
            title: Text("Trending",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.subscriptions,
              color: Colors.black54,
            ),
            title: Text("Subscriptions",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.email,
              color: Colors.black54,
            ),
            title: Text("Inbox",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.folder,
              color: Colors.black54,
            ),
            title: Text("Library",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
        ],
        type: BottomNavigationBarType.fixed,
      ),
    );
  }
}
```

Here videos is simply a list of the video details like titles and publishers.

This is how the recreated home screen looks like:

![home_screen](https://s2.loli.net/2022/01/28/uzvyTHjwr8gZRSE.png)

Now,we’ll move on to the slightly harder part,the video detail screen.

### Creating the Video Detail Screen

The video detail screen is the screen which actually displays videos in the YouTube app. The hightlight of the screen is that we can shrink the video and it keeps on playing on the bottom right side of the screen.For this article,we’ll focus on the shrinking animation instead of actually playing a video.

Notice,the screen is not a different page,but more of an overlay onto the existing screen.So,we’ll use the Stack widget to overlay the screen instead.

So at the background,we’ll have our home screen and on the top will be our video page.

### Building the Floating Video Player (Picture-in-picture)

To build the floating video player which can expand to fill the whole screen,we use a LayoutBuilder to fit the screen perfectly.

Before going ahead we define a few values,namely,what size the video player will be when shrunk and expanded.We don’t set the width for the expanded player,instead,we take it from the Layout Builder.

```dart
var currentAlignment = Alignment.topCenter;
  var minVideoHeight = 100.0;
  var minVideoWidth = 150.0;
  var maxVideoHeight = 200.0;

  var maxVideoWidth = 250.0;
  var currentVideoHeight = 200.0;
  var currentVideoWidth = 200.0;

  bool isInSmallMode = false;
```

Here,”small mode” is when the video player is shrunk.

The LayoutBuilder which builds the VideoDetail Screen can be written as:

```dart
LayoutBuilder(builder: (context, constraints) {
      maxVideoWidth = constraints.biggest.width;
      if (!isInSmallMode) {
        currentVideoWidth = maxVideoWidth;
      }
      return Column(
        crossAxisAlignment: CrossAxisAlignment.end,
        children: <Widget>[
          Expanded(
            child: Align(
              child: Padding(
                padding: EdgeInsets.all(isInSmallMode ? 8.0 : 0.0),
                child: GestureDetector(
                  child: Container(
                    width: currentVideoWidth,
                    height: currentVideoHeight,
                    child: Image.asset(
                      videos[videoIndexSelected].imagePath,
                      fit: BoxFit.cover,
                    ),
                    color: Colors.blue,
                  ),
                  onVerticalDragEnd: (details) {
                    if (details.velocity.pixelsPerSecond.dy > 0) {
                      setState(() {
                        isInSmallMode = true;
                      });
                    } else if (details.velocity.pixelsPerSecond.dy < 0) {
                      setState(() {});
                    }
                  },
                ),
              ),
              alignment: currentAlignment,
            ),
            flex: 3,
          ),
          currentAlignment == Alignment.topCenter
              ? Expanded(
                  flex: 6,
                  child: Container(
                    child: Column(
                      children: <Widget>[
                        Row(),
                        Padding(
                          padding: const EdgeInsets.all(8.0),
                          child: Card(
                            child: Padding(
                              padding: const EdgeInsets.all(8.0),
                              child: Text('Video Recommendation'),
                            ),
                          ),
                        ),
                        Padding(
                          padding: const EdgeInsets.all(8.0),
                          child: Card(
                            child: Padding(
                              padding: const EdgeInsets.all(8.0),
                              child: Text('Video Recommendation'),
                            ),
                          ),
                        ),
                        Padding(
                          padding: const EdgeInsets.all(8.0),
                          child: Card(
                            child: Padding(
                              padding: const EdgeInsets.all(8.0),
                              child: Text('Video Recommendation'),
                            ),
                          ),
                        ),
                      ],
                    ),
                    color: Colors.white,
                  ),
                )
              : Container(),
          Row(),
        ],
      );
    });
```

Notice how we’re getting the maximum screen width and then using that instead of using our first arbitrary value for maximum screen width.

We have attached a GestureDetector to detect swips on the screen so that we can shrink and expand it accordingly.Let’s create the animations.

### Animating the Video Detail Screen

When we animate,we need to take care of two things:

1. Bringing the video from top to the bottom right.
2. Changing the size of the video and making it smaller.

For these things,we use two Tweens,an AlignmentTween and a Tween and construct two separate animations which will run concurrently.

```
AnimationController alignmentAnimationController;
  Animation alignmentAnimation;

  AnimationController videoViewController;
  Animation videoViewAnimation;

  @override
  void initState() {
    super.initState();
    alignmentAnimationController =
        AnimationController(vsync: this, duration: Duration(seconds: 1))
          ..addListener(() {
            setState(() {
              currentAlignment = alignmentAnimation.value;
            });
          });
    alignmentAnimation =
        AlignmentTween(begin: Alignment.topCenter, end: Alignment.bottomRight)
            .animate(CurvedAnimation(
                parent: alignmentAnimationController,
                curve: Curves.fastOutSlowIn));
    videoViewController =
        AnimationController(vsync: this, duration: Duration(seconds: 1))
          ..addListener(() {
            setState(() {
              currentVideoWidth = (maxVideoWidth * videoViewAnimation.value) +
                  (minVideoHeight * (1.0 - videoViewAnimation.value));
              currentVideoHeight = (maxVideoHeight * videoViewAnimation.value) +
                  (minVideoHeight * (1.0 - videoViewAnimation.value));
            });
          });

    videoViewAnimation =
        Tween<double>(begin: 1.0, end: 0.0).animate(videoViewController);
  }
```

We trigger these animations when our video player is swiped up or down.

```dart
onVerticalDragEnd: (details) {
                    if (details.velocity.pixelsPerSecond.dy > 0) {
                      setState(() {
                        isInSmallMode = true;
                        alignmentAnimationController.forward();
                        videoViewController.forward();
                      });
                    } else if (details.velocity.pixelsPerSecond.dy < 0) {
                      setState(() {
                        alignmentAnimationController.reverse();
                        videoViewController.reverse().then((value) {
                          setState(() {
                            isInSmallMode = false;
                          });
                        });
                      });
                    }
                  },
```

This is the final result of the code:

![youtube_final](https://s2.loli.net/2022/01/28/mMlwiQB8Z2GXRPx.png)

This is the final code:

```dart
import 'package:flutter/material.dart';
import 'package:flutterchallenge/VideoItemModel.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge YouTube',
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

class _MyHomePageState extends State<MyHomePage> with TickerProviderStateMixin {
  var videos = [
    VideoItemModel(
        "Gordon Ramsay Cooked For Vladimir Putin",
        "The Late Show with Stephen Colbert\n1.1M views.2 weeks ago",
        "assets/images/youtube/youtube_one.jpg"),
    VideoItemModel(
        "Hailee Steinfeld, Alesso - Let Me Go",
        "Hailee Steinfeld\n57M views.8 months ago",
        "assets/images/youtube/youtube_two.jpg"),
    VideoItemModel(
        "Charlie Puth - Look At Me Now",
        "Lyricwood\n4.7M views.4 months ago",
        "assets/images/youtube/youtube_three.jpg")
  ];

  var currentAlignment = Alignment.topCenter;
  var minVideoHeight = 100.0;
  var minVideoWidth = 150.0;
  var maxVideoHeight = 200.0;

  var maxVideoWidth = 250.0;
  var currentVideoHeight = 200.0;
  var currentVideoWidth = 200.0;

  bool isInSmallMode = false;
  var videoIndexSelected = -1;

  AnimationController alignmentAnimationController;
  Animation alignmentAnimation;

  AnimationController videoViewController;
  Animation videoViewAnimation;

  @override
  void initState() {
    super.initState();
    alignmentAnimationController =
        AnimationController(vsync: this, duration: Duration(seconds: 1))
          ..addListener(() {
            setState(() {
              currentAlignment = alignmentAnimation.value;
            });
          });
    alignmentAnimation =
        AlignmentTween(begin: Alignment.topCenter, end: Alignment.bottomRight)
            .animate(CurvedAnimation(
                parent: alignmentAnimationController,
                curve: Curves.fastOutSlowIn));
    videoViewController =
        AnimationController(vsync: this, duration: Duration(seconds: 1))
          ..addListener(() {
            setState(() {
              currentVideoWidth = (maxVideoWidth * videoViewAnimation.value) +
                  (minVideoHeight * (1.0 - videoViewAnimation.value));
              currentVideoHeight = (maxVideoHeight * videoViewAnimation.value) +
                  (minVideoHeight * (1.0 - videoViewAnimation.value));
            });
          });

    videoViewAnimation =
        Tween<double>(begin: 1.0, end: 0.0).animate(videoViewController);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.white,
        title: Row(
          mainAxisSize: MainAxisSize.min,
          children: <Widget>[
            Icon(
              FontAwesomeIcons.youtube,
              color: Colors.red,
            ),
            Padding(
              padding: const EdgeInsets.only(left: 8.0),
              child: Text(
                'YouTube',
                style: TextStyle(
                  color: Colors.black,
                  letterSpacing: -1.0,
                  fontWeight: FontWeight.w700,
                ),
              ),
            )
          ],
        ),
        actions: <Widget>[
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 12.0),
            child: Icon(
              Icons.videocam,
              color: Colors.black54,
            ),
          ),
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 12.0),
            child: Icon(Icons.search, color: Colors.black54),
          ),
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 12.0),
            child: Icon(Icons.account_circle, color: Colors.black54),
          )
        ],
      ),
      body: Stack(children: <Widget>[
        Center(
          child: ListView.builder(
            itemCount: 3,
            itemBuilder: (context, position) {
              return GestureDetector(
                onTap: () {
                  setState(() {
                    videoIndexSelected = position;
                  });
                },
                child: Column(
                  children: <Widget>[
                    Row(
                      children: <Widget>[
                        Expanded(
                          child: Image.asset(
                            videos[position].imagePath,
                            fit: BoxFit.cover,
                          ),
                        )
                      ],
                    ),
                    Padding(
                      padding: const EdgeInsets.all(12.0),
                      child: Row(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: <Widget>[
                          Expanded(
                            child: Icon(
                              Icons.account_circle,
                              size: 40.0,
                            ),
                            flex: 2,
                          ),
                          Expanded(
                            child: Column(
                              children: <Widget>[
                                Padding(
                                  padding: const EdgeInsets.only(bottom: 4.0),
                                  child: Text(
                                    videos[position].title,
                                    style: TextStyle(fontSize: 18.0),
                                  ),
                                ),
                                Text(
                                  videos[position].publisher,
                                  style: TextStyle(color: Colors.black54),
                                ),
                              ],
                              crossAxisAlignment: CrossAxisAlignment.start,
                            ),
                            flex: 9,
                          ),
                          Expanded(
                            flex: 1,
                            child: Icon(Icons.more_vert),
                          ),
                        ],
                      ),
                    )
                  ],
                ),
              );
            },
          ),
        ),
        videoIndexSelected > -1
            ? LayoutBuilder(builder: (context, constraints) {
                maxVideoWidth = constraints.biggest.width;
                if (!isInSmallMode) {
                  currentVideoWidth = maxVideoWidth;
                }
                return Column(
                  crossAxisAlignment: CrossAxisAlignment.end,
                  children: <Widget>[
                    Expanded(
                      child: Align(
                        child: Padding(
                          padding: EdgeInsets.all(isInSmallMode ? 8.0 : 0.0),
                          child: GestureDetector(
                            child: Container(
                              width: currentVideoWidth,
                              height: currentVideoHeight,
                              child: Image.asset(
                                videos[videoIndexSelected].imagePath,
                                fit: BoxFit.cover,
                              ),
                              color: Colors.blue,
                            ),
                            onVerticalDragEnd: (details) {
                              if (details.velocity.pixelsPerSecond.dy > 0) {
                                setState(() {
                                  isInSmallMode = true;
                                  alignmentAnimationController.forward();
                                  videoViewController.forward();
                                });
                              } else if (details.velocity.pixelsPerSecond.dy <
                                  0) {
                                setState(() {
                                  alignmentAnimationController.reverse();
                                  videoViewController.reverse().then((value) {
                                    setState(() {
                                      isInSmallMode = false;
                                    });
                                  });
                                });
                              }
                            },
                          ),
                        ),
                        alignment: currentAlignment,
                      ),
                      flex: 3,
                    ),
                    currentAlignment == Alignment.topCenter
                        ? Expanded(
                            flex: 6,
                            child: Container(
                              child: Column(
                                children: <Widget>[
                                  Row(),
                                  Padding(
                                    padding: const EdgeInsets.all(8.0),
                                    child: Card(
                                      child: Padding(
                                        padding: const EdgeInsets.all(8.0),
                                        child: Text('Video Recommendation'),
                                      ),
                                    ),
                                  ),
                                  Padding(
                                    padding: const EdgeInsets.all(8.0),
                                    child: Card(
                                      child: Padding(
                                        padding: const EdgeInsets.all(8.0),
                                        child: Text('Video Recommendation'),
                                      ),
                                    ),
                                  ),
                                  Padding(
                                    padding: const EdgeInsets.all(8.0),
                                    child: Card(
                                      child: Padding(
                                        padding: const EdgeInsets.all(8.0),
                                        child: Text('Video Recommendation'),
                                      ),
                                    ),
                                  ),
                                ],
                              ),
                              color: Colors.white,
                            ),
                          )
                        : Container(),
                    Row(),
                  ],
                );
              })
            : Container()
      ]),
      bottomNavigationBar: BottomNavigationBar(
        items: [
          BottomNavigationBarItem(
            icon: Icon(
              Icons.home,
              color: Colors.black54,
            ),
            title: Text("Home",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              FontAwesomeIcons.fire,
              color: Colors.black54,
            ),
            title: Text("Trending",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.subscriptions,
              color: Colors.black54,
            ),
            title: Text("Subscriptions",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.email,
              color: Colors.black54,
            ),
            title: Text("Inbox",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.folder,
              color: Colors.black54,
            ),
            title: Text("Library",
                style: TextStyle(
                  color: Colors.black54,
                )),
          ),
        ],
        type: BottomNavigationBarType.fixed,
      ),
    );
  }
}
```
