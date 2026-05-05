---
title: "Core Principles and Design Practices of OLAP Engines"
url: "https://trino.io/blog/2025/03/27/olap-principles-book.html"
date: "2025-03-27T00:00:00+00:00"
author: "Yiteng Xu, Yingju Gao, Manfred Moser"
feed_url: "https://trino.io/blog/feed.xml"
---
<p>Yiteng Xu and Yingju Gao are proudly announcing the new book “Core Principle and
Design Practices of OLAP Engines” from China Machine Press. This is great news
for the Trino community, since the book is based on the open source project
Trino, specifically Trino 350. It took more than four years for the two authors
to finish writing. All concepts and details are explained with Trino falvor and
generalized to all OLAP engines. Let us walk throught the chapters and you will
find out the two author dive deep into the source code layer and bring you so
many treasures.</p>

<!--more-->

<h2 id="author-introduction">Author introduction</h2>

<p><a href="https://github.com/medsmeds">Yiteng (Ivan) Xu</a>: is a data security engineer and
is currently utilizing Trino, Spark, and Calcite for SQL analysis. His work
encompasses various scenarios, including data warehouse metrics, SQL
auto-rewriting, SQL purpose detection, and the development of SQL-based
Purpose-Aware Access Control System.</p>

<p><a href="https://github.com/garyelephant">Yingju (Gary) Gao</a> is an Apache Seatunnel PMC
member and the lead of the time series database team. He currently serves as the
technical lead for the observability-engine team, and is responsible for
building the ecosystem for observability data, including metrics, trace, log,
and event data, providing a high-performance, high-throughput data pipeline from
ingestion to consumption, storage, querying, and data warehousing. Additionally,
he oversees metrics stability, multi-tenant access, and user requirement
integration.</p>

<p>Both authors are passionate about sharing their technical knowledge. They have
delved deep into source code and excel in technical writing, breaking down
complex underlying principles into a linear and comprehensible format for
readers. They firmly believe that sharing is a virtue and are committed to
continuing their technical contributions.</p>

<p>So now it is time to get the book, or read on for a walk through of the content:</p>

<div class="card-deck spacer-30">
    <a class="btn btn-pink" href="https://product.dangdang.com/11974653727.html" target="_blank">
        Get the book from dangdang.com
    </a>
    <a class="btn btn-pink" href="https://item.m.jd.com/product/10136949561522.html" target="_blank">
        Get the book from jd.com
    </a>
</div>

<h2 id="walk-through">Walk through</h2>

<p>Let’s have a look at the different chapters in a high-level walk through.</p>

<h3 id="part-1-background-knowledge">Part 1: Background knowledge</h3>

<p><strong>Chapter 1</strong>: Introduce the concept of OLAP (Online Analytical Processing),
provide comparsion among different engines like Trino, Impala, Doris and others.</p>

<p><strong>Chapter 2</strong>: Provides a comprehensive introduction to the Trino engine,
covering its principles, architecture, enterprise use cases, compilation, and
execution. It also compares Trino with the Presto project and introduces the
SQL statements that are referenced throughout the book.</p>

<h3 id="part-2-core-principles">Part 2: Core principles</h3>

<p><strong>Chapter 3</strong>: Offers an overview of the distributed SQL query process, serving
as a high-level introduction to the subsequent chapters.</p>

<p><strong>Chapter 4</strong>: Begins with the generation of query execution plans, including
the transformation of SQL into abstract syntax trees, semantic analysis, and the
creation of initial logical plans. It then delves into the theoretical knowledge
of optimizers and the overall framework of the Trino optimizer.</p>

<h3 id="part-3-classic-sql">Part 3: Classic SQL</h3>

<p><strong>Chapter 5</strong>: Explains the generation and optimization of execution plans for
SQL statements involving only <code class="language-plaintext highlighter-rouge">TableScan</code>, <code class="language-plaintext highlighter-rouge">Filter</code>, and <code class="language-plaintext highlighter-rouge">Project</code> operations,
along with their scheduling and execution processes.</p>

<p><strong>Chapter 6</strong>: Focuses on SQL statements with <code class="language-plaintext highlighter-rouge">Limit</code> and <code class="language-plaintext highlighter-rouge">Sort</code> operations,
detailing the generation and optimization of execution plans, as well as their
scheduling and execution.</p>

