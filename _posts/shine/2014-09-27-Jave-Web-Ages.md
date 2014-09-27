---
layout: post
title: How we got to today's Java web application options
category: shine
description: Based on <Starting Struts2>, I would like to review the Java web development ages and make you get insights of how history goes in this field.
published: true
---

Today's Web Pages are quickly response, separated in modularity and functional as desktop software. If you visit large website as Taobao.com, over hundreds requests will be sent to render the current page, it is definitely a contribute of modularity and Ajax technology. Now let's take a brief view of how we got to today's Java web application options.

## Servlets
Servlets is a server site application who provide a way to map a URL to a special class whose methods would be called. But generating the HTML code from within the Java code was a maintenance nightmare. Each time a simple user interface change was needed, the Java developer should modify the Servlet code, recompile the source and then deploy the application into the server environment. It makes that difficult to iterated development.

## JSP and Scriptlet Development
JSP(Java Server Pages) is an upside-down method of former Servlets. Rather than placing the HTML code within the Servlet or Java code, the Java code was placed (as script-lets) inside the HTML code - as Java Server Pages. Each JSP provide both the logic for processing of requests, and the presentation logic.

One problem solved, but another was introduced. Looking through early JSP files, you would find Cut-and-Pasted code can be seen in plenty of pages as they need similar processing logic. Propagating any defects or errors from the original code, and increasing the amount of work required to make a common change.

From these findings, a best practice as well as a pattern emerged - use Java objects from JSPs.

As the JSP specification evolved, tags were introduced to encapsulate re-usable Java objects. Tags provided a HTML-like facade for accessing the underlying code, allowing designer and IDEs to interact with dynamic elements to compose page layout. Examples of the tags provided by JSP are *<jsp:userBean ... />* and *<jsp:getProperty ... />*.

![model-view-controller](http://findshine.qiniudn.com/servlet.gif "model-view-controller")

## Action-Based Frameworks
Action-Based frameworks came onto the scene to **combine the concepts of servlets and JSPs**.
The idea being to split the request processing for the page the user sees into processing logic and the presentation logic, letting each part do what it does the best. The implementation is known as the *model-view-controller* pattern.

In this pattern, the servlet is the *controller*, providing a centralized point of control for all client page requests. It maps the request URL to a unit of work know as an *action*. The action's job was to perform specific functionality for a given URL by accessing the HTTP session, HTTP request and form parameter, calling business services, and then mapping the response into a *model*, whose form is a plain old java object. Finally, the action returned a result, which was mapped to a JSP to render as the *view*.

## Component-Based Frameworks
As web application become more complex, it was realized that a page was no longer the logical separation - web applications had multiple forms per page, links for content updates and many other custom widgets - all which needed processing logic to perform their tasks.

Web development became event-driven. A component could be a HTML input filed, a HTML form or custom widgets. Events, such as form submits or links, are mapped to methods of class representing the component, or to special listener classes. 

## Ajax
As component-based frameworks evolving, Ajax(Asynchronous JavaScript and XML) become commonly used. First amazing application of Ajax is Google Maps. The web page had come alive - you could interact with controls and widgets by using a mouse you could scroll maps around the screen and do not refresh the page.

User interface with Ajax functionality allows the web browser to make requests to the server for smaller amounts of information, and only when it is needed. Only the sections of the pages that changed are re-rendered, not the entire page, making the user feed that the web application is more responsive to their actions.

Actions can provide JSON, XML or HTML fragment views for the Ajax components as well as being combined with other actions to provided HTML views for non-Ajax user interface. JavaScript now is a popular language in web development for every Web engineer to master.