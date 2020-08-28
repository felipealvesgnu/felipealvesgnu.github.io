---
author: felipealvesgnu
comments: true
date: 2016-11-23 14:05:04+00:00
slug: intellij-application-context-configuration
title: IntelliJ - Application context configuration
categories:
- IntelliJ
- Java
---

After2 spending some time trying to discover why my JSF application worked on eclipse and didn't on IntelliJ, I found a TomCat setup configuration at **Run/Debug Configurations.**

And it was like in that picture bellow:

<figure>
    <a href="/assets/images/intelliJ/pic_01_context.png"><img src="/assets/images/intelliJ/pic_01_context.png"></a>
</figure>



Without notice I was getting http status 500 error message as the pic bellow after the deployment:

<figure>
    <a href="/assets/images/intelliJ/pic_02_context.png"><img src="/assets/images/intelliJ/pic_02_context.png"></a>
</figure>


### **Solution**


After search a little I found what was missing on that configuration.

The context root of a web application determines which URLs Tomcat will delegate to your web application.

In **Application context** field, you need to put the context path for your project. Generally is recommended that you specify the same context path that you chose to the Maven project. The context path can be found in the root POM file of your project as a <_context-root_> tag.

<figure>
    <a href="/assets/images/intelliJ/pic_03_context.png"><img src="/assets/images/intelliJ/pic_03_context.png"></a>
</figure>



So the main point here is: Be aware of the application context, which defaults to “/” This means your application when deployed to the server would be available at “http://localhost:8080**//**” if you want it to be deployed to another path, you need to update the value in the **Application Context** field. As in my case I put the same WAR application name.



After that all the things are working and Mojarra found the root context as the image bellow:

<figure>
    <a href="/assets/images/intelliJ/pic_04_context.png"><img src="/assets/images/intelliJ/pic_04_context.png"></a>
</figure>





That's All.