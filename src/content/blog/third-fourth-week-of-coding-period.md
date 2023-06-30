---
author: Satyam Bansal
pubDatetime: 2023-06-29T02:00:00
title: Third and Fourth Week of Coding Period | GSoC 2023
postSlug: third-fourth-week-of-coding-period-gsoc-2023
featured: true
draft: false
tags:
  - gsoc
  - zulip
  - open source
  - coding period
ogImage: ""
description: "Work I did during the third and fourth week of the coding period for GSoC 2023."
---

## Introduction

For half a week or so I was away from my laptop. So it kind of makes
sense to write about the work I did in the third and fourth week of
the coding period in a single blog post. I mostly worked on creating a
modal that simplifes the process that users have to go through when
creating a webhook URL for a Zulip Integration. Also, I worked on a
couple of user reported issues in GitHub Integration.

## Simplifying the process of creating a webhook URL

The prcocess for creating a webhook URL for an integration is a very
manual process. Below are the steps that the user has to follow to
create a webhook URL for an integration:

```
Construct the URL for the Airbrake bot using the bot's API key and the desired stream name:

https://yourZulipDomain.zulipchat.com/api/v1/external/airbrake?api_key=abcdefgh&stream=stream%20name

Modify the parameters of the URL above, where api_key is the API key of your Zulip bot, and stream is the URL-encoded stream name you want the notifications sent to. If you don't specify a stream, the bot will send notifications via private messages to the creator of the bot.

If you'd like this integration to always send notifications to a specific topic in the specified stream, just include the URL-encoded topic as an additional parameter. E.g., for your topic, append &topic=your%20topic to the URL.
```

As it is very clear that this requires a lot of manual work from
copying the API key to URL encoding the stream name. So the core
maintainers of Zulip
[suggested](https://github.com/zulip/zulip/issues/25976) that we
should simplify this process by providing a form to the user which
will generate the webhook URL for the user.

I started working on this and the first hurdle for me was to figure
out how a dialog box is shown to the user. There were other dialog
boxes in the codebase -- I inspected them and figured out that they
were being handled by
[dialog_widget.ts](https://github.com/zulip/zulip/blob/main/web/src/dialog_widget.ts)
which is very nice abstraction for handling dialog boxes.

I just have to say that the more I explore the Zulip codebase the more
I am amazed by the abstractions that the codebase provides -- which in
turns leads to a great developer experience. 💙

After figuring this out, I started on creating a
[Handlebars](https://handlebarsjs.com/) template for the dialog box.
Thanks to the detailed requirements that the maintainers provided, I
was able to create the template fairly quickly.

![Design of the dialog
box](/assets/generate_url_integration_design.png)

Some changes in the backend were also required to make this work as
the list of integrations is required for the `Integration` field.
Zulip also allows filtering of events for an integration. So some
further changes to the backend were required to get this done. I
[discussed](https://chat.zulip.org/#narrow/stream/378-api-design/topic/integration.20display.20name/near/1598446)
the changes with the community members and their suggestions were
really helpful.

I found that my design is not perfectly compatible with the dark theme
of Zulip. So I am working on that and will be done with it soon. I
will keep updating about the progress on this in the subsequent blog
posts.

## Other issues

Through the week I also worked on the following issues related to
GitHub integration:

- [#25789](https://github.com/zulip/zulip/issues/25789) was related to
  improving the notifications that were sent when an issue was labeled
  or unlabeled. Earlier, the notification had no info about what label
  was added or removed. Instead it had the description of the issue.
  So I changed the notification to include the label that was added or
  removed. I also discussed about this in my [first week
  blog](http://localhost:3000/posts/first-week-of-coding-period-gsoc-2023).
- [#26055](https://github.com/zulip/zulip/issues/26055) was fixing a
  mistake which resulted in a field being allowed to be only string.
  But there exists a case where the field can be a string or null. So
  had to change the validator to allow null values.

Both of the above issues are fixed now. 🎉

## Social Event

We were able to host a video call with other GSoC students and one of
the team members of Zulip. It was really nice to meet other GSoC
students and was very fun to talk to them. From that meet I found that
there are two other GSoC students who are working on Zulip from my
city. So we are planning to meet in person as well. 😄

## Conclusion

I think the week was fairly productive. I still need to cut some time
off from the things that eat my time ~INSTAGRAM~. Also, spent a good
amount of time watching the Ashes series. I am really enjoying it. 🏏

I have also started triaging issues -- this is something that I have
never done before. I was able to get amount of work done in the week.
My mentor has also been very helpful and has been guiding me
throughout the process. 💓

Looking forward to have a great week ahead.

Feel free to drop any suggestions either on my
[Twitter](https://twitter.com/sbansal1999) or you can do it the school
way by sending me an email at
[sbansal1999@gmail.com](mailto:sbansal1999@gmail.com)