<p><strong>Chapter 7</strong>: Introduces the basic principles of aggregate queries. It then
covers the generation and optimization of execution plans for grouped and
non-grouped aggregate SQL statements, along with their scheduling and execution
processes.</p>

<p><strong>Chapter 8</strong>: Discusses SQL statements with count distinct and multiple
aggregate operations, explaining the generation and optimization of execution
plans, as well as their scheduling and execution. This includes the
<code class="language-plaintext highlighter-rouge">Scatter-Gather</code> model and <code class="language-plaintext highlighter-rouge">MarkDistinct</code> optimization. Finally, a complex SQL
statement is used to tie together the concepts from Chapters 5 to 8.</p>

<h3 id="part-4-data-exchange-mechanism">Part 4: Data exchange mechanism</h3>

<p><strong>Chapter 9</strong>: Introduces the overall concept of data exchange mechanisms and
how data exchange is incorporated during the query optimization phase via the
<code class="language-plaintext highlighter-rouge">AddExchanges</code> optimizer, along with the design principles for scheduling and
execution.</p>

<p><strong>Chapter 10</strong>: Explains how tasks establish connections during the query
scheduling phase and the mechanisms for upstream and downstream data flow during
execution. It also covers the principles of intra-task data exchange, RPC
interaction mechanisms, and analyzes backpressure, Limit semantics, and
out-of-order request handling.</p>

<h3 id="part-5-plugin-mechanisms-and-connectors">Part 5: Plugin mechanisms and connectors</h3>

<p><strong>Chapter 11</strong>: Begins with an introduction to Trino’s plugin system and SPI
mechanism, including plugin loading and JVM’s class loading principles. It then
dissects connectors, covering metadata modules, read modules, pushdown
optimization, and providing in-depth insights into connector design.</p>

<p><strong>Chapter 12</strong>: Uses the example-http connector to help readers understand
connector design and implements a simple data source using Python’s Flask
framework.</p>

<h3 id="part-6-function-principles-and-development">Part 6: Function principles and development</h3>

<p><strong>Chapter 13</strong>: Provides an overview of Trino’s function system, including
function types, lifecycle, and several function development methods. It delves
into the data structures and annotations related to functions and explains the
function registration and parsing process during semantic analysis.</p>

<p><strong>Chapter 14</strong>: Focuses on how to write a udf in practice. It covers
annotation-based development methods for scalar functions, as well as low-level
development methods using <code class="language-plaintext highlighter-rouge">codeGen</code> or <code class="language-plaintext highlighter-rouge">methodHandle</code> APIs. For aggregate
functions, it introduces annotation-based development methods and low-level
methods where developers handle serialization and state on their own.</p>

<h3 id="why-trino">Why Trino?</h3>

<p>In 2020, one of the authors, Yiteng Xu, encountered a scenario at work where
data needed to be read from two Hive instances, each modified by different
internal teams. The company’s infrastructure team attempted a simple solution by
registering virtual tables and using MapReduce for federated queries. However,
this approach proved inadequate for the agile analysis needs of data analysts,
with complex queries taking nearly 12 hours to complete. One mistake per SQL
meant an entire day was wasted.</p>

<p>Later, another team researched and adopted Presto (before Trino became
independent). By adapting the Hive engine at the connector level, they enabled
federated queries across the two Hive instances without data migration or
extensive code changes. Users only needed to be aware of a catalog prefix,
making the process incredibly convenient. The author later had the opportunity
to participate in the project and developed a strong interest in its source
code. The elegance of the open-source project, its plugin design, and the inner
workings of connectors and Airlift framework sparked a deep curiosity, leading
the author on a journey of source code exploration. As the PrestoSQL project was
more active and receptive to developer feedback, the author chose to continue
following the Trino project when it emerged in late 2020.</p>

<h2 id="get-your-copy">Get your copy</h2>

<p>Now it is time for you to get your copy of <strong>Core Principles and Design Practices of OLAP Engines</strong>:</p>

<div class="card-deck spacer-30">
    <a class="btn btn-pink" href="https://product.dangdang.com/11974653727.html" target="_blank">
        Get the book from dangdang.com
    </a>
    <a class="btn btn-pink" href="https://item.m.jd.com/product/10136949561522.html" target="_blank">
        Get the book from jd.com
    </a>
</div>
