---
title: "Coursera's Data Deluge (and what it tells us about the behavior of online learners)"
layout: post
date: "2015-04-17T11:20:00"
categories: ["analysis"]
tags: ["MOOCs", "Coursera", "Learning analytics"]
blog: true
description: Each one of Leiden University's online courses generates a wealth of behavioral data by which we can evaluate teaching methods, improve on-campus education, and offer a platform for academic research to investigate learning behavior of students with different educational and cultural backgrounds.
---

<em>This post was first published on the website of the Leiden Center for Innovation <a href="http://leidenuniv.onlinelearninglab.org/blog/-courseras-data-deluge" target="_blank">Online Learning Lab</a>.</em>

Leiden University runs a variety of different Massive Open Online Courses (MOOCs). Over the past two years, it has produced 11 iterations of 6 unique MOOCs, and further moved Terrorism and Counterterrorism: Comparing Theory and Practice (as one of an initial 73 courses) to Coursera’s new on-demand platform. Each one of these courses generates a wealth of behavioral data by which we can evaluate teaching methods, improve on-campus education, and offer a platform for academic research to investigate learning behavior of students with different educational and cultural backgrounds.

The Online Learning Lab, which is part of the Leiden University Centre for Innovation at Campus The Hague, is continuously exploring methods by which research into learning and the integration of online and on-campus education can be better facilitated. Recently, we started exploring the wealth of data made available to us via Coursera. For researchers and MOOC organizers, this data offers an exciting way to explore the behavioral patterns of online learners.

Previously, our understanding of MOOC learners was limited to answers to pre-course and post-course surveys and the analytics delivered by Coursera in the course overview. Now that we have access to the databases of user interactions with Leiden University MOOCs, we would like to provide access to researchers at Leiden University such that we can stimulate research in the area of online education and tighten the integration between MOOCs and on-campus courses.

<strong>A look at Leiden University MOOC data</strong>

Coursera supplies us with a variety of data, ranging from user posts on forum threads, student grades, user interactions with videos and rich user metadata. This data can be geared to explore intriguing questions about our learners. For example, figure 1 shows the percentage of new and returning learners for each completed Leiden MOOC to date.

<div>
  <img src="https://drive.google.com/uc?id=0BwFSCwDYNUS9c2U3WmNyXzJ1YjQ">
  <figcaption class="caption">Figure 1: Percentage of new and returning students. The MOOCs on the x-axis are structured chronologically. A ‘returning student’ is defined as a student who has previously signed up for a Leiden University MOOC.</figcaption>
</div>

As we can see, returning learners make up a sizeable minority in Leiden MOOCs. At the very least, this means that there exists a pool of students who are familiar with Leiden University and who return to participate in new courses. Similarly, we see that new iterations of existing courses contain a high percentage of returning learners. Figure 2 shows the percentage of returning learners over the first three iterations of the MOOC <em>Terrorism and Counterterrorism: Comparing Theory and Practice</em>.

<div>
  <img src="https://drive.google.com/uc?id=0BwFSCwDYNUS9eWhJMExrNmE0N0U">
  <figcaption class="caption">Figure 2: Percentage of new and returning students in the first three iterations the Leiden University MOOC ‘Terrorism and Counterterrorism: Comparing Theory and Practice’.</figcaption>
</div>

Concretely, this indicates that roughly 20% of learners who participated in the second and third iteration of the course had already participated in an earlier iteration of the same MOOC. These learners likely return to view extra content that was previously not available to them, or because they were simply not able to finish the course the first time they participated. Whatever their reasons, it paints an interesting picture of learner engagement with our online content. For these learners, content is not linear; instead, they engage with it at their own pace and according to their needs and interests. 

Figure 3 below shows several user interactions of users with the forum of the course <em>Wheels of Metals: Urban Mining for a Circular Economy.</em>

<div>
  <img src="https://drive.google.com/uc?id=0BwFSCwDYNUS9b1hRUmJRLS1NUTQ">
  <figcaption class="caption">Figure 3: Passive forum interactions over the course of the MOOC ‘Wheels of Metal: Urban Mining for a Circular Economy’.</figcaption>
</div>

Other than the seasonality of interactions (e.g. the forum interactions spike at the beginning and the end of weeks), this figure indicates that the number of forum interactions is relatively low compared to the number of MOOC participants, which roughly comprises 4000 active learners and 6800 signups. We further observe that forum and thread views (which are relatively passive user actions) are by far the most used interaction throughout the course, and that user interactions are highest at the beginning of the course and after the course has ended.[^1]

Similarly, the data provides insight in the interaction of students with our MOOC videos. Figure 3 below shows the interactions of 100 learners with the video buttons of one of the videos in the Wheels of Metals: Urban Mining for a Circular Economy course.

<div>
  <img src="https://drive.google.com/uc?id=0BwFSCwDYNUS9ZVpDTzlvZWZKc0E">
  <figcaption class="caption">Figure 4: Bipartite graph of 100 users interacting with a video from the ‘Wheels of Metal: Urban Mining for a Circular Economy’ MOOC. Learners are depicted at the bottom of the graph (yellow rectangles). Video actions are depicted at the top of the graph (purple rectangles). The lines connect the actions users take with the specific video action. The size of each interaction, user, and video action is determined by the amount (i.e. count) of user interactions.</figcaption>
</div>

Learners seem uninterested in the use of the ‘stop’ button in videos. This makes sense, as learners can simply close the tab in which the video is playing. Figure 3 further indicates that the ‘seek’ button, which allows users to navigate through the video, is the most widely used user action, and that (somewhat counterintuitively) the ‘play’ button is pressed more often that the ‘pause’ button.

When we plot these interactions over the course of the video (figure 4), we further see that certain user certains (e.g. ‘pause’ and ‘play’) are primarily used at the start and the end of videos. Conversely, users use the ‘seek’ button at a constant rate throughout the video. Of course, this differs from video to video. We might further expect to see that seeking increases at moments where the video pauses to ask the viewer a question. 

<div>
  <img src="https://drive.google.com/uc?id=0BwFSCwDYNUS9bkw5eHVGYl9ybHc">
  <figcaption class="caption">Figure 5: Distribution of user interactions over the course of a video from the ‘Wheels of Metal: Urban Mining in a Circular Economy’ MOOC.</figcaption>
</div>

These figures are a small selection of what we can do with the Coursera data, and they raise a lot of questions suitable for research projects, especially with regards to the behavioral aspects of user interaction with our content. For example, we might want to investigate when users start seeking in a video, and whether they are triggered to do so when faced with difficult material. With respect to the forum, we could ask when and why user engagement peaks, and whether the content of user posts is less positive and more focused on questions about quizzes and videos rather than discussion about course concepts. 

<strong>What’s Next?</strong>

The Leiden University MOOC data provided to us by Coursera offers us exciting avenues to better understand the behavior of our online learners. Additionally, we are gearing up to implement possibilities for experimentation with course content. Such experiments focus on the flexible adaptations of, for example, video length and question formats over time to register critical differences, and further include experimentation in fusing online and on-campus education with the aim of creating more flexible course structures that focus on the specific learning style of the students. These tools open up exciting new possibilities to gain insights in the learning behavior of thousands of learners, and will no doubt serve to better the educational experiences of our on-campus students.

<h1>Notes</h1>

[^1]: This may seem counterintuitive. A preliminary analysis indicates that learners return to the course forums at the end of the course to post questions with regards to their final grades and when they will receive their certificates.