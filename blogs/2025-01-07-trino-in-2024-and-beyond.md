---
title: "Trino in 2024 and beyond"
url: "https://trino.io/blog/2025/01/07/2024-and-beyond.html"
date: "2025-01-07T00:00:00+00:00"
author: "Manfred Moser"
feed_url: "https://trino.io/blog/feed.xml"
---
<p>Wow, what an amazing year 2024 was for Trino! Martin Traverso presented about
the achievements and progress of the project at the <a href="https://trino.io/blog/2024/12/18/trino-summit-2024-quick-recap.html">recent Trino Summit
2024</a>. Let me dive
deeper into the content of his keynote and elaborate some more about our amazing
plans for the future.</p>

<!--more-->

<h2 id="statistics">Statistics</h2>

<p>In his first slide of the presentation <strong>Enduring with persistence to reach the
summit</strong> Martin presented some of the amazing statistics of the year:</p>

<ul>
  <li>Over 30 releases packed with features and improvements - <a href="https://trino.io/docs/current/release.html#releases-2024">Trino releases 436-467</a></li>
  <li>5,000+ additional commits to the 40,000+ total commits since project start</li>
  <li>225+ unique contributors in 2024, 925+ total</li>
  <li>10.5k+ stars on GitHub</li>
  <li>13,500+ Slack members</li>
  <li>Trino Community Broadcast episodes 54-67</li>
</ul>

<h2 id="improvements">Improvements</h2>

<p>Some of the major improvements in Trino are:</p>

<ul>
  <li>Access controls with
<a href="https://trino.io/docs/current/security/opa-access-control.html">Open Policy Agent</a> and
<a href="https://trino.io/docs/current/security/ranger-access-control.html">Apache Ranger</a></li>
  <li>Improved observability with <a href="https://trino.io/docs/current/admin/event-listeners-openlineage.html">OpenLineage</a>, 
<a href="https://trino.io/docs/current/admin/opentelemetry.html">OpenTelemetry</a>, OpenMetrics, and 
<a href="https://trino.io/docs/current/admin/event-listeners-kafka.html">Kafka</a></li>
  <li>Significant <a href="https://trino.io/docs/current/client/client-protocol.html">client protocol</a> improvements</li>
  <li><a href="https://trino.io/docs/current/udf/python.html">Python user-defined functions</a></li>
  <li>New connectors such as <a href="https://trino.io/docs/current/connector/faker.html">Faker</a>,
<a href="https://trino.io/docs/current/connector/snowflake.html">Snowflake</a>, or
<a href="https://trino.io/docs/current/connector/vertica.html">Vertica</a></li>
  <li>Numerous improvements on object storage connectors and integrations</li>
</ul>

<p>Of course we also paid a lot of attention to bug fixes and shipped tremendous
performance improvements.</p>

<h2 id="slides-and-video">Slides and video</h2>

<p>If you want to find out all the details, have a look at the
<a href="https://trino.io/assets/blog/trino-summit-2024/trino-summit-2024-keynote.pdf"><strong>slides</strong></a>
and the video recording:</p>

<p><a href="https://www.youtube.com/watch?v=wmR6kzOCo-I"><img alt="YouTube" src="https://img.youtube.com/vi/wmR6kzOCo-I/0.jpg" /></a></p>

<h2 id="other-projects">Other projects</h2>

<p>Martin also talked about the many improvements in other Trino projects such as
<a href="https://trinodb.github.io/trino-gateway/">Trino Gateway</a>,
<a href="https://github.com/trinodb/trino-python-client">trino-python-client</a>, the new
<a href="https://github.com/trinodb/trino-js-client">trino-js-client</a>, and the new
<a href="https://github.com/trinodb/trino-csharp-client">trino-csharp-client</a>.</p>

<h2 id="plans-for-2025">Plans for 2025</h2>

<p>For 2025, we have some pretty big plans in addition to our continued software
supply chain attention, performance improvemsnts and bug fixes.</p>

<ul>
  <li>Secrets management and dynamic catalogs</li>
  <li>Client protocol improvements for all client drivers</li>
  <li><a href="https://github.com/trinodb/trino/issues/22597">Packaging improvements</a></li>
  <li>More connectors such as DuckDB, LanceDB, HsqlDB, Loki, …</li>
  <li>Continued and even increased work on performance improvements</li>
  <li>Research and prototype towards a next generation optimizer</li>
  <li>SQL language improvements such as <code class="language-plaintext highlighter-rouge">PIVOT</code>, <code class="language-plaintext highlighter-rouge">ASOF</code> joins, …</li>
</ul>

<p>Of course, what really happens in 2025 and Trino depends on you all. The project
lives and breathes only thanks to the efforts of all our contributors and
maintainers and we look forward to working with you all.</p>

<h2 id="trino-survey">Trino survey</h2>

<p>Besides filing issues, sending pull requests, and discussing topics on Slack and
GitHub, we also have some specific questions and would really appreciate your
feedback. Answering should take less than a minute.</p>

<div class="card-deck spacer-30">
    <a class="btn btn-pink" href="https://docs.google.com/forms/d/e/1FAIpQLSfrEIZ_5iyj17_hMJMdFhCIx9bQyHm6G-x6-CIq2VajURm6cQ/viewform?usp=sharing" target="_blank">
        Help by answering the Trino survey
    </a>
</div>
<p><br /></p>

<p>With Trino as a huge collaborative effort only one thing is for certain:</p>

<blockquote>
  <p>2025 will be an exciting year for Commander Bun Bun, Trino, and the Trino project.</p>
</blockquote>
