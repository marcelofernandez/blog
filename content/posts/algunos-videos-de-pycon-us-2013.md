---
author: mfernandez
category:
  - codear
  - programación
  - python
date: "2013-03-21T12:29:11+00:00"
guid: https://blog.marcelofernandez.info/?p=1281
title: Algunos videos de PyCon US 2013
url: /2013/03/algunos-videos-de-pycon-us-2013/

---
Ya están subiéndose al sitio [PyVideo](http://pyvideo.org/ "Python related video indexed so you can find it") los videos de la [PyCon US 2013](https://us.pycon.org/2013/ "PyCon US 2013") que está cerrando hoy. Algunas de las charlas que quería compartir, de temas que son particularmente de mi interés, son:

**[PyPy without the GIL - Armin Rigo](http://pyvideo.org/video/1739/pypy-without-the-gil "PyPy without the GIL")**:
PyPy has a version without the Global Interpreter Lock (GIL). It can run multiple threads concurrently. But the real benefit is that you have other, new ways of using all your cores. In this talk I will describe how it is possible (STM) and then focus on some of these new opportunities, e.g. show how we used multiple cores in a single really big program without adding thread locks everywhere.
{{< youtube id="Q9wf63flICs" >}}**[Python Profiling - Amjith Ramanujam](http://pyvideo.org/video/1770/python-profiling "Python Profiling Talk")**:
This talk will give a tour of different profiling techniques available for Python applications. We'll cover specific modules in Python for doing function profiling and line level profiling. We'll show the short comings of such mechanisms in production and discuss how to do sampled profiling of specific functions. We'll finish with statistical profilers that use thread stack interrogation.
{{< youtube id="QJwVYlDzAXs" >}}**[Making Apache suck less for hosting Python web applications - Graham Dumpleton](http://pyvideo.org/video/1773/making-apache-suck-less-for-hosting-python-web-ap "Making Apache suck less for hosting Python web apps")**:
It is not hard to find developers who will tell you that Apache sucks for running Python web applications. Is there a valid basis to such claims or have they simply been misguided by the views of others? This talk will endeavor to shine a light on the realities of and limitations in working with Apache, as well as the challenges in implementing the mod\_wsgi module for Apache.
{{< youtube id="k6Erh7oHvns" >}}**[Using futures for async GUI programming in Python 3.3 - Dino Viehland](http://pyvideo.org/video/1762/using-futures-for-async-gui-programming-in-python "Using futures for async GUI programming in Python 3.3")**:
In Python 3.2 a new feature was added for concurrent programming - futures. In Python 3.3 generators have been extended to allow returning from a generator with a value. In this talk we'll show how these features can be combined to create a rich and easy to use asynchronous programming model which can be used for creating highly responsive GUI applications or easy async programming.
{{< youtube id="oJQdX_w1vXY" >}}**[Kivy: Building GUI and Mobile apps with Python - Mathieu Virbel / Thomas Hansen](http://pyvideo.org/video/1701/kivy-building-gui-and-mobile-apps-with-python "Kivy Talk")**:
This talk will introduce the Kivy project (http://kivy.org). Kivy’s mission is to make building graphical user interfaces on any device fun, efficient, and pythonic.
The talk will focus on giving attendees an overview of how they can use kivy to build exiting UIs and mobile apps.
{{< youtube id="yPWj6k5MRak" >}}**[Make More Responsive Web Applications with SocketIO and gevent - Luke Sneeringer](http://pyvideo.org/video/1798/make-more-responsive-web-applications-with-socket "Make More Responsive Web Applications with SocketIO and gevent")**:
An explanation of how to implement a socket.io server in Python to serve websocket requests from browsers.
{{< youtube id="TH-ZCuOdrQE" >}}

El resto de los videos del evento, acá: [http://pyvideo.org/category/33/pycon-us-2013](http://pyvideo.org/category/33/pycon-us-2013)
