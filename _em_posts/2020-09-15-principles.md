---
title: Principles
date: 2020-09-15 13:00:46 Z
categories:
- Postman
- Engineering Management
version: 1
layout: signals
comments: true
---

These are essential principles for being a better engineering manager and setting up better engineering management processes. Most of these principles are from the books that I have mentioned in the introductory post. I deserve very little credit for this.

Some of these principles are for Engineering Managers, and some are for Engineering Management processes/tools. But all of these principles are worth going through for an EM.

## 🖖 Consistency

-   Everything that we, as managers, do has to be consistent.
    Any strategy/policy becomes effective when applied for sufficiently long periods.
-   So pick the tools you are comfortable with, and implement if you pledge to use them consistently.

## ♻️ Iterative

-   Change is the only constant 😌. And every process needs changes and iterations to make it perfect.
-   Keep these processes open for iterations. Do not lock on something unless you can prove it works. Also, do not engage the entire team from the inception of the processes.
-   Have [strong opinions loosely held!](https://medium.com/@ameet/strong-opinions-weakly-held-a-framework-for-thinking-6530d417e364) You will find many counter-arguments for this philosophy, but this is highly important to keep your approach iterative.
-   Don't add exceptions, count the exceptions, and update the process if an exception repeatedly occurs over a considerable amount of time.

## 🤖 Automated

- Engineers hate redundancy. And they should!
- Ensure that the processes will leverage advanced APIs, ML, and NLP systems to remove the friction.
- Long threads of daily standups notes. Everyone from a team pushes what they have worked on, what they will do today and blockers.
- These are plain text, without forced formatting on the messages. So it depends on individuals to make it concisely insightful
- Team members (not all team members but most of them) have very little insightful information while pushing these updates
<div class="info-box" style="margin-left:20px;margin-top:0px;" role="alert">
<h3 class="title">🤔 How can we improve this?</h3>
<hr style="margin-top:10px;margin-bottom:10px"/>
<div class="content">
<span>✅ They should be able to see what they had written yesterday in a checklist format to verify what they accomplished yesterday.</span><br/>
<span>✅ They should be able to see all the meetings/interviews they have to participate in and different PR reviews pending.</span><br/>
<span>🏆 This information will help team members to push data, which is realistic.</span>
</div>
</div>

-   Engineering managers keep on losing track as this information is not easily queryable.
<div class="info-box" style="margin-left:20px;margin-top:0px;" role="alert">
<h3 class="title">🤔 How can we improve this?</h3>
<hr style="margin-top:10px;margin-bottom:10px"/>
<div class="content">
<span>✅ Parse these standup messages and automatically updated on a dashboard complemented with advanced filtering would be a big help..</span>
</div>
</div>    
- Hence results in repeated information pulling, more status sync-up meetings, which wastes everyone’s time.
- Robust and habit forming management flows are built with **Trigger ➡️ Action ➡️ Variable Rewards ➡️ Investment**.
  - 📣 Timely triggers are crucial for an automated system.
    - Example Sending a Slack message with an action button to push the Standup update every morning at 10:30 am (local time of the team member).
    - Worst case, send an email 😃(I ❤️Slack)
  - 🥊 Action happens out of Intention. But this intention comes with timely triggers.
  - 💰 Rewards are in terms insightful information, higher visibility, automated sync-ups, less distraction for team members.
  - 👩‍💻 Investment is pushing information into these system to keep on getting rewards.

## 🥊 Actionable
- The outcome of each engineering management process should be actionable with little or no manual efforts.
  - For the standup flow mentioned above, each standup message processing should give information about
    - 🏃‍♂️Progress of the project
    - 🛑Blockers that an EM needs to look into on high priority.
- Each actionable output should be converted into a metric and tracked throughout the lifetime of the action.
  - Based on the overall progress and blockers list, the system should give an overall health score based on some pre-defined weighting formula.
- If it’s too low, the project needs some special attention. 🥋

## ⭕️ Complete the loop
- Most of the processes adopted by teams do not complete the loop. So no one can learn from these processes and keep on repeating almost the same mistakes in different forms.
- Completing the loop is the job of an engineering manager. And one of the trickiest and tedious one.
- For standups, making sure all the blockers are solved, digging deeper if the health score is low are the examples of completing the loop.

## 📣 Precisely over-communicate
- Firmly establish the directions of the culture code/expectations from the team members. And keep on repeating it, even if you think you have done it well in the first go.
- Sometimes these conversations become unpleasant, repetitive, and impertinent. But its all worth it.
- Aligning everyone from the team towards the common goal with consistent practices and culture code is the recipe for successful execution.

## 🤗 Complaint friendly environment
- Some members, specifically junior members of the team, may not feel comfortable voicing their opinions. Make sure senior members of your team are standing up for junior members.
- Force team leads/project leads to create private channels, which are not accessible to the managers/leadership where every person in the group can voice all of their problems.
- Never attribute issues to specific people. If there is a problem, there is a problem 😁. And its EMs job to look into it constructively.

<div class="info-box" style="margin-top:0px;" role="alert">
<h3 class="title">🤔 Things to consider when selecting engineering management tools </h3>
<hr style="margin-top:10px;margin-bottom:10px"/>
<div class="content">
	<span>✅ Pick one central tool like Slack or Teams, where most of the team is already present.</span><br/>
	<span>✅ Bring tools that connect with these central tools, so that teams don’t have to switch the context.</span><br/>
	<span>✅ Tools should give insights to speed up the decision making process for the team.</span><br/>
	<span>✅ And one important thing: These tools are vital to your job, not to the team. So make sure you don’t expect teams to make massive time and effort investments just to make your job easier.</span>
</div>
</div>