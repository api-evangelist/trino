---
title: "Trino and Javascript?! YES!"
url: "https://trino.io/blog/2024/11/18/javascript.html"
date: "2024-11-18T00:00:00+00:00"
author: "Manfred Moser"
feed_url: "https://trino.io/blog/feed.xml"
---
<p>Trino is written in Java. Trino contributors and maintainers are often veterans
in the Java ecosystem and community, and Trino is very modern when it comes to
Java. For example, Trino now requires the latest Java version and actively uses
new features.</p>

<p>When it comes to JavaScript however, the story is a bit more complicated. Of
course, JavaScript is commonly used in the Trino ecosystem and codebase. Let’s
look at some of the specifics.</p>

<!--more-->

<h2 id="client-driver-and-applications">Client driver and applications</h2>

<p>Client applications that allow users to submit queries to Trino, and then
receive the results are written in numerous languages. Trino has good support
for <a href="https://trino.io/ecosystem/index.html#clients">many of them</a>.</p>

<p>Thanks to the collaboration with <a href="https://github.com/regadas">Filipe Regadas</a>
and the contribution of his JavaScript client driver to the Trino community, we
now have an official
<a href="https://github.com/trinodb/trino-js-client">trino-js-client</a> project. After his
initial donation we have applied numerous improvements and recently cut our
first release.</p>

<p>The client is already used in the <a href="https://trino.io/ecosystem/client#vscode">VisualCode
support</a>, the <a href="https://trino.io/ecosystem/client#emacs">Emacs
support</a>, the example project discussed
in <a href="https://trino.io/episodes/63.html">Trino Community Broadcast episode 63</a>,
and numerous other applications.</p>

<p>And we have big plans as well:</p>

<ul>
  <li>Add support for more authentication methods supported in Trino</li>
  <li>Improve documentation and example projects</li>
  <li>Add support for the new spooling client protocol from Trino</li>
  <li>Test with Trino Gateway and adjust as needed</li>
</ul>

<p>While this project is a great addition for many users of Trino and their custom
web applications, there are numerous other usages of JavaScript in the project.</p>

<h2 id="user-interfaces">User interfaces</h2>

<p>Web-based user interfaces are one important use of JavaScript. Trino includes
the <a href="https://trino.io/docs/current/admin/web-interface.html">Trino Web UI</a> and
the ongoing effort to replace it with a more modern and feature rich UI -
currently called the <a href="https://trino.io/docs/current/admin/preview-web-interface.html">Preview
UI</a>. It was
inspired by the replacement of the legacy UI for <a href="https://trinodb.github.io/trino-gateway/">Trino
Gateway</a> with a new UI based on
current tools and libraries.</p>

<p>All three user interfaces require constant work in terms of upkeep to current
libraries, bug fixes, and addition of new features.</p>

<h2 id="other-projects">Other projects</h2>

<p>Beyond the user interfaces we also provide a <a href="https://github.com/trinodb/grafana-trino">plugin for
Grafana</a> that is mostly written in
Javascript, and there might be more projects on the way.</p>

<h2 id="whats-next">What’s next?</h2>

<p>The skills and experience needed for all these JavaScript-based efforts are
different enough to ensure that there are developers out there who can help in
these efforts without knowing much about Trino and Java.</p>

<p>If that is you, we want to hear from you. And if you are also knowledgable in
Trino, Java, and many other things, and also interested to help on the
JavaScript stuff, we also want to hear from you. There is always more stuff we
want to get done and we need your help.</p>

<p>So have a look at the codebase that interests you the most, chat with us on
<a href="https://trino.io/slack.html">Trino Slack</a>, join an <a href="https://trino.io/community.html#events">upcoming Trino contributor
call</a> and <a href="https://trino.io/blog/2024/10/17/trino-summit-2024-tease.html">Trino Summit</a>, and let me know if you would be
interested in a regular Trino JavaScript call - for example monthly?</p>

<p>And if you don’t want to code in Java or JavaScript? Well, you can help us write
<a href="https://github.com/trinodb/trino/tree/master/docs">documentation in Markdown</a>,
work on the <a href="https://github.com/trinodb/trino-python-client">Python client</a>, the
<a href="https://github.com/trinodb/trino-go-client">Go client</a>, or maybe even
contribute a client we don’t even have yet.</p>

<p>In all cases, we look forward to your help.</p>
