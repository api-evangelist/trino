---
title: "Out with the old file system"
url: "https://trino.io/blog/2025/02/10/old-file-system.html"
date: "2025-02-10T00:00:00+00:00"
author: "Manfred Moser, David Phillips, Mateusz Gajewski"
feed_url: "https://trino.io/blog/feed.xml"
---
<p>What a long journey it has been! From the start Trino supported querying Hive
data and used libraries from the Hive and Hadoop ecosystem. With the release of
<a href="https://trino.io/docs/current/release/release-470.html">Trino 470</a> we mark
another milestone to more features and better performance for data lake and
lakehouse querying with Trino. We deprecated the legacy file system support, and
will permanently remove them in an upcoming release.</p>

<!--more-->

<h2 id="background">Background</h2>

<p>Trino always had a focus on performance and security. As a result we implemented
custom readers for file formats like Apache ORC and Apache Parquet many years
ago. We also have improved libraries for compression and decompression of files
from object storage and and implemented our own support for other table formats
with the Apache Iceberg, Delta Lake and Apache Hudi connectors.</p>

<p>For the underlying object storage solutions and file systems, we originally
extended the libraries around the Hive system and added implementations for
Amazon S3, Azure Storage, Google Cloud Storage and others. Over time the
mismatch of the HDFS libraries and the cloud-centric usage with modern file
systems became more and more of a maintenance headache. It also represented an
unnecessary complexity overhead, resulted in performance problems, and forced us
to carry the Hadoop dependencies with all their baggage of old Java code and
security issues.</p>

<p>In the end David Philips, as our file system lead, decided in 2022 that it was
time to write our own file system support as needed for Trino. By summer of 2023
and with Trino 419 a <a href="https://github.com/trinodb/trino/pull/17498">first support for
S3</a> became available for the
Iceberg and Delta Lake connectors. Over a year later in September 2024 and with
<a href="https://trino.io/docs/current/release/release-458.html">Trino 458</a>, we declared
the old file system support on top of the Hadoop libraries legacy and advised
users to migrate.</p>

<p>Since then you are required to declare what file system you want to enable in
each catalog with <code class="language-plaintext highlighter-rouge">fs.native-azure.enabled=true</code>,<code class="language-plaintext highlighter-rouge">fs.native-gcs.enabled=true</code> or
<code class="language-plaintext highlighter-rouge">fs.native-s3.enabled=true</code>. If you are truly using HDFS, or if you insist on
using the old legacy support you can also use <code class="language-plaintext highlighter-rouge">fs.hadoop.enabled=true</code>.</p>

<h2 id="trino-470">Trino 470</h2>

<p>With the recent <a href="https://trino.io/docs/current/release/release-470.html">Trino 470
release</a> from February
2025, we took the next step. All catalog configuration properties for using the
old, legacy support for accessing Azure Storage, Google Cloud Storage, S3, and
S3-compatible file systems are now <strong>deprecated</strong>.</p>

<p>These properties include all names starting with <code class="language-plaintext highlighter-rouge">hive.azure</code>, <code class="language-plaintext highlighter-rouge">hive.cos</code>,
<code class="language-plaintext highlighter-rouge">hive.gcs</code>, and <code class="language-plaintext highlighter-rouge">hive.s3</code>. The result of this deprecation is that Trino emits
warnings during the startup for each of these properties in the server log.</p>

<p>We also removed all documentation for the old properties, leaving only relevant
migration guides in place.</p>

<h2 id="next-steps">Next steps</h2>

<p>Within the next weeks or months we will completely remove all these properties
and the underlying code. We therefore renew our call out from numerous
contributor calls, Trino Community Broadcast episodes, and our Trino Fest and
Trino Summit events:</p>

<blockquote>
  <p>Stop using the old legacy file systems today.</p>
</blockquote>

<p>If you need help, have a look at the documentation for your connector, the file
system you use, and the migration guide for each file system:</p>

<ul>
  <li><a href="https://trino.io/docs/current/connector/hive.html">Delta Lake connector</a></li>
  <li><a href="https://trino.io/docs/current/connector/hive.html">Hive connector</a></li>
  <li><a href="https://trino.io/docs/current/connector/hive.html">Hudi connector</a></li>
  <li><a href="https://trino.io/docs/current/connector/hive.html">Iceberg connector</a></li>
  <li><a href="https://trino.io/docs/current/object-storage/file-system-azure.html">Azure Storage file system support</a></li>
  <li><a href="https://trino.io/docs/current/object-storage/file-system-gcs.html">Google Cloud Storage file system support</a></li>
  <li><a href="https://trino.io/docs/current/object-storage/file-system-s3.html">S3 file system support</a></li>
</ul>

<p>The new systems are more stable and performant, and save you time and money.
Migrate today, and if you encounter any issues, or find that there are features
missing, ping us on <a href="https://trino.io/slack./html">Slack</a> and chime in on the
<a href="https://github.com/trinodb/trino/issues/24878">roadmap issue for the removal of the legacy file system
support</a>.</p>
