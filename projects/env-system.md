---
layout: default
---

# Environment Monitoring System

> This project was done during Junru's bachelor study. With the support of CST programme, DST, UIC.

<video style="width: 100%;" src="/assets/video/env-system.mp4" controls preload="none"></video>

This is an environmental monitoring system with a 4-axis drone with sensors, and a web-based back-end server.

The project was initially cooperated by me, [Garfieid Wu](https://github.com/wjqzero), and [Covey Liu](https://github.com/Curton), and now it is limit maintained by me.

This project started in October 2014, and the first version participated in the 2016 Undergraduate Electronic Design Contest – 2016 Embedded System Design Invitational Contest (Intel Cup, ESDC in short) held in July.

After the contest, several improvements were applied to this system. In those improvements, not only the original participants, other classmates also joined.

## Basic System Structure

![System Structure](/assets/img/env-system/structure.png)

The system follows a database-centred structure. The onboard devices will directly write collected data to the database, and the client side will also directly read the database.

The structure is simple and easy to implement. Consider the situations at that particular moment, I can't say this structure is bad. But its shortages are obvious. Since there will always be time difference and handling cost, the system can never be working at "real-time" data.

## The First Prototype

This was the only version that was put to the contest.

In this version, some "bad" designs were applied in today's point of view, made the software extremely hard to understand and maintain. But anyway, it was this set of code helped us won the award.

![52766898848](/assets/img/env-system/showcase.jpg)

### Compatibility Nightmare

In this contest, we were required to use the develop boards provided by Intel, which are [Genuino 101](https://ark.intel.com/products/92346/Genuino-101) and [Minnowboard Turbot](https://minnowboard.org/minnowboard-turbot/). Since Intel claimed the Genuino 101 compact with the Arduino standard library, we decided to migrate an open source flight controlling software to it. However, according to the teammate, it was impossible to migrate the project with only Arduino standard library, it was not enough to power the software.

Problems also appeared at data sending procedure. As the Internet of Things developer, *serial port* is the most common connection method between controllers and sensors. But at that moment, we had a limit choice: the only programming language we familiar with is Java, but the serial port library in Java was out of maintenance for a long time and we need to use JDBC to upload data to the server.

At the last moment, we used a serial port reading software which can save the data in text files, then applied Java to read the text files, parse the data, then upload the data.

### "Framework" on Server Side

![env-system-realtime-screenshot](/assets/img/env-system/realtime-screenshot.png)

The server-side program was written in PHP cooperate by me, [Covey Liu](https://github.com/Curton), [Jason Yang](https://github.com/JiayuYANG), and [Alicia Wang](https://github.com/JEUDominic).

We applied [CodeIgniter](https://www.codeigniter.com/) as the framework of this system. CI is a framework highly follows MVC design pattern. People always say that using framework can improve the efficiency of development. However, we had difficulties much more than benefits when applying this framework at that moment. The cons of applying framework are learning cost. I think for those experienced professional developers, they can easily figure out the design philosophy of a framework. But for us, it might be too difficult, especially we don't know the phrase "software engineering".

## Improvement: Data Uploading

After experiencing a complex procedure of sensor data collecting and uploading, we decided to use C# instead of Java to make a data collecting and uploading software in November 2016. It was reasonable because our Minnowboard Turbot ran a standard Windows 10 desktop. The source code can be found on [GitHub](https://github.com/billzhonggz/SerialPortReadWinForm).

![](/assets/img/env-system/csharp.png)

In this case, we applied out-of-box C# serial port library to read serial port data from the sensor, then the data string would be parsed according to our instructions. After that, we use a third-party MySQL library to upload directly to the database.

## Improvement: Android Mobile App

By November 2016, the system already had a web front-end. Cooperate with [Mike Liu](https://github.com/FizzHawkeye), we made an Android Mobile App had exactly same functions as the web front-end in April 2017, the source code can be found on [GitHub](https://github.com/billzhonggz/EnvAircraftClient).

![](/assets/img/env-system/android.png)

This application queried data by RESTful method (no more MySQL direct connection), and display the location data by Google Map. The connection between application and server was HTTPS instead of HTTP.

![](/assets/img/env-system/android-query.png)

## Refactor: Server Side

Time flies, I have already finished all of my major requires courses in my program. All of us are completely different from two years ago.

Base on another project I have just done, the server side of Intelligent Vehicle Locator & Alarm, I can migrate the existing code to adapt this system. The new project applied [Django](https://www.djangoproject.com), a web framework written in Python, [Tornado TCP server](http://www.tornadoweb.org/en/stable/), [Django REST framework](http://www.django-rest-framework.org/), and a de-coupled front-end written in JavaScript and [Vue.js](https://vuejs.org).

Both of these two projects follow the system structure I mentioned [above](#basic-system-structure). They still cannot get rid of the disadvantages of the database-centred structure. Maybe we can re-design the big picture of this project if there is a chance.

## Future: Bidirectional Controls

The so-call bidirectional controls mean the users can use their handheld devices to control the drone, like DJI's application [*DJI GO*](https://www.dji.com/goapp).

![](/assets/img/env-system/dji.png)

We have planned these functions almost in the very first version of the proposal. However, it is still not easy for us even today.