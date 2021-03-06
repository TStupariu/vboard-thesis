BABES-BOLYAI UNIVERSITY CLUJ-NAPOCA

FACULTY OF MATHEMATICS AND COMPUTER SCIENCE

COMPUTER SCIENCE, ENGLISH



DIPLOMA THESIS

Virtual Boards and Server-less Streaming



Supervisor:

Bogdan Pop

Sterca Adrian



Author:

Tudor Filip Stupariu



2018

---



# Abstract



This paper presents a concept platform that allows its users to create different rooms, public or private, and create content via graphics and text. Next, the content is transmitted in real time to other users via a video stream capturing the graphics being drawn. The platform also allows for users to comment in real time using a chat system. The platform consists of a web application based on an API created in Ruby on Rails and a front-end client based on VueJS.



This paper is structured as follows. Chapter 1 provides an introduction, the motive behind creating this application and why this solution is needed. Chapter 2 presents a thorough analysis of possible solutions to the problem described in Chapter 1. Chapter 3 presents the application both from a theoretical aspect and from a practical aspect while providing samples of code throughout. Chapter 4 brings a conclusion and a short summary of the solution presented while Chapter 5 contains the bibliography and inspiration used for the paper.



# Contents



1. Introduction

   1.1 Problem definition

   1.2 Web based graphics and WebRTC P2P Video Streaming

   1.3 General description and personal contribution

2. Current Theoretical Approaches

   2.1 Graphics and virtual boards

   2.2 Streaming approaches

   2.3 Problems with current solutions

   2.4 Solution to the problems

3. Application details

   3.1 Functionalities

   3.2 Design

      3.2.1 Backend Server

      3.2.2 Database

      3.2.3 Streaming Solution

      3.2.4 Web Graphics

      3.2.5 Server-less Computing

      3.2.6 UI and Front-end

   3.3 Implementation

      3.3.1 Backend Server

      3.3.2 Database

      3.3.3 Streaming Solution

      3.3.4 Web Graphics

      3.3.5 Server-less Computing

      3.3.6 UI and Front-end

   3.4 Validation and Testing

   3.5 Benefits of proposed solution

   3.6 Personal Contribution

4. Conclusion

   5.1 Drawing the line

   5.2 Future directions

5. Bibliography




# 1. Introduction



This chapter is aimed at providing a short inside look to the problem this platform is trying to solve and how/why it's doing things the way it currently is implemented.

This paper is structured as follows. Chapter 1 provides an introduction, the motive behind creating this application and why this solution is needed. Chapter 2 presents some current possible solutions to the problem described in Chapter 1, and analyses their issues and faults. Chapter 3 goes through the main solutions introduced by the application and why they are superior to current solutions. Chapter 4 presents the application in details, both from a theoretical aspect and from a practical aspect, also providing samples of code throughout. Chapter 5 brings a conclusion and a short summary of the solution presented, while Chapter 6 contains the bibliography and inspiration used for the paper.



## 1.1 Problem Definition



Collaboration and remote teaching tools have existed for a long time, but they have always been platform dependent or hard to use. Nowadays, the internet represents a key part of people's lives and integrates with them more than ever, so it was just a matter of time until collaboration tools would go to a web only approach. The problem of drawing graphics on the web has always existed and different solutions have been available for some time now. One of the most popular ones,  Adobe Flash, was not always reliable and has received limited support and development in recent years. Since the introduction of HTML5, a new element was added called "canvas" and along with it an API developers can interact with using JavaScript. [1]

> Third, there’s reliability, security and performance.
>
> Symantec recently highlighted Flash for having one of the worst security records in 2009. We also know first hand that Flash is the number one reason Macs crash. We have been working with Adobe to fix these problems, but they have persisted for several years now. We don’t want to reduce the reliability and security of our iPhones, iPods and iPads by adding Flash.
>
> In addition, Flash has not performed well on mobile devices. We have routinely asked Adobe to show us Flash performing well on a mobile device, any mobile device, for a few years now. We have never seen it. Adobe publicly said that Flash would ship on a smartphone in early 2009, then the second half of 2009, then the first half of 2010, and now they say the second half of 2010. We think it will eventually ship, but we’re glad we didn’t hold our breath. Who knows how it will perform?
>
> Steve Jobs, 2010

[2]

