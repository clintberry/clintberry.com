---
title: Engineering Compensation - The Pursuit of Fairness
author: Clint Berry
layout: post
date: 2019-02-03 
url: /engineering-compensation-fair/
tags:
  - Culture
---

In the product development department at Weave, our motto is **Never Satisfied**. We are constantly trying to improve how we work, including experimenting with changes to department processes and organization as a whole.

One of the ways we have improved over the last couple of years is how we determine engineering compensation. Historically it has been hard to determine pay structure and ultimately those who could negotiate best had an advantage. Some of our best engineers are more introverted than the average person and maybe aren't the best at negotiating. So the VP of Engineering and I set out to find a way that would be more fair and accurate for all engineers at Weave. Ultimately, we wanted a compensation algorithm. After two years of research, planning, and debating, we have arrived at our algorithm:

```(skill-level) * (scope-level) * (career add-ons) * (experience-modifiers) = salary```

Read on to see how we determine each of these variables.

## Applying Kahn Academy Method for Skill and Scope

One of our employees pointed us to the [Kahn Academy Engineering Career Development][1] document that they have so graciously shared with the world-- and we absolutely loved it. We immediately set out to see how we could implement it in our engineering department. You should definitely take the time to read the whole document, but here is a summary of how Kahn Academy determines compensation:

* They have a goal to be: Fair, Understood, Transparent, and Competitive; things we all should be aiming for.
* They have three categories that, when combined, determine salary: Skill, Scope, and Experience.
* Skill and Scope each have five levels of achievement as can be seen in the chart below. (see the document for more details on what each level entails). There are two modifiers placed on each level to add more fine-grained improvement: _approaching_ and _exceeding_.
![Skill & Scope Levels][skill-and-skope]
* Levels are attained by meeting specific requirements in these categories: technical, communication, planning, execution, and maturity. Requirements are framed as characteristics that you meet. For example, under the technical section of the More Skillful level, you must exhibit this characteristic (among others): 
  _Code review feedback is sought after, respected, and often the source of others' learning_.
* Levels are attained when the engineer has shown they have fully embodied the characteristics over a period of time. One example of exhibiting a characteristic does not mean you have achieved it.

Let's look at a simple example to illustrate how this could work:

Sandra has shown that she has fully embodied all the characteristics of the More Skillful level and the Contributor level. She has also shown that she has embodied 50% of the characteristics of the Super Skillful level. This places her skill level at Approaching Super Skillful and her scope level at Contributor. Let's say we have the Approaching Super Skillful salary amount at $105,000 and Contributor amount at $20,000, then Sandra's salary is $125,000.

## Career & Bonus Add-ons

The last example with Sandra is very simple, and the real world is often not that simple. We have some engineers who have specialized skills that demand more money. For example, familiarity with open source VoIP technologies like FreeSWITCH and Kamailio are rarer and highly valued. Also, mobile developers often make more than average. To compensate for these types of situations we implemented something called Add-ons.

We first saw the concept of add-ons at Spotify, [where they also use them for career development][2]. Spotify listed out some of their example add-ons: Writer, presenter, and mentor. We loved that but also thought we could apply this same method to our engineers with skill sets that we really valued and typically required higher pay. After lots of thought and debate, we settled on the following structure:

There two types of add-ons: Bonus Add-ons and Career Add-ons. Bonus add-ons are achieved on a yearly basis and end with a year-end bonus, while career add-ons increase your salary overall but could take several years to achieve. 

Our first two bonus add-ons are **writer** and **presenter**. If an engineer writes a blog post for our [engineering blog][3] they will get a bonus, and if they write 4 for the year they get an extra bonus. Same goes for presenting at meetups and conferences. This gives some of our more senior engineers an opportunity to evangelize more and have something extra to work towards.

Our first career add-on is **Beginning VoIP** engineer. To achieve this, an engineer has to take [SIP school][4] and pass the test. If they do, they get a permanent 2% bump to their salary.

We will be adding more career and bonus add-ons over the next few months.

## Experience Modifiers

The last part of our algorithm is our experience modifiers. We feel that experience brings its own value. Even in situations where one engineer might be at a higher skill and scope level, an engineer with more experience often brings insight that the former wasn't aware of or hasn't encountered yet. So we decided to add a modifier for years of professional experience. It is a small bump (0.3% per year of experience) but can show an engineer that we value their experience.

We also added a modifier for how many years an engineer has been at Weave. Each year an engineer has been at Weave they gain all sorts of domain knowledge that makes them more valuable to Weave than perhaps to other companies. We assigned a bigger percentage bump for this modifier, putting it at 2% per year.

## Results so Far

So far it has been a good experience. At the very least the team sees the desire for fairness
and gives good feedback to make the process better. As with everything at Weave, this is a snapshot in time and we will continue to refine and improve this going forward. One of the best parts of this is that managers have really specific career path guidance they can give to their direct reports on how to increase their skill and scope. 

## Next Steps

While I love the direction we have gone with this, it is mostly tailored around the individual contributor. We need to come up with a better and more transparent way of evaluating managers, hopefully following these same methods.
Also, the evaluations for skill and scope are for engineers and not anyone in the product department (product owners, UI/UX, data analysts), so the product team is working on their own version of the skill and scope questions.

 [1]: https://docs.google.com/document/d/1qr0d05X5-AsyDYqKRCfgGGcWSshTMd_vfTggfhDpbls/edit#
 [2]: https://labs.spotify.com/2013/11/21/personal-development-at-spotify/
 [3]: https://medium.com/weave-lab
 [4]: https://www.thesipschool.com/
 [skill-and-skope]: /images/skill-and-scope.png

