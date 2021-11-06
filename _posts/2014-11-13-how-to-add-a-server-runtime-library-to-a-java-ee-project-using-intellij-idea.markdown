---
author: felipealvesgnu
comments: true
date: 2014-11-13 15:23:49+00:00
slug: how-to-add-a-server-runtime-library-to-a-java-ee-project-using-intellij-idea
title: How to add a Server Runtime Library to a Java EE project using IntelliJ IDEA
categories:
- Java
---

After I have some difficult to add server libraries to the project, because in IntelliJ is a lit bit different from Eclipse, I’ve decided to post the solution:

After the project has been opened you will need go in **File** -> **Project Structure**.

<figure style="width: 300px" class="align-center">
    <a href="/assets/images/intelliJ/pic_01.png"><img src="/assets/images/intelliJ/pic_01.png"></a>
</figure>

After that you’ll see the structure of project. In this way, where you need to add a dependency. In the left side column you’ll choose **modules** in Project Settings, choose **Dependencies tab(1),** to click on **+** in the bottom of window and select **Library… (2)**, like below.

<figure>
    <a href="/assets/images/intelliJ/pic_02.png"><img src="/assets/images/intelliJ/pic_02.png"></a>
</figure>

When you click on **Library…** it will open a window asking you to select the server libraries.
IntelliJ will show only the servers that **you have added** before.

<figure>
    <a href="/assets/images/intelliJ/pic_03.png"><img src="/assets/images/intelliJ/pic_03.png"></a>
</figure>

<figure style="width: 300px" class="align-right">
    <a href="/assets/images/intelliJ/pic_04.png"><img src="/assets/images/intelliJ/pic_04.png"></a>
</figure>

After all of this steps your project will rebuild the project and problems related with compile-time should be solved.

Finally your project will be this way.

Any problems that you have, ask me.