> But as open standards like HTML5, WebGL and WebAssembly have matured over the past several years, most now provide many of the capabilities and functionalities that plugins pioneered and have become a viable alternative for content on the web. Over time, we’ve seen helper apps evolve to become plugins, and more recently, have seen many of these plugin capabilities get incorporated into open web standards. Today, most browser vendors are integrating capabilities once provided by plugins directly into browsers and deprecating plugins.
>
> Given this progress, and in collaboration with several of our technology partners – including [Apple](https://webkit.org/blog/7839/adobe-announces-flash-distribution-and-updates-to-end/), [Facebook](https://developers.facebook.com/blog/post/2017/07/25/Games-Migration-to-Open-Web-Standards/), [Google](https://www.blog.google/products/chrome/saying-goodbye-flash-chrome/), [Microsoft](https://blogs.windows.com/msedgedev/2017/07/25/flash-on-windows-timeline/) and [Mozilla](https://blog.mozilla.org/futurereleases/2017/07/25/firefox-roadmap-flash-end-life/) – Adobe is planning to end-of-life Flash. Specifically, we will stop updating and distributing the Flash Player at the end of 2020 and encourage content creators to migrate any existing Flash content to these new open formats.
>
> Adobe Blog

Another problem has been streaming while keeping costs down. Big platforms like Twitch have been doing it using intermediate servers to handle the stream data and distribution, but for small groups of people this is a really costly solution since the processing power required by the servers in incredibly high. [3]

> First of all, by getting the data from the first user, the  second subscriber wouldn’t access *House of Cards* from Netflix’s servers, which would mean that in total, about the same amount of data would change hands. And in reality, there wouldn’t just be two people watching the same content, but likely thousands
>
> Janko Roettgers

[ 4 ]

> WebRTC is a modern protocol supported by modern browsers. It uses UDP, allows for quick lossy data transfer as opposed to RTMP which is TCP based. WebRTC has very high security built right in with DTLS and SRTP for encrypted streams, whereas basic RTMP is not encrypted. There are many other advantages to using WebRTC over RTMP, but it's not always the right choice.

Combining the two, remote learning and teaching has always been a bit of a challenge, and this platform aims to make it easy to use and integrate seamlessly in the day-to-day life of its users while also providing enough tools for someone to manage to be productive using the application.



## 1.2 Web Based Graphics and WebRTC P2P Video Streaming



Drawing graphics on the web isn't really a challenge anymore. The problem comes when developers have to add user interaction. Just displaying shapes has always been decently easy, but making a user dynamically draw shapes is not the simplest of tasks. Further more, former solutions involved plugins which some users may not have installed or it may confuse a user about why the platform does not seem to work. This platform provides a general solution using a standard provided by HTML itself.



![ximg_559f5e897a1c0.png.pagespeed.gp+jp+jw+pj+ws+js+rj+rp+rw+ri+cp+md.ic.KniA4YevUK](assets/ximg_559f5e897a1c0.png.pagespeed.gp+jp+jw+pj+ws+js+rj+rp+rw+ri+cp+md.ic.KniA4YevUK.png)



WebRTC peer-to-peer video streaming is also a new idea on the web. Peer-to-peer file transfers have existed for a long time, but just recently there has been introduced a standard for managing such connections that also support data, video and audio streams in the form of WebRTC P2P video streaming. This is a standard/API that can easily be accessed by JavaScript on the web. It provides a general solution for any user on any platform, without requiring extra plugins or intermediate servers to handle the data, which might introduce more latency in the stream feed. [5]



## 1.3 General Description and Personal Contribution



This application provides a simple to use web client accessible by any user through a web browser. It provides authentication in order to keep the platform secure and allows the user to create both public and password protected private groups. Once in a group, the initiator can start the stream while the other attendees can start watching a video feed. The initiator has a canvas on which he can freely draw graphics, text, images and shapes. Afterwards, that information is transmitted live through the video feed. It also provides a form of live interaction through a chat such that the attendees can freely ask questions or leave comments.

I have personally developed the API for the web client and implemented the authentication, database structure and back-end logic. Also, I created the web client and layout of the interface. The logic behind signalling peers and live chat has been implemented from scratch, as well as all the tools for drawing on the canvas combined with the user interaction with these tools. Finally, I had to manage connections between different technologies such as a regular back-end server hosting an SQL database and a Firebase Realtime Database.



# 2. Current Theoretical Approaches



This chapter describes the most used current approaches to the problem. It also goes a little bit into each approach and its downsides for the goal of achieving a low cost and easy to use streaming and drawing learning platform.



## 2.1 Graphics and virtual boards



Currently there are multiple other approaches for drawing on a blank canvas on the web without the need for native clients. Some of these include SVG graphics and Flash drawing.

SVG graphics work pretty well performance-wise. Moreover, since they are vector graphics they scale really well. The problem is that graphics display is not the only requirement. Allowing users to create them is also extremely important. Such advanced SVG tooling is trickier at best. The handling of shapes, text or other types of primitive graphics is harder to implement and doesn't make too much sense in an SVG environment.

Flash could be a viable option, but it is an old technology that has started to lack support in the recent years. First and foremost, users do need a plugin for running flash applications which makes it much less user friendly. Secondly, Flash's developer, Adobe, responsible for building, maintaining and supporting the technology has released statements that Flash support will be dropped by the year 2020. The things that made Flash stand out in the earlier days is most of the  times no longer unique or needed since other web standards have come to take their spot.



## 2.2 Streaming Approaches



Streaming in general is an action that requires a lot of resources, both computational and network ones. Traditionally, streaming has been done through standalone servers used exactly for that purpose. While they do produce decent scalability for a big number of concurrent users trying to watch a stream they also introduce a couple problems with the main one being high costs. The server cost is the highest since it is a resource hungry activity. Therefore maintenance and scalability can be quite costly. Further more, since this application is made for small rooms of people it will not directly benefit from the ability to handle a great amount of users watching a stream at the same time. [6]



## 2.3 Problems with current solutions



Current solutions are either too old, cumbersome or expensive to implement and maintain. Server resources are expensive, and the more processing that can be done on the client, the cheaper the maintenance cost of the application will be. Having more servers and nodes that a stream has to pass through can also introduce more latency along the line. Making the user interaction as straight-forward as possible is not so easy using these technologies. [7]



## 2.4 Solutions to the problems



Every problem has a solution. Graphics are the starting point. The release of HTML5 has brought a new element: canvas with an API that allows advanced programming. It offers developers an editable area that does not rely on a DOM for creating graphics. It also needs no other external tool or extension. The interaction and drawing rely solely on the browser's Javascript engine. Its main advantages are ease of use and the effortless user interaction it can provide. Furthermore, this API offers an easy way to capture a video stream of the actions and graphics being drawn in real time. [8]

For the streaming part, the solution for expensive server resources is eliminating servers all together. WebRTC P2P video streaming is a technology optimised for streaming data, including video, between multiple peers directly. This way, the system only needs a server for signalling clients when a connection offer is incoming, but no actual video stream data passes through it. The connection for the video stream is made directly between the 2 clients' browsers, minimising the needed server resources.



# 4. Application Details



This chapter goes in depth into describing the applications and all the core components that make the whole platform work the way it should.



## 4.1 Functionalities



This first thing a user sees when going onto the platform is a login screen where he can create/sign into an already existing account. This is the only way to use the platform and confirm his identity, through a secure authentication form.



![Screen Shot 2018-06-09 at 17.40.31](assets/Screen Shot 2018-06-09 at 17.40.31.jpg)



Next, a user has two options. Either browse the already existing rooms, or create his own. In the case where a new room is created there is a simple form where the streamer has to input some details, and if the room is meant to be password protected a new one will have to be written down in an input. Afterwards, the application will automatically redirect to that specific room's page. In case a user wants to join an existing room, there will be a need to browse to his preferred one and click to join. If the room is private, a password be required to be input corresponding to that room.

Afterwards the streamer will be able to go to the actual creation page, where canvas and a chat will be displayed simultaneously on the page. In the case of the viewer, a redirect to the view page containing the stream video and the chat will be made.

As an owner, the canvas will have several tools, from shape drawing to image insertion and text. As a viewer, basic video controls like play/pause will be provided.

All parties have unlimited access to the chat which runs along in real time with the stream, allowing seamless interaction between teachers and students.



![Screen Shot 2018-06-10 at 22.39.36](assets/Screen Shot 2018-06-10 at 22.39.36.jpg)



## 4.2 Design



This sub chapter describes the theoretical aspects and the tools used for creating the application.



### 4.2.1 Backend Server



The language used for handling all the back-end logic is Ruby in conjunction with the Rails framework. It works as a stand alone API allowing for future integrations with mobile, desktop or any other type of client. Ruby is a dynamic, interpreted, reflective, object-oriented programming language designed and developed in the 1990s. While it can work as a standalone programming language, its popularity has really grown with the rise of Rails, a framework for easy and rapid development of web applications. The current iteration of Ruby used in this project is Ruby 2.3.3, while Rails' version is 5.1.4 . Rails is a server-side web application framework that uses a general MVC pattern for data transmission. [9]

[10]

> Startup friendly, flexible, well-supported — what else can we add about Ruby on Rails framework? A bunch of successful startups can tell you why they prefer using Ruby on Rails to build their websites. In the programming world, Ruby on Rails is strongly associated with startups.
>
> Ruby Garage

The way Ruby on Rails uses MVC in this project is as follows: The model properties are being defined in migrations while different limitations and validations on those properties are done into their own file. Using these models that automatically map to the tables in the database using the Active Record pattern, the controller receives all inbound HTTP requests and handles the data, later returning the result to the view in JSON format. [11]



![Screen Shot 2018-05-24 at 13.29.57-7157830](assets/Screen Shot 2018-05-24 at 13.29.57-7157830.jpg)



One advantage and feature that makes Ruby on Rails stand out from other frameworks and helps in creating a really agile development environment is its convention over configuration philosophy. This is a software design paradigm that aims to reduce the amount of tedious decisions a developer has to take in order to increase the time allocated for developing actually important features. This way, a lot of things like views, controllers and models aren't tied together using import statements, dependency injection or other techniques. Instead, they are connected through naming conventions. These conventions will speed up development and keep the code concise and readable. Most important though, they allow developers to easily navigate within the application. [12]

> Not only does the transfer of configuration to convention free us from deliberation, it also provides a lush field to grow deeper abstractions. If we can depend on a Person class mapping to people table, we can use that same inflection to map an association declared as has_many :people to look for a Person class. The power of good conventions is that they pay dividends across a wide spectrum of use.
>
> But beyond the productivity gains for experts, conventions also lower the barriers of entry for beginners. There are so many conventions in Rails that a beginner doesn’t even need to know about, but can just benefit from in ignorance. It’s possible to create great applications without knowing why everything is the way it is.
>
> David Heinemeier Hansson, 2016

The mapping between the controller actions and the HTTP requests is done using a routing module that redirects every inbound request to its corresponding controller method. This application uses a combination of GET and POST requests in order to handle all CRUD (Create-Read-Update-Delete) operations. [13]

Authentication is another point being handled by the server. The application uses a gem called devise-jwt allowing for easy authentication using JSON Web Tokens. This provides a really adaptable authentication scheme that is able to work on the web, mobile, desktop, and virtually any platform a client would be on. The passwords are hashed and salted when stored in the database so that at no time the application knows the actual credentials of a user. This module also allows for future expansion supporting social media authentication using Google, Facebook or other providers. [14]

The transfer of data from the server is done through views which contain JSON strings. These strings are rendered using a gem called Jbuilder. This extension gives a user a simple DSL for declaring JSON structures, coming in handy especially in places where you are working with big and complicated data structures. It can loop through your structure and create a dynamic model for transmitting the data. [15]

This being an API, it has to be able to support CORS (Cross-Origin Resource Sharing). Without it, a web app could not work in browsers like chrome, since the back-end server and the client would not be on the same server. The gem rack-cors allows you to easily enable this functionality through header alteration while also making it easy to restrict allowance to certain domains. [16]



### 4.2.2 Database



The application currently uses a simple file-based database called SQLite. It is ideal for development since the setup takes virtually no time at all, especially since Rails uses it as a default database. SQLite is a relational database management system contained in the C language. It being embedded in the application itself, without the need of a server has both advantages and disadvantages. The bad part is that the size of it can grow quickly and can become a burden for your back-end server. On the bright side it is really portable, easy to setup and makes for a great tool during the development process. [17]



![Screen Shot 2018-06-09 at 18.50.50](assets/Screen Shot 2018-06-09 at 18.50.50.jpg)



One big advantage of Rails is that it makes it painless to migrate to another relational database system. It uses an abstraction layer over your database so that you never have to manually write SQL queries or modify tables. If you decide to switch to another database system like PostgreSQL you simply have to modify a couple configuration files. The abstraction layer is called Active Record and it is based on a pattern carrying the same name. In a nutshell, what it does is it maps every single table to an entity that is globally available throughout the application controllers, this way also removing the need for separate repositories. [18]

Another big helper flexibility-wise are ActiveRecord Migrations. They provide a code-first database building workflow which allows you to gradually modify the database. Further more, it provides an easy way of reverting back to older versions of it. A developer does not have to manually migrate databases from computer to computer, making the database creation and seeding just a terminal command away. Facilitating the versioning of the Database together with the server is also a strong point of ActiveRecord Migrations. [19]



### 4.2.3 Streaming Solution



In order for the platform to allow a user to stream the content of a canvas it uses a streaming standard called WebRTC. WebRTC is an open source project aimed to provide browsers and mobile applications an easy access to Real Time Communication capabilities via APIs. The part of this technology used by this application is Peer-To-Peer Video Streaming, allowing video streams between users without the data needing to pass through intermediary servers. How it currently works is that the peers are creating a unique id and using a simple signalling server made for letting peers know about each other, afterwards making the connection between clients and starting the video stream. Current supported platforms include Google Chrome, Firefox, Opera, Android and IOS, but many more browsers seek to adopt it. Furthermore, this technology is optimised for data streams, this way keeping a smaller footprint on your network traffic while keeping the stream quality high. [20]

The application also uses a wrapper over this standard in the form of a JavaScript library called simple-peer. This provides a simpler to use API for communicating between peers and a default server for the peers to get their unique ids from. This wrapper supports both data streams and video/voice streams. [21]



### 4.2.4 Web Graphics



A user has to be able to draw things somewhere in order to stream them and in order to do that the streamer will be using an HTML Canvas element. This element exposes an API that can be interpreted with JavaScript. This results in a virtual board that can be drawn or written upon. [22]

Rather than using the native canvas API, the application uses a wrapper for drawing certain types of shapes, text or images. FabricJS is a library made for displaying graphics easier on a canvas element. It also allows setting different behaviours of elements like drag and drop or rotation. The difference between this and the lower level API a Canvas element provides is that it abstracts all the elements into an object-oriented style. This way anything you draw will become a simple JavaScript object with properties, allowing for easier management and drawing of graphics. [23]

While FabricJS does provide a more intuitive way of drawing and manipulation objects in an image, the actual process of defining where to draw items on a canvas has to be handled separately. Currently it is done using a combination of JavaScript event handlers for figuring out the state of the mouse on a canvas. This way, it can easily detect when a user clicks, drags or holds an element and react accordingly while drawing the shapes. All the mechanics for drawing had to be implemented from scratch since they need to be really customisable dependent on what the drawn item is. [24]



![Screen Shot 2018-06-09 at 19.15.16](assets/Screen Shot 2018-06-09 at 19.15.16.jpg)



Also, in order to get a stream from the Canvas element, the application uses an HTML5 API called captureVideoStream. This allows you to set a target frame-rate to capture depending on your internet connection and requirements and pass it onwards to the WebRTC connection in order to be streamed onwards. Also, it combines an audio feed from the microphone, so that users can also hear explanations offered by a teacher. [25]



### 4.2.5 Server-less Computing



Some of the features provided are available using a technology called Firebase. To name them exactly, the Chat and the Signalling Server are both made using the Firebase Real-Time Database. The Firebase Realtime Database is a NoSQL database hosted in the cloud that stores information in JSON format. Being Realtime, it constantly synchronises with other connected clients using sockets and event listeners. [26]

For the application chat, this technology is useful since you don't have to implement a socket interface from scratch. Further more, all communications are done without using the server, just Firebase. By attaching an event listener to certain nodes of the JSON structure a client can be automatically notified of a change if another client decides to alter the structure of that node or the nodes below. [27]

Firebase also comes into help for peer signalling. In order for 2 clients to connect and establish the stream they need to first know about each other and accept each other's offers. For this, there is a certain sequence to follow starting from the initiator of the stream. Firebase allows clients to automatically be notified when an offer or answer is present and react according to that. This eliminates the need for implementing a socket interface or HTTP polling, making the connection easier and faster.



### 4.2.6 UI and Front-End



The client of the application is web based and uses a library called VueJS. It is an open-source JavaScript library for building user interfaces. It is a versatile, easy to adapt/integrate and performant library having a really small footprint comparing to other frameworks. [28]



![JS_Size](assets/JS_Size.PNG)



It represents the perfect compromise between big frameworks like Angular 5 that contain every package one might want or don't want and small packages like React while also keeping up and out-performing traditional frameworks performance wise. It uses a component based system, allowing for easy re-rendering and communication between different parts of the application.



![maxresdefault](assets/maxresdefault.jpg)

[29]

> Material Design is a unified system that combines theory, resources, and tools for crafting digital experiences.
>
> William Woodhead

In order to not have to design every component from scratch the system uses a Material Design based library called Vuetify. Other than providing an intuitive way to customise theming and elements it also provides a a grid system for designing complicated layouts. It is inspired by the 12-column Bootstrap grid while also having some extra Material Design specific elements. Material Design is a design language and guideline developed in 2014 by Google expanding on the card based interface that was first introduced in Google Now. It provides a clean and minimalistic look for components while keeping functionality richness and a clean user experience. Another great part about Vuetify components is their high reusability allowing you to duplicate elements across different parts of the website while retaining design and behaviour without the need to manually duplicate code and logic. [30]

In order to communicate with the Ruby on Rails server, the platform uses a JavaScript library called Axios. Its advantage over native APIs such as fetch is that is provides a cleaner interaction and also it supports intercepting requests in order for transformation of attributes like headers. More than that, the code used for making an HTTP request is much smaller in size, allowing for a more friendly representation between lines of code.

Another part from where the client gets its data is Firebase. On the web there are currently 2 ways of communicating with a Firebase Realtime Database, one being using HTTP requests with the other being using their proprietary SDK. The advantage of the SDK is that is allows for easy subscription to their event listener system so that updates to the database are automatically transmitted to all clients. Also, Firebase uses a caching system where it caches all the data between requests except for the first one in order to allow for faster transfers. The SDK is easy to setup using just a configuration file and it provides a service-like system of interaction.

Since it is developed as a Single Page Application, the usage of a router is required for navigating between pages and components. This is why the app is using the officially supported library for routing called vue-router. It provides a way for dynamically displaying components dependent on user interaction. Further more, different guards can be set up on routes so that a user is forced to authenticate before accessing certain parts of the system, this way restricting access to certain parts of a website with more sensible data to unauthenticated users. [31]

The routing and its guards are based on the HTMl localStorage API. Basically, on every request the client has to include a JSON Web Token in order to validate with the server the identity of the user. Afterwards, as a response, the client is given a different token for every request, this way increasing the security of the application. The system then stores this token in the localStorage of the page. [32]

Despite using Vuetify as a component library there are times when classic CSS rules have to be applied in a custom manner. For this, instead of using classic CSS which has some limitations like no variable support or the fact that rule selectors can become really verbose, the application uses SCSS which then gets interpreted to standard CSS. Basically, it is a Sass syntax and an extension to traditional CSS that also provides a syntax for shortening the stylesheet code you have to write while also providing useful add-ons like variables or mix-ins. Overall, while it does not add anything to the final user experience, the place where it really shines is in developer satisfaction since SCSS is much easier to understand rapidly and code complicated selectors and rules. [33]

In order to bundle all the JavaScript files together for usage in the browser, the Vue-CLI (Command Line Interface) comes with WebPack built in, this way not requiring any additional configurations or setups.

There are plenty of external JavaScript packages bundled in the application and, in order to manage them all, a package manager like NPM (Node Package Manager) or YARN is used. This way, other than creating an easy way to add packages to a solution it also provides a really simple way of keeping track of all modules and their dependencies while simplifying the way code can be migrated from one computer to the another.



##4.3 Implementation



This chapter describes the implementation of all the technologies described above. With the theoretical part in mind, this chapter's goal is to explain how these technologies are used and how they are tied together to create one cohesive application.



### 4.3.1 Backend Server



 A big part of the way Ruby on Rails works is based on the convention over configuration programming paradigm. In this case a developer can see that in action using the Profile model, controller and the Profile views set. There is no other necessary import or dependency injection; the only way Rails knows about all these files is from its naming conventions.

The flow from when an HTTP request comes in and to the point where data is returned to the client is as follows: The request reaches a routing table where it will be assigned a controller and an action. Afterwards it moves on to the actual controller where processing is being done. Rails automatically matches the view corresponding to the action by using the Controller and Action names and returns that data. Let's take the example of an HTTP POST request to "/profile/create". First of all, the routing table finds the first match of the URL string and assigns that specific request the appropriate controller. In this case, if a request is a POST to '/profile/create' it will redirect the processing to the profile controller, at an action called 'create'.



![Screen Shot 2018-05-27 at 13.33.12](assets/Screen Shot 2018-05-27 at 13.33.12.jpg)



Once it reaches the controller, there is an extra step the application will have to perform before starting the processing of data. The before_action tag specifies that each time a request comes in it will have to go through some extra steps. In this case, in order to support authentication and to properly secure the controller this piece of software has to first check the validity of the JWT token. Devise takes care to validate the identity of the computer making the request and in this case some exceptions to the validation are in place for the actions 'index' and 'getByUserId' since not all methods of an API have to be private. Basically, a user has the possibility to view all the user profiles or search for a profile based on an ID, but without being properly authenticated he cannot create new user profiles. This way, anyone is allowed read access to this part of the database, though only authenticated users are allowed to perform any modifications.

The next step it does is it matches the name from the routing table to an action in the controller, in this case it being 'create'. The parameters of the request are being accessed using a map called 'params'. Also, the controller doesn't have to explicitly specify what does it want to render in the view, it can simply store those bits in variables with an '@' sign before. Basically, this allows a variable to have such a scope that it can be read by Jbuilder from the view. What this allows for is for the controller to only handle the data processing, not needing to care about the rendering logic.



 ![Screen Shot 2018-05-27 at 13.35.24](assets/Screen Shot 2018-05-27 at 13.35.24.jpg)



Next, the view engine is invoked and it will render the entire @profile variable. It had the possibility to access this since it has been declared with the '@' sign in front.



![Screen Shot 2018-05-27 at 13.45.17](assets/Screen Shot 2018-05-27 at 13.45.17.jpg)



There are multiple ways of defining a model in Ruby on Rails, but one of the most popular ones is leaving the data properties of models in migration files and in the generated schema file while keeping the validations in the model file. This way it creates a cleaner way of setting up complex validations without having a lot of attributes declared in a file.



![Screen Shot 2018-05-27 at 13.47.42](assets/Screen Shot 2018-05-27 at 13.47.42.jpg)



Another important part of the application is Devise, the gem that handles authentication. It does everything from hashing and salting the passwords before storing them in the database, creating and passing around JSON Web Tokens and checking the validity and identity of every request. It supports a lot of features like email confirmation, password recovery and social media integration, and in this case it is set up for email authentication, password recovery, tracking of user authentication activity, validation of passwords and emails and social media authentication.



![Screen Shot 2018-05-27 at 13.52.00](assets/Screen Shot 2018-05-27 at 13.52.00.jpg)



Since this is an API, it will need to support Cross-Origin Resource Sharing, and for this the rack-cors gem is used. What it basically does it attaches some specific Access Control headers to every response so that the browser knows that the application is setup for handling multiple origins for requests.



![Screen Shot 2018-05-27 at 13.55.58](assets/Screen Shot 2018-05-27 at 13.55.58.jpg)



Finally, Jbuilder is used for rendering the views. Rails comes with it setup out of the box so little to no configuration is required. While there are multiple alternatives to JBuilder, this gem is used in this context since it makes it easy to render complex relations and model the response according to the needs of the client.



### 4.3.2 Database



Ruby on Rails comes with SQLite configured by default and for development purposes there is no need to switch to a server based SQL Database. The schema for this is quite a simple one, but it solves all the current needs of the platform. At the end of the day, the only purpose of this backend server is to handle authentication and manage the different groups and users belonging to those groups since no part of the stream will actually pass through this server. Other than the attributes declared in the migrations files themselves, Devise adds a couple columns to the user model since it would need these for tracking user activity through authentication. Only 4 main tables are required in order to run the application, those being 'users', 'profiles', 'rooms' and 'user_rooms'.



![erd](assets/erd.png)



One of the most important features of the entire Ruby on Rails ecosystem combined with Active Record is support for database migrations. They allow a developer to easily create versions of the database while also making the reversion to older versions a breeze. Each migration is basically a class that has multiple changes defined. They can range from adding, removing, altering tables and even inserting SQL code manually. Another advantage is that unlike other migration solutions for other languages like Entity Framework, you only really have to define the way the migrations go upwards. This means that a developer will have to define how it will behave going from, let's say 1.1 to 1.2, but if there is a need to revert the migrations Ruby on Rails will figure out by itself what and how it wants to revert the database. This way it removes user error when manually writing reversion migrations. [34]

Security is a big problem especially on the internet these days, with data breaches becoming a common issue every single day. In order to mitigate this developers have to actively think about security while developing the features of their application. This is yet another place where Active Record comes to play since it does a lot of data validation for security purposes for you. For example, as long as the project is using their well defined syntax without having manually written SQL queries developers do not have to worry about SQL injection since it does string escaping out of the box. It has the possibility to watch out for any types of special characters and eliminate them from a potentially malicious string. [35]

For development purposes, seeding the database is also a popular option in order to avoid creating some dummy entities each time you reset the database. Rails offers an easy solution for this in the form of a file called 'seed.rb'. As a developer you only need to define some entities in there and that file can be run at any time using a simple command line tool. This application automatically generates a couple users, rooms and some links between them in order to make testing a little easier and straight forward. While this is ideal for a testing environment it can easily be excluded in case of deploying the latest modification to a production environment.

The last important thing that Rails and Active Record handle are relations. Since the whole ideology of using migrations is a code-first attitude towards databases, meaning that you should never have to touch the database directly, relations are also set directly in the models themselves without manually having to add foreign keys to databases. Further more, Rails can automatically generate join tables for many to many relations but custom join tables can also be manually defined if more properties are needed. Again, using conventions over configurations, in most relation cases there is no need to define the property which stores the foreign key ID, since Rails will figure it out itself from the naming of the entities.



![Screen Shot 2018-05-27 at 14.27.22](assets/Screen Shot 2018-05-27 at 14.27.22.jpg)



### 4.3.3 Streaming Solution



For the video live streaming part of the application a JavaScript library called 'simple-peer' was used that acts as a wrapper over WebRTC Peer-To-Peer video streaming. The way it currently works is once the streamer finished loading his page the program will start capturing a video stream of the canvas. At the same time, a user will be asked for permission to access the microphone since the final video may contain an audio track as well. If that permission is granted, the application will attach to the main video track of the stream another audio track containing the recording from the computer's microphone. Afterwards, that stream object will be given to the streaming library in order to be sent over the peer connection. The stream is captured at a frame-rate of 30 frames per second, but that can easily be changed in the future by the user in order to accommodate for slower internet connections.



![Screen Shot 2018-05-29 at 21.55.54](assets/Screen Shot 2018-05-29 at 21.55.54.jpg)



In order to initially capture the stream the client has to first find the canvas element on the page and it currently does that by using its ID. Also, another important aspect to note is that the canvas is forced to re-render its content whenever a peer decides to connect. This has to be done since otherwise the newly connected peer would not see anything on the video stream until something modified on the streamer's canvas.

Also, this streaming method does support multiple peers. While it wouldn't be recommended to have thousands of users connected to the same stream, having somewhere around 20 to 30 users should not be a problem depending on the streamer's internet connection. The signalling system is built in such a way that whenever a new user wants to view the stream it will not disrupt the other users' watching experience.

Last but not least, a good reason to use such a wrapper library is that it handles connection closures by itself. This way, when a user disconnects, either because of closing the tab or internet issues, the streamer's connection to it will be automatically closed and it will not put extra stress on the internet connection. [36]



### 4.3.4 Web Graphics



In order for the canvas to actually be useful there have to be tools in place for the streamer to draw on it. For this, the application uses a JavaScript library called 'FabricJS' for actually drawing the items on the canvas. While this library does a great job at drawing, the only interaction it has out of the box is dragging the elements around. The platform has to manually implement the tools for drawing and the way a user would interact with the canvas in order to create the shapes, text and desired images.



![Screen Shot 2018-05-29 at 21.40.00](assets/Screen Shot 2018-05-29 at 21.40.00.jpg)



For different mechanics like deleting an object when the delete key is pressed or for click and hold in order to draw a shape the application attaches a series of event listeners to the canvas object. This way, it can intercept the interactions a user makes with it and convert them to drawn shapes according to the tool selected by the streamer.

Different items require different parameters in order to be created. Another thing to keep in mind is the order of those given parameters. Since you could draw a rectangle from left to right or the other way around, the app also keeps track of the direction of the drawn shapes in order to accurately position them on the canvas object. Another thing that needs an event listener is the color picker. This tool allows a user to select different fill colors for shapes or text colors. While it does use the default color picker embedded into most browsers, it still needs to be able to see the dynamic changes in color a user might make without reselecting the tool. This allows you to draw the same shape multiple times with a different color each time without forcing you to reselect the tool after a color change.

There are multiple tools offered to a user in order to allow him to be creative in drawing the items. This first one is a brush tool which allows you to select the color and thickness. Next up is a selection tool, allowing you to select an item for rearranging it on the canvas or to mark it for deletion. 2 of the current supported shapes are rectangles and circles for which the fill color and, in case of the circle, the stroke width can be specified. Text is a really essential tool that also provides quite a bit of customisation in the form of font size, color and alignment in the drawn container. Also a streamer can create simple straight lines while also specifying the width of the drawn line. The canvas also has support for images, simply inputing the URL of the image being required and that will automatically be added to the canvas. The last 2 items are a deletion tool which works by first selecting the desired item to be deleted and then clicking the appropriate tool or the pre-assigned hotkey and a color picker which controls the color of the element currently being drawn.



### 4.3.5 Server-less Computing



A really important part in the simplicity and in keeping the server costs low is done through server-less computing. Basically, there are 2 main parts of the application that utilise a service provided by Google called 'Firebase Real-Time Database'.

The first of them is peer signalling. In a nutshell, before the peers can start streaming video between each other they need to establish a connection and to notify themselves about their existence. First of all, the viewer has to express its existence. The streamer will automatically detect its presence through firebase's SDK and post an offer. This is automatically picked up by the viewer which validates the offer and sends a response in the same tree hierarchy. Again, the response is automatically signalled to the streamer and it allows the peers to connect and start the video stream. In order for this back-and-forward communication to happen there are some validations that need to be in place in order for the component not to unnecessarily re-render on every change to the node in the database, this way possibly killing some active connections.

Another part of the application that works solely using Firebase is the chat system. This allows any users in a room to interact with each other by using a realtime chat. Once they connect to the room they will be attached to an event listener in the database. This way, any time a node changes because, for example, a viewer added a new comment the client will automatically fetch the remaining data in order to be displayed in real time. This approach eliminates the need  for a standalone socket implementation or for server polling. Also, if a user decides to add a new comment it will trigger the Firebase server to automatically signal all the connected clients. This way a refresh of that specific content will be forced on every other user connected to the stream. [37]

One of the great parts about using Firebase in conjunction with a regular server with a database is that it simplifies the socket process by a lot. Also, it does relieve a lot of network and processing stress from the server making the costs associated to hosting it smaller.



![Screen Shot 2018-05-31 at 16.07.31](assets/Screen Shot 2018-05-31 at 16.07.31.jpg)



### 4.3.6 UI and Front-End



In order to help with creating a really fast web application the client uses a library called 'VueJS' for handling UI navigation and not only. It is based on an MVC like model, meaning that it uses templates to render the content and controllers to keep track of the displayed content. Further more, it is really light-weight, allowing for a page to render quickly and to avoid loading unnecessary resources. It allows for a developer to keep components in a single file or split them apart, however he wants, this way making it easier to keep track of variables across views and JavaScript files. [38]

An important part in rendering the content is Vue's templating system. One of the main features it offers is 2 way data binding. Basically, this allows a user to dynamically change a page's content whenever a variable in the controller modifies, but also automatically modify a controller variable whenever a bound input is modified by the user. This allows for a much less verbose communication method between the 2 main parts of a component than a simpler one way data binding system would offer. Further more, certain template directives allow it for easier rendering of elements like lists. By using an attribute such as 'v-for', Vue is able to render a list of items from the controller in the view without needing to generate the HTML code in the JavaScript file. Also, through a key system, it is able to easily figure out which item from a list has been interacted with.

One place where Vue excels in comparison to other libraries and frameworks is in code organisation. Everything in a Vue file has to be split between data items which are variables that can be seen and modified from the view, separate methods for data processing and handling more complicated interactions like routing logic between components and even processed variables which change their value whenever another variable changes, directly or indirectly, throughout the app. All these methods declared like this are accessible to the client through the view if they are tied to an action on an element.

The way Vue handles page rendering and mounting is pretty similar to any other JavaScript framework, and, just like ReactJS it provide a number of lifecycle hooks of a component in order to give you proper control over the flow of data throughout the component. Some of the most popular lifecycle hooks are created, mounted, updated and destroyed, this app mostly using the mounted hook. Basically, any time you would have to do an HTTP request to the server you will render the UI first without loading the data, afterwards populating it once the data arrives. This will give a user the impression that a website loads faster than if he would have to wait for the data in order for anything to be displayed. Also, there are other initialisations that can be done only after the UI has rendered, increasing the responsiveness of the page.

An important part of the components used are rendered using Vuetify. This is basically a JavaScript and CSS library that provides a suite of Material Design components which make the creation of a design easy and effortless. Other than some pre-styling applied by the library, the greatest advantage of using it is that it also comes with a 12 column grid system for creating complicated layouts. These layouts are easy to adapt to being responsive, this way allowing the user interface to be rendered properly both on the desktop and on mobile or tablet. Below there is an example of styling between standard HTML components and Vuetify components. [39]



![Screen Shot 2018-05-31 at 16.13.52](assets/Screen Shot 2018-05-31 at 16.13.52.jpg)

![Screen Shot 2018-05-31 at 16.14.14](assets/Screen Shot 2018-05-31 at 16.14.14.jpg)



Axios is the HTTP Client of choice since it has multiple advantages over an approach using just the standard fetch API. While the fetch API can basically do any type of request you might need, it does it in a much more verbose way than AXIOS. Also, AXIOS introduces some interceptors for HTTP requests allowing you to modify headers of any incoming or outgoing request. All in all, it provides an easier and more concise way of interacting with the data on a server. [40]

A key part of the application is Firebase as we mentioned before, and since only the client ever gets to interact with the database it needs a complete suite of actions to match. While Firebase does provide an API-like approach giving you HTTP endpoints for all your data, a better way of retrieving and writing data into the real-time database is by using their SDK in the form of a JavaScript library. Also, a crucial feature of this app is being able to automatically listen to events triggered in the database and for that using the SDK is the only real viable option. In order to use the SDK a developer needs to go to the Firebase console and create a new project. After setting up the database access rules one is able to get all the configuration parameters needed in order for the application to establish contact with the cloud hosted database.



![Screen Shot 2018-05-31 at 16.20.02](assets/Screen Shot 2018-05-31 at 16.20.02.jpg)



Routing is a really important but basic part of the web platform. In a way it provides functionality that a user might take as a given, but on the code side of things it handles not only navigation between components but authentication as well. The officially supported router for VueJS is vue-router and that is also the one used here. In order to setup the routes the application uses a matching pattern to match the URL with one of the configured ones and it renders the appropriate component depending on the match. It also supports wildcards, this way, for example the application can redirect any navigation that is unknown like an undefined URL to a certain page like Home. Besides serving these purposes, keeping an unauthenticated user out of places where he shouldn't be is a key responsibility. For this to be possible, at every triggered navigation to a different component, before entering and rendering it the router checks if a user is authenticated before letting him in. In some cases, it might redirect the user to the login page if his JSON Web Token is invalid. This is how it is able to decide which user can access what part of the page and redirect unauthorised ones to public zones of the application.



![Screen Shot 2018-05-31 at 16.28.47](assets/Screen Shot 2018-05-31 at 16.28.47.jpg)



The last essential link in the authentication chain is done through localStorage. The way devise authentication was configured on the server is this: First of all, a user signs in and the back-end server generates a token. For the next request, the client will need to include the previously generated token in the headers of the request. When returning, other than giving back the requested data, the server will also issue a new token which the client will again have to memorise and attach to the next request. This creates a really secure chain of events but adds a little complexity to handling the tokens on the client side. Basically, the token changes and the client has to keep track of the latest issued one. While it could be kept in memory, the problem is that would force the user to login every single time he closes the tab. While it works great from a security standpoint, from a user experience perspective it's really bad. For this, the browser has to store the current token locally. In order to do this it uses an HTML5 API called localStorage which allows him to store it on the client's computer using the browser. Despite being stored on the computer, the JSON string that is being stored can only be accessed from the domain it has been set. This way, a website like 'www.facebook.com' can not access the locally stored items of a website like 'www.vboard.com', helping mitigate session hijacking and other potential security risks.

On the styling part of the UI, while Vuetify handles a lot of the basic work there are always adjustments which will require manually writing some CSS code. While CSS is not inherently bad it can easily be enhanced by using SCSS, a language that is interpreted and converted into standard CSS. One of the things this adds are variables and rule nesting. Variables are an easy way to keep track of things like colors throughout CSS code, this way even making theming easier. On the other hand, rule nesting is a life saver when having to write long CSS rules. Further more, the fact that any CSS syntax is valid SCSS makes it a really easy transition especially for designers, since they would not have to rewrite or necessarily adapt to any new coding style. Here is an example of the differences:



![Screen Shot 2018-05-31 at 16.44.47](assets/Screen Shot 2018-05-31 at 16.44.47.jpg)



The last thing making the development part much more pleasant throughout the application is the use of features from the EcmaScript 6 Standard. Things like constants, arrow functions and async/await make programming for the front-end much more pleasing. For starters, the 'const' keyword allows you to define a variable which cannot be directly modified in code, this way making sure that other unexpected pieces of code do not modify its value. Next up, arrow functions, other then being easier to read and follow, remove the need to bind the 'this' context. In the past, handling this context was a nightmare in JavaScript and you would have to manually do bindings but by using arrow functions the 'this' context is transmitted automatically without any developer intervention. Last but not least is the 'async/await' syntax. What it basically allows is to treat asynchronous code as synchronous code. This way a developer can eliminate most of the promises throughout the app and the so-called 'callback hell' where nesting of multiple promises had to be implemented in order to be able to define a clear order of execution of multiple async functions like API calls. While all calls to the API are made using this syntax, the calls to the Firebase Real-Time Database are still handled with promises since they don't use a standard HTTP architecture, but more of a socket-like interface combined with event listeners. [41]



## 4.4 Validation and Testing



Testing is a crucial part of developing an application since it helps write more bug-free code from the beginning. In order to do this there are some tests implemented for Ruby on Rails. The framework has a gem called 'test_helper' used for writing integration tests.



![Screen Shot 2018-05-31 at 16.53.31](assets/Screen Shot 2018-05-31 at 16.53.31.jpg)



This suite helps a developer test anything from methods to controller actions and request handling. You can automatically simulate HTTP requests towards the API and assess the results of that specific call. Since it comes out of the box with Ruby on Rails it features a great integration with the entire ecosystem.

Another thing that helps manual testing is seed data. Especially during the early stages of development, the database might me rebuilt multiple times and a lot of the times data will be lost while testing. While that is not a problem during this stage of product development, having to constantly manually create new entities can be tedious and time consuming. In order to automate this process, Ruby on Rails provides a way to automatically generate entities in the database by using one file and a command. The file stores the ActiveRecord instruction for seeding like the entities to be added and addition logic. In our case, the seed method checks if there are already existing elements of a certain type in the database before adding new ones in order to not get duplicate names. After this initial check it automatically creates and saves these entities just like it would if those actions would be performed in a controller. This file can only be written once and it can be run at any time using the 'rake db:seed' command.



![Screen Shot 2018-06-10 at 21.15.48](assets/Screen Shot 2018-06-10 at 21.15.48.jpg)



Last but not least, the Rails console is a really valuable addition to the entire ecosystem. Even in the controllers, at the end of the day, every action simply executes some lines of Ruby code and ActiveRecord commands. The console allows you to do this same thing dynamically, sort of like a browser console might allow you this for JavaScript. It is a great way to initially test your database queries and CRUD (create-read-update-delete) operations. You can see the results of your queries instantly in the console and even save things directly into the database. Basically, it creates a simulated environment where it allows the developer to play around in order to find the best and most efficient queries. [42]



## 4.5 Benefits of proposed solution



First of all, the server API is built using technologies that are really fast to design, develop and implement. Ruby on Rails is a really good environment for developing applications quickly without too much hustle. Secondly, the JavaScript library used, VueJS, provides huge amounts of flexibility and speed. Its flagship benefit is rendering only the parts it needs in a single page application form. Firebase allows easy communication using sockets. The canvas and WebRTC protocols allow for a cross-platform implementation that works in all browsers from desktop to mobile. In turn, this facilitates all types of users to watch video streams.

The benefits have a wide range from development time which directly impacts the development costs of any project, reduced server fees and improved quality and latency for the streams. Another key aspect in the chosen technologies is that they are implemented with an agile environment in mind, allowing ideas to pivot easily and quickly adapt to new technology trends.

The Agile methodology in software development describes a process for building a product in which everything evolves over time, from requirements to the code base itself. This allows a company to quickly adapt to market standards and new technologies while also tailoring to the client's needs. In the past the main methodology used to be Waterfall. What that did was taking a step by step approach to developing a platform. At first you had to create the requirements, afterwards a design, later develop the application and finally test is and go to market. The issue with this is that the process could go on for so long that the market might not require the software by the time of launch. Also, a lot of team members would have to simply sit around waiting for others to finish their job since every aspect was closely related to another one. What Agile does it allows everyone to work at the same time on small chunks of the final puzzle. By doing this is maximises the employees' time while also producing better customer satisfaction.

Since the methodology changed from Waterfall to Agile, the tools that were used also had to adapt. This is where Ruby on Rails and VueJS mainly excel. The pure nature and rapidness of the Rails framework allow it to change main logic really easily. Also, the component based approach of VueJS makes it easy to replace and communicate between smaller portions of a web page.



## 4.6 Personal Contribution



This sub-chapter has the purpose of detailing the personal contribution added to the project. We will start with the back-end side of the project and finish with the UI of the application.

First of all we will address the backend server. While the base structure of the project was generated using the Rails command line tools I personally had to create the structure of the entities, models and validations together with the Database queries. All those parts had to be joined in the controllers and create the entire back-end logic from scratch. Not only that but manually configuring the project from Cross-Origin Resource Sharing to the security of the platform had to be implemented from scratch. Next, as fas as the Firebase storage goes I had to create a project and figure out the structure of that Database and how it could relate to the Rails database. Especially in NoSQL databases, having a well defined structure can help a lot in future development and in avoiding critical bugs. Also, while the CRUD(Create-Read-Update-Delete) operations on the firebase server do not have to be implemented by hand, the interaction with it form the client does. This way every event listener for changes and modification logic has to be implemented from scratch. A good example of this is the Peer signalling where clients have to listen for certain specific events in a certain order for them to properly establish a valid communication channel using WebRTC Peer-to-Peer video streaming.

Speaking of WebRTC Video Streaming, I had to implement the streaming logic and the way peers connect to each other. Getting the stream is an easy task, the HTML5 API providing an a quick and painless way to get such a media object. That object had to be manipulated in order to support additional audio tracks for having the auditive part as well.

We come to the graphics part, where every single tool and interaction had to be implemented form zero. Every event listener and mouse handling logic had to be created separately. Further more, handling the parameters for the shape drawing library is done manually since they might change without the used tool actually changing.

This is all implemented as a part of the UI made in VueJS. For this, the canvas part is a self-sustaining component in itself. This way it allows for easy modifications down the line where if other technologies or implementations appear they could easily swap the current approach by simply modifying a Vue file. Other elements also share this component-based approach like the chat being its own standalone component. The client is set up as a Single Page Application and in order for a good user experience I had to implement routing between components. When doing this I always had to keep in mind what components are publicly accessible and which require a certain authentication state. Last but not least, all the rendering and logic together with UI interactions were implemented manually in VueJS.

While the base design of the components are done using Vuetify, the layout and structuring of the HTML page was done without the use of a pre-determined template.

In a nutshell, while the platform does use plenty of libraries and gems as wrappers over lower level technologies they all still require plenty of alterations and coding in order to get them all to work together.



# 5. Conclusion



This chapter goes through a brief conclusion on how the problem was solved using the above technologies and approaches while also presenting some future possibilities of expansion to this entire ecosystem.



## 5.1 Drawing the line



First of all, the problem of platform compatibility is solved by using a web only approach. Everything is done through a web application that requires no extra plug-ins or extensions. While the Peer-to-Peer video streaming solution is a new web standard that hasn't yet been adopted by all browsers, the major ones like Google Chrome and Mozilla Firefox do have it working reliably. Since every interaction is based on the HTML5 standard, basically any device, be it desktop, laptop, phone or tablet could theoretically make use of the application. While drawing on a phone might not offer an optimal user experience, any browser that has support for HTML video should be able to display an incoming stream. In the future the platform would allow for integrations with 3D rendering engines and mobile platform tools such as React-Native. This way, the drawing system can be optimally implemented on each platform while the streaming of the content staying the same across all types of devices.

Further more, another problem this application solves is server cost. The thing is, the Rails server does not need a lot of data in order to function properly, so server costs are low. Also, Firebase might become a cost if the user base grows but again, not much data needs to be stored there so that might happen only over time and incrementally. The big problem that is solved is the server cost associated to streaming. Up until recently, the only way to do streaming of video was through an intermediary server. With the peer-to-peer approach we can basically eliminate it entirely, reducing these costs to 0. Even as far as a user data consumption is concerned, the WebRTC standard offers great compression and is really well optimised for video streaming so it should really not consume much data. At the end of the day, all these new technologies and standards could help a company keep the cost low and by this automatically lowering the price of the platform for the end user. While server storage is really cheap, renting out or building scalable servers for computational power isn't. This way if any sort of processing can be moved on a client the business would benefit in terms of cost lowering. [43]

Last but not least, libraries like VueJS allow for easy maintainability and code swapping. If one part of the product becomes deprecated or better alternatives exist, the only required modifications are to the specific components encapsulating a particular feature. Code reusability is also encouraged by using high-order and dumb components, allowing for example to use the exact same chat component both in the streamer's view and in the watcher's view without the need to write it multiple times. [44]



## 5.2 Future directions



Given the nature of the technology stack being used the product suits very well for cross-platform applications. This way, while it currently functions great in Web Browsers it would be an easy idea to implement as a native application for Android or IOS using React-Native or even as a Desktop application for MacOS, Windows or Linux by using a framework such as ElectronJS. Since the server API would essentially be the same and the communication protocol being established the only requirement would be building the User Interface in for any of these applications. By keeping technologies as open and possible the platform can benefit from great community support and also, at the end of the day, with only 3 client code bases it can target over 6 platforms ( Browsers, Android, IOS, MacOS, Linux, Windows ) making the adaptability of the product really high.



# Bibliography





[1]: https://www.apple.com/hotnews/thoughts-on-flash/	"Thoughts on Flash - Steve Jobs, 2010"
[2]: https://theblog.adobe.com/adobe-flash-update/?origref=https%3A%2F%2Fwww.androidpolice.com%2F2017%2F07%2F25%2Fchrome-will-completely-remove-flash-support-end-2020%2F	"Flash & The Future of Interactive Content"
[3]: https://gigaom.com/2014/03/21/what-if-netflix-switched-to-p2p-for-video-streaming/	"What if Netflix switched to P2P for video streaming? - Janko Roettgers, 21.03.2014"
[4]: https://blog.red5pro.com/rtmp-webrtc-which-protocol-live-streaming-app/	"WebRTC vs. RTMP – Which Protocol Should You Choose for Your Live Streaming App? - Red5Pro, 17.03.2017"
[5]: https://skylink.io/webrtc/	"WHAT IS WEBRTC? - Syslink"
[6]: https://themindstudios.com/blog/how-much-does-it-cost-to-build-a-streaming-website-like-twitch-tv/	"How Much Does It Cost to Build a Streaming Website Like Twitch.tv? - Elina Bessarabova, 20.01.2017"
[7]: https://ieeexplore.ieee.org/document/7946410/	"Edge computing for interactive media and video streaming - Kashif Bilal/Aiman Erbad, 15.06.2017"
[8]: https://www.w3schools.com/tags/ref_canvas.asp	"HTML Canvas Reference - w3schools.com"
[9]: http://guides.rubyonrails.org/	"Ruby on Rails Guides"
[10]: https://medium.com/@RubyGarage/what-are-the-benefits-of-using-ruby-on-rails-for-your-startup-f3d6f6b9938d	"What are the Benefits of Using Ruby on Rails for Your Startup - Ruby Garage, 05.05.2017"
[11]: https://medium.freecodecamp.org/understanding-the-basics-of-ruby-on-rails-http-mvc-and-routes-359b8d809c7a	"Understanding the basics of Ruby on Rails: HTTP, MVC, and Routes - TK, 02.01.2018"
[12]: https://rubyonrails.org/doctrine/#convention-over-configuration	"The Rails Doctrine - David Heinemeier Hansson, January 2016"
[13]: https://medium.com/@mazik.wyry/rails-5-api-jwt-setup-in-minutes-using-devise-71670fd4ed03	"Rails 5 API + JWT setup in minutes (using Devise) - Adam Mazur, 14.02.2018"
[14]: https://jbuilder.readthedocs.io/en/latest/	"JBuilder Documentation"
[15]: https://richonrails.com/articles/getting-started-with-jbuilder	"Getting Started with JBuilder - Richonrails, 03.10.2013"
[16]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS	"Cross-Origin Resource Sharing (CORS) - MDN Web Docs"
[17]: https://www.sqlite.org/whentouse.html	"Appropriate Uses For SQLite"
[18]: https://medium.com/@thejasbabu/activerecord-the-heart-of-rails-fe977d1d4125	"ActiveRecord: The Heart of Rails - Thejas Babu, 11.04.2016"
[19]: http://api.rubyonrails.org/classes/ActiveRecord/Migration.html	"Active Record Migrations - Ruby on Rails Documentation"
[20]: http://www.streamingmedia.com/Articles/Editorial/Featured-Articles/The-State-of-WebRTC-and-Streaming-Media-2018-124068.aspx	"The State of WebRTC and Streaming Media 2018 - Tsahi Levent-Levi"
[21]: https://github.com/feross/simple-peer	"Simple Peer"
[22]: https://medium.com/@AmyScript/introduction-to-html5-canvas-8c1bad20e855	"Introduction to HTML5 Canvas - Amy, 27.03.2017"
[23]: http://fabricjs.com/docs/	"FabricJS Documentation"
[24]: https://www.w3schools.com/js/js_events.asp	"JavaScript Events"
[25]: https://developers.google.com/web/updates/2016/10/capture-stream	"Capture a MediaStream From a Canvas, Video or Audio Element - Sam Dutton"
[26]: https://medium.com/@limhenry/why-i-love-firebase%EF%B8%8F-and-what-is-firebase-%EF%B8%8F-96b54509d450	"Why I love Firebase (and what is Firebase) - Henry Lim, 15.08.2017"
[27]: https://firebase.google.com/docs/database/	"Firebase Realtime Database - Firebase Documentation"
[28]: https://medium.com/unicorn-supplies/angular-vs-react-vs-vue-a-2017-comparison-c5c52d620176	"Angular vs. React vs. Vue: A 2017 comparison - Jens Neuhaus, 28.08.2017"
[29]: https://medium.com/pilcro/should-you-use-material-design-bfb596a04bae	"Should you use Material Design? - William Woodhead, 11.04.2018"
[30]: https://github.com/axios/axios	"Axios Documentation"
[31]: https://medium.com/vandium-software/5-easy-steps-to-understanding-json-web-tokens-jwt-1164c0adfcec	"5 Easy Steps to Understanding JSON Web Tokens (JWT) - Mikey Stecky-Efantis, 16.05.2016"
[32]: http://sass-lang.com/documentation/file.SASS_REFERENCE.html	"SASS Documentation"
[33]: https://alistapart.com/article/why-sass	"Why Sass? - Dan Cederholm, 13.11.2013"
[34]: http://jonkruger.com/blog/2010/10/21/ruby-on-rails-vs-net-the-active-record-pattern-is-it-ok/	"Ruby on Rails vs. NET: The Active Record Pattern, is it OK? - Jon Kruger, 21.10.2011"
[35]: http://guides.rubyonrails.org/security.html	"Securing Rails Application - Ruby on Rails Documentation"
[36]: https://bloggeek.me/webrtc-rtcpeerconnection-one-per-stream/	"WebRTC RTCPeerConnection. One to rule them all, or one per stream? - Tsahi Levent-Levi, 09.01.2017"
[37]: https://medium.com/react-native-development/build-a-chat-app-with-firebase-and-redux-part-1-8a2197fb0f88	"Build a chat app with Firebase and Redux, part 1 - Shoutem, 30.03.2017"
[38]: https://vuejs.org/v2/guide/index.html	"VueJS Documentation"
[39]: https://material.io/design/	"Material Design Documentation"
[40]: https://firebase.google.com/docs/web/setup	"Add Firebase to your JavaScript Project"
[41]: http://es6-features.org/#Constants	"EcmaScript6 Constants Documentation"
[42]: https://medium.com/@apneadiving/metaprogramming-debugging-in-ruby-13c3a5a80667	"Debug with Metaprogramming in Ruby - Benjamin Roth, 10.04.2018"
[43]: https://hackernoon.com/cloud-costs-arent-actually-dropping-dramatically-cd94051b021c	"Cloud costs aren’t actually dropping dramatically - Eric Lu, 06.01.2018"
[44]: https://codeburst.io/creating-reusable-components-with-vue-js-button-component-503167facfde	"Creating Reusable Components with Vue.js : Button Component - Casey Morris, 06.04.2017"