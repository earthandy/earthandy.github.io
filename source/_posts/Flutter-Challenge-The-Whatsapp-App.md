---
title: Flutter Challenge The Whatsapp App
date: 2022-01-29 00:09:32
summary: Flutter Challenges will attempt to recreate a particular app UI or design in Flutter.This challengs will attempt the home screen of the Whatsapp Android app.
keywords: Flutter,Challenge,Whatsapp,Whatsapp App
categories: Flutter
tags:
  - Whatsapp
---

![whatsapp](https://s2.loli.net/2022/01/29/gXrMsFU5ELHwamO.jpg)

Flutter Challenges will attempt to recreate a particular app UI or design in Flutter.

This challengs will attempt the home screen of the Whatsapp Android app.
Note that the focus will be on the UI rather than actually fetching messages.

### Getting Started

The WhatsApp home screen consists of

1. An AppBar with a search action and an menu
2. Four tabs in the bottom of the AppBar
3. A camera tab to take a photo
4. A FloatingActionButton for multiple purposes
5. A “Chats” tab to view all conversations
6. A “Status” tab to view all statuses
7. A “Calls” tab to view all past calls

### Setting up the Project

Let’s make a Flutter project named whatsapp_ui and remove all the default code leaving just a blank screen with the default app bar.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge WhatsApp',
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
          title: Text('WhatsApp'),
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

### The AppBar

The AppBar has the title of the app,and the two actions:Search and a menu.

Adding that to the AppBar,

```dart
AppBar buildAppBar() {
    return AppBar(
      title: Text(
        'WhatsApp',
        style: TextStyle(
            color: Colors.white, fontSize: 22.0, fontWeight: FontWeight.w600),
      ),
      actions: <Widget>[
        Padding(
          padding: const EdgeInsets.only(right: 20.0),
          child: Icon(Icons.search),
        ),
        Padding(
          padding: const EdgeInsets.only(right: 16.0),
          child: Icon(Icons.more_vert),
        )
      ],
      backgroundColor: whatsAppGreen,
    );
  }
```

This is the result of the code:

![whatsapp_appbar](https://s2.loli.net/2022/01/29/orcmUBxXzMRiN42.png)

Now moving on to

### The Tabs

The tabs are a simple extension to the AppBar and Flutter makes it extremely easy to implement them.

The AppBar has a “bottom” field which holds our tabs:

```dart
bottom: TabBar(
        tabs: <Widget>[
          Tab(
              icon: Icon(
            Icons.camera_alt,
          )),
          Tab(
            child: Text('CHATS'),
          ),
          Tab(
            child: Text('STATUS'),
          ),
          Tab(
            child: Text('CALLS'),
          ),
        ],
        indicatorColor: Colors.white,
      ),
```

Also,we need a TabController for this to work.

Create a new TabController.

```dart
TabController tabController;

 @override
 void initState() {
   super.initState();
   tabController = TabController(vsync: this, length: 4);
 }
```

Now add that controller to the “controller” field of both the TabBar.

```dart
bottom: TabBar(
        tabs: <Widget>[
          Tab(
              icon: Icon(
            Icons.camera_alt,
          )),
          Tab(
            child: Text('CHATS'),
          ),
          Tab(
            child: Text('STATUS'),
          ),
          Tab(
            child: Text('CALLS'),
          ),
        ],
        indicatorColor: Colors.white,
        controller: tabController,
      ),
```

And for the TabBarView

```dart
body: TabBarView(
          controller: tabController,
          children: <Widget>[
            Icon(Icons.camera_alt),
            Center(child: Text('Chat Screen')),
            Center(child: Text('Status Screen')),
            Center(child: Text('Call Screen')),
          ],
        ));
```

![whatsapp_home](https://s2.loli.net/2022/01/29/Apd1TzuUV9fqCIG.png)

Now before going to the individual pages,we’ll add the pages that the tabs represent. Switch the existing “body” code of the Scaffold with above code.

The children represent the pages that the tabs are an entire page is a Text widget for now.

### Floating Action Button

The Floating Action Button changes depending on which page is on screen.

First add a Floating Action Button in the Scaffold.

```dart
floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: fabIcon,
        backgroundColor: whatsAppGreenLight,
      ),
```

The “fabIcon” field simply stores which icon to display because we need to change which icon is displayed according to the screen displayed.

To listen to tab selected changes,attach a listener to the TabController.
When tab controller realises the page has changed,change the FAB icon.

```dart
tabController = TabController(vsync: this, length: 4)
      ..addListener(() {
        setState(() {
          switch (tabController.index) {
            case 0:
              break;
            case 1:
              fabIcon = Icon(Icons.message);
              break;
            case 2:
              fabIcon = Icon(Icons.camera_enhance);
              break;
            case 3:
              fabIcon = Icon(Icons.call);
              break;
          }
        });
      });
```



![whatsapp_tab_change](https://s2.loli.net/2022/01/29/eywHKPz38fbdMDQ.png)Moving on,

### The Chat Screen

The Chat Screen has a list of messages we need to display.To make a list of messages,we use a ListView.builder() and construct our items.

Let’s take a look at the list item for the chat screen.

![whatsapp_list_chat](https://s2.loli.net/2022/01/29/yJLAVmEPtw3poX5.png)

The outer most widget is a row of one icon and another row.

Inside the second row is a column consisting of one row and one text widget.

The row has the title and message date.

Let’s construct a Chat Item Model as a class for storing the details of the list item.

```dart
class ChatItemModel {
  String name;
  String mostRecentMessage;
  String messageDate;
  ChatItemModel(this.name, this.mostRecentMessage, this.messageDate);
}
```

Right now,I’ve omitted adding a profile picture for brevity.

```dart
ListView buildChatListView() {
    return ListView.builder(
          itemCount: ChatHelper.itemCount,
          itemBuilder: (context, position) {
            ChatItemModel chatItem = ChatHelper.getChatItem(position);
            return Column(
              children: <Widget>[
                Padding(
                  padding: const EdgeInsets.all(8.0),
                  child: Row(
                    children: <Widget>[
                      Icon(
                        Icons.account_circle,
                        size: 64,
                      ),
                      Expanded(
                        child: Padding(
                          padding: const EdgeInsets.all(8.0),
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: <Widget>[
                              Row(
                                mainAxisAlignment:
                                    MainAxisAlignment.spaceBetween,
                                children: <Widget>[
                                  Text(
                                    chatItem.name,
                                    style: TextStyle(
                                      fontWeight: FontWeight.w500,
                                      fontSize: 20.0,
                                    ),
                                  ),
                                  Text(chatItem.messageDate,
                                      style: TextStyle(
                                        color: Colors.black45,
                                      ))
                                ],
                              ),
                              Padding(
                                padding: const EdgeInsets.only(top: 2.0),
                                child: Text(chatItem.mostRecentMessage,
                                    style: TextStyle(
                                      color: Colors.black45,
                                      fontSize: 16.0,
                                    )),
                              )
                            ],
                          ),
                        ),
                      ),
                    ],
                  ),
                ),
                Divider(),
              ],
            );
          },
        );
  }
```

After creating the first list this is the result:

![whatsapp_chat_listview](https://s2.loli.net/2022/01/29/IuvhHoFUynm7XpQ.png)

The other tabviews are very same as the chats’s tab. So we don’t talk about it.
The final code is here:

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Challenge WhatsApp',
      debugShowCheckedModeBanner: false,
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

class _MyHomePageState extends State<MyHomePage>
    with SingleTickerProviderStateMixin {
  Color whatsAppGreen = Color.fromRGBO(18, 140, 126, 1.0);
  Color whatsAppGreenLight = Color.fromRGBO(37, 211, 102, 1.0);

  TabController tabController;

  @override
  void initState() {
    super.initState();
    tabController = TabController(vsync: this, length: 4)..addListener(() {});
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: buildAppBar(),
      body: TabBarView(
        controller: tabController,
        children: <Widget>[
          Icon(Icons.camera_alt),
          Scaffold(
            body: buildChatListView(),
            floatingActionButton: FloatingActionButton(
              onPressed: () {},
              child: Icon(Icons.message),
              backgroundColor: whatsAppGreenLight,
            ),
          ),
          Scaffold(
            body: buildStatusListView(),
            floatingActionButton: FloatingActionButton(
              onPressed: () {},
              child: Icon(Icons.camera_enhance),
              backgroundColor: whatsAppGreenLight,
            ),
          ),
          Scaffold(
            body: buildCallsListView(),
            floatingActionButton: FloatingActionButton(
              onPressed: () {},
              child: Icon(Icons.call),
              backgroundColor: whatsAppGreenLight,
            ),
          ),
        ],
      ),
    );
  }

  ListView buildCallsListView() {
    return ListView.builder(
      itemBuilder: (context, position) {
        CallItemModel callItemModel = CallHelper.getCallItem(position);

        return Column(
          children: <Widget>[
            Padding(
              padding: const EdgeInsets.all(8.0),
              child: Row(
                children: <Widget>[
                  Icon(
                    Icons.account_circle,
                    size: 64.0,
                  ),
                  Expanded(
                    child: Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: <Widget>[
                          Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: <Widget>[
                              Row(
                                mainAxisAlignment:
                                    MainAxisAlignment.spaceBetween,
                                children: <Widget>[
                                  Text(
                                    callItemModel.name,
                                    style: TextStyle(
                                        fontWeight: FontWeight.w500,
                                        fontSize: 20.0),
                                  ),
                                ],
                              ),
                              Padding(
                                padding: const EdgeInsets.only(top: 2.0),
                                child: Text(
                                  callItemModel.dateTime,
                                  style: TextStyle(
                                      color: Colors.black45, fontSize: 16.0),
                                ),
                              ),
                            ],
                          ),
                          Icon(
                            Icons.call,
                            color: whatsAppGreen,
                          )
                        ],
                      ),
                    ),
                  )
                ],
              ),
            ),
            Divider(),
          ],
        );
      },
      itemCount: CallHelper.itemCount,
    );
  }

  ListView buildStatusListView() {
    return ListView.builder(
      itemBuilder: (context, position) {
        StatusItemModel statusItemModel = StatusHelper.getStatusItem(position);

        return Column(
          children: <Widget>[
            Padding(
              padding: const EdgeInsets.all(8.0),
              child: Row(
                children: <Widget>[
                  Icon(
                    Icons.account_circle,
                    size: 64.0,
                  ),
                  Expanded(
                    child: Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: <Widget>[
                          Row(
                            mainAxisAlignment: MainAxisAlignment.spaceBetween,
                            children: <Widget>[
                              Text(
                                statusItemModel.name,
                                style: TextStyle(
                                    fontWeight: FontWeight.w500,
                                    fontSize: 20.0),
                              ),
                            ],
                          ),
                          Padding(
                            padding: const EdgeInsets.only(top: 2.0),
                            child: Text(
                              statusItemModel.dateTime,
                              style: TextStyle(
                                  color: Colors.black45, fontSize: 16.0),
                            ),
                          )
                        ],
                      ),
                    ),
                  )
                ],
              ),
            ),
            Divider(),
          ],
        );
      },
      itemCount: StatusHelper.itemCount,
    );
  }

  ListView buildChatListView() {
    return ListView.builder(
      itemCount: ChatHelper.itemCount,
      itemBuilder: (context, position) {
        ChatItemModel chatItem = ChatHelper.getChatItem(position);
        return Column(
          children: <Widget>[
            Padding(
              padding: const EdgeInsets.all(8.0),
              child: Row(
                children: <Widget>[
                  Icon(
                    Icons.account_circle,
                    size: 64,
                  ),
                  Expanded(
                    child: Padding(
                      padding: const EdgeInsets.all(8.0),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: <Widget>[
                          Row(
                            mainAxisAlignment: MainAxisAlignment.spaceBetween,
                            children: <Widget>[
                              Text(
                                chatItem.name,
                                style: TextStyle(
                                  fontWeight: FontWeight.w500,
                                  fontSize: 20.0,
                                ),
                              ),
                              Text(chatItem.messageDate,
                                  style: TextStyle(
                                    color: Colors.black45,
                                  ))
                            ],
                          ),
                          Padding(
                            padding: const EdgeInsets.only(top: 2.0),
                            child: Text(chatItem.mostRecentMessage,
                                style: TextStyle(
                                  color: Colors.black45,
                                  fontSize: 16.0,
                                )),
                          )
                        ],
                      ),
                    ),
                  ),
                ],
              ),
            ),
            Divider(),
          ],
        );
      },
    );
  }

  AppBar buildAppBar() {
    return AppBar(
      title: Text(
        'WhatsApp',
        style: TextStyle(
            color: Colors.white, fontSize: 22.0, fontWeight: FontWeight.w600),
      ),
      actions: <Widget>[
        Padding(
          padding: const EdgeInsets.only(right: 20.0),
          child: Icon(Icons.search),
        ),
        Padding(
          padding: const EdgeInsets.only(right: 16.0),
          child: Icon(Icons.more_vert),
        )
      ],
      backgroundColor: whatsAppGreen,
      bottom: TabBar(
        tabs: <Widget>[
          Tab(
              icon: Icon(
            Icons.camera_alt,
          )),
          Tab(
            child: Text('CHATS'),
          ),
          Tab(
            child: Text('STATUS'),
          ),
          Tab(
            child: Text('CALLS'),
          ),
        ],
        indicatorColor: Colors.white,
        controller: tabController,
      ),
    );
  }
}

