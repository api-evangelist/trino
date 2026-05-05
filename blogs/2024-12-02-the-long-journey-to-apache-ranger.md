---
title: "The long journey to Apache Ranger"
url: "https://trino.io/blog/2024/12/02/ranger.html"
date: "2024-12-02T00:00:00+00:00"
author: "Manfred Moser"
feed_url: "https://trino.io/blog/feed.xml"
---
<p><a href="https://trino.io/ecosystem/add-on.html#apache-ranger">Apache Ranger</a> has
arrived! With the new <a href="https://trino.io/docs/current/release/release-466.html">Trino
466</a> you all get another
jam-packed release of Trino awesomeness. One of the goodies is a new plugin for
access control for your data with Apache Ranger, and it has gone through a long
story to get here.</p>

<p>Apache Ranger has a long history and wide adoption as an access control system
for data lakes using Hadoop and Hive. Since Trino brings fast analytics to this
space, and also supports modern data lakehouses and other data sources, Apache
Ranger is a natural fit for access control on a Trino-powered data platform.</p>

<!--more-->

<h2 id="the-beginnings">The beginnings</h2>

<p>Apache Ranger has been in use with Trino for a long time - in fact there are
<a href="https://github.com/trinodb/trino/pull/244">early</a>,
<a href="https://github.com/trinodb/trino/pull/1069">rudimentary</a> pull requests from
2019 that implemented some support. And even before then, various hacks existed.
In 2020, a plugin for PrestoSQL was added to Apache Ranger. Aakash Nand blogged
about <a href="https://towardsdatascience.com/integrating-trino-and-apache-ranger-b808f6b96ad8">Integrating Trino and Apache
Ranger</a>
in 2021 to adjust for the changes to Trino. Jeff Xu followed up with
<a href="https://medium.com/@jeff.xu.z/integrating-trino-and-apache-ranger-in-a-kerberos-secured-enterprise-environment-997c95cd10e9">Integrating Trino and Apache Ranger in a Kerberos-secured enterprise
environment</a>
in 2022, followed quickly by the addition of the Trino support to the Apache
Ranger repository.</p>

<h2 id="testing-and-container-images">Testing and container images</h2>

<p>However that was only half of the needed support. The Trino project moves very
fast with nearly weekly releases, so the best approach is to have the supporting
plugin in Trino directly so every release includes the relevant updates. <a href="https://github.com/dprophet">Erik
Anderson</a> created a more mature plugin that was in
production use for quite a while for Trino. His <a href="https://github.com/trinodb/trino/pull/13297">pull request from July
2022</a> included great background
reasoning for having the plugin in Trino. One of the issues that Erik solved for
the Trino project is testing. Trino plugins require the availability of a
container image for testing whatever integration. Apache Ranger did still not
ship a container in 2022, but thanks to the lobbying efforts of Erik this
changed and a container image became available over the months.</p>

<h2 id="a-long-sprint">A long sprint</h2>

<p>Unfortunately, focus changed and while the PR from Erik existed and was useful,
it never made it to merge due to waning priorities. That changed when <a href="https://github.com/mneethiraj">Madhan
Neethiraj</a> from the Apache Ranger project stepped
up and created <a href="https://github.com/trinodb/trino/pull/22675">new PR</a> in July 2024.</p>

<p>We knew this could be another shot at it, and it would require a lot of work to
get it done, since we put a high focus on quality so that we can maintain the
Trino codebase for the long run. Monitoring all PRs regularly <a href="https://github.com/mosabua">I (Manfred
Moser)</a> noticed it and jumped in with first help.</p>

<p>Erik and other interested users chimed in.
<a href="https://github.com/lozbrown">lozbrown</a> and Manfred helped with documentation
and getting other developers interested. The heavy technical reviews and lots of
guidance came from <a href="https://github.com/ksobolew">Krzysztof Sobolewski</a> and
<a href="https://github.com/kokosing">Grzegorz Kokosiński</a>.</p>

<p>During the whole process, Madhan had to react to comments, update the code, and
also regularly rebase his PR to adjust for the constantly changing Trino
codebase in the master branch. Starburst recognized Madhan’s effort and
<a href="https://www.starburst.io/community/trino-champions/">featured him as Starburst Trino
Champion</a>. Interestingly,
the container image ended up not being used for testing, however it will be
crucially important for many users deploying Apache Ranger on Kubernetes anyway.
Nearly 400 comments and over four months later we all got to celebrate. The
Trino maintainer Grzegorz took on the responsibility and merged the PR. <a href="https://github.com/ebyhr">Yuya
Ebihara</a> and <a href="https://github.com/martint">Martin
Traverso</a> followed up with
<a href="https://github.com/trinodb/trino/pull/24238">minor</a>
<a href="https://github.com/trinodb/trino/pull/24252">cleanups</a>, and we finally shipped
the plugin as part of <a href="https://trino.io/docs/current/release/release-466.html">Trino
466</a>.</p>

<blockquote>
  <p><strong>A huge congratulations and thank you goes out to everyone involved.</strong></p>
</blockquote>

<p>Now it is your turn to have a look at the
<a href="https://trino.io/docs/current/security/apache-ranger-access-control.html">documentation</a>,
learn more about Trino and Apache Ranger, and maybe even proceed to help us
improve the integration.</p>

<h2 id="next-steps">Next steps</h2>

<p>Beyond our celebration, more tasks are waiting for all of us:</p>

<ul>
  <li>Test it out in your usage and migrate from any old or custom versions.</li>
  <li>Help us improve the
<a href="https://trino.io/docs/current/security/apache-ranger-access-control.html">documentation</a>
significantly to allow easier adoption.</li>
  <li>Work with lozbrown on adding support to the <a href="https://github.com/trinodb/charts">Helm chart</a>.</li>
  <li>Check out the codebase and help us fix bugs and add features.</li>
</ul>

<p>And last, but not least - join us all to celebrate Trino at the upcoming <a href="https://trino.io/blog/2024/11/22/trino-summit-2024-lineup.html">Trino
Summit 2024 for two days of amazing sessions and interaction with your peers
from the Trino community</a>
and the <a href="https://trino.io/community.html#events">Trino Contributor Call</a> for
more open community chat and discussion.</p>
