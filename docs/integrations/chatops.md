---
layout: page
title: ChatOps
group: Integrations
---

# ChatOps

ChatOps is a term that's been coined recently to refer to using Slack or other
chat services to deploy code or run other operations. It's a simple concept,
to gain visibility over deployments and other operational concepts, we expose to
the developers tools in Slack so that they can deploy code, look at graphs or
do other operational things within a conversation channel.

ChatOps is great for building living documentation for your team. Onboarding a
new team member? They can watch Slack as a way of learning how deployments
happen and even chime in with feedback or questions in real time.

The Deliverybot Slack integration is simple. It simply takes the functionality
you get from the dashboard where clicking on a commit deploys that code and
moves it into Slack.

Deploying code looks like the following slash command:

```
/deliverybot deploy deliverybot/example@heads/master production
```

![slack-app](/assets/images/deploy-slack.png)

That's it! All your current integrations with Deliverybot will work just the
same. Just call `/deliverybot help` if you have any questions.

## Install

Install using the link below:

<a href="https://slack.com/oauth/authorize?client_id=761732924261.765903727079&scope=commands"><img alt="Add to Slack" height="40" width="139" src="https://platform.slack-edge.com/img/add_to_slack.png" srcset="https://platform.slack-edge.com/img/add_to_slack.png 1x, https://platform.slack-edge.com/img/add_to_slack@2x.png 2x"></a>