class ChatItemModel {
  String name;
  String mostRecentMessage;
  String messageDate;
  ChatItemModel(this.name, this.mostRecentMessage, this.messageDate);
}

class ChatHelper {
  static var chatList = [
    ChatItemModel("Alice", "Lunch in the evening?", "16/07/2018"),
    ChatItemModel("Jack", "Sure", "16/07/2018"),
    ChatItemModel("Jane", "Meet this week?", "16/07/2018"),
    ChatItemModel("Ned", "Received!", "16/07/2018"),
    ChatItemModel("Steve", "I'll come too!", "16/07/2018")
  ];

  static ChatItemModel getChatItem(int position) {
    return chatList[position];
  }

  static var itemCount = chatList.length;
}

class StatusItemModel {
  String name;
  String dateTime;

  StatusItemModel(this.name, this.dateTime);
}

class StatusHelper {
  static var statusList = [
    StatusItemModel("Jane", "Yesterday, 11:21 PM"),
    StatusItemModel("Jack", "Yesterday, 10:22 PM")
  ];

  static StatusItemModel getStatusItem(int position) {
    return statusList[position];
  }

  static var itemCount = statusList.length;
}

class CallItemModel {
  String name;
  String dateTime;

  CallItemModel(this.name, this.dateTime);
}

class CallHelper {
  static var callList = [
    CallItemModel("Alice", "Today, 3:39 PM"),
    CallItemModel("John", "Today, 4:41 PM")
  ];

  static CallItemModel getCallItem(int position) {
    return callList[position];
  }

  static var itemCount = callList.length;
}
```
