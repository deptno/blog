---
title: slackbot
layout: post
category: [dev, js]
date: 2016-04-12
tags: [slackbot, slack]
description: hubot과 연동해서 slack에서 귀찮은 일들을 대신해줄 봉봇(bongbot)을 만들기로 했다.
---

hubot과 연동해서 slack에서 귀찮은 일들을 대신해줄 봉봇(bongbot)을 만들기로 했다.

```bash
npm install -g yo generator-hubot
mkdir slackbot
cd slackbot
yo bongbot
vi bin/hubot
```

```bash
export HUBOT_SLACK_TOKEN=[TOKEN]
# export HUBOT_SLACK_TEAM=[TEAM]
# export HUBOT_SLACK_BOTNAME=[BOTNAME]
# export HUBOT_LOG_LEVEL=[LOG_LEVEL]
```

`HUBOT_SLACK_TOKEN`은 <https://slack.com/apps/>에서 생성할 수 있다.

```bash
npm install
./bin/hubot --adapter slack 
```

---

그리고...

## 봉봇

``` bash
git clone https://github.com/deptno/bongbot.git
cd bongbot
npm install
./bin/hubot
```

refs:
<https://github.com/github/hubot/blob/master/docs/index.md>
