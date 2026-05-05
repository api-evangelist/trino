---
title: "Twenty four"
url: "https://trino.io/blog/2025/03/03/java-24.html"
date: "2025-03-03T00:00:00+00:00"
author: "Manfred Moser, Mateusz Gajewski"
feed_url: "https://trino.io/blog/feed.xml"
---
<p>Six month ago <a href="https://trino.io/blog/2024/09/17/java-23.html">we adopted Java 23 as requirement</a>, following our standard procedure to upgrade with each Java version as soon
as it becomes available. This allows us to take advantage of all the great
improvement each release brings. The upgrade to 23 was pretty easy since the
changes from 22 to 23 were not that big. The story turns out to be a bit
different now with our upgrade to Java 24.</p>

<!--more-->

<h2 id="java-24-features">Java 24 features</h2>

<p>We have been <a href="https://github.com/trinodb/trino/issues/23498">planning and working towards the
upgrade</a> consistently since the
23 bump in September. Java 24 is set to be released in March 2025 and the list
of changes is quite significant:</p>

<ul>
  <li>JEP 450 Compact Object Headers (Experimental)</li>
  <li>JEP 472 Prepare to Restrict the Use of JNI</li>
  <li>JEP 475 Late Barrier Expansion for G1</li>
  <li>JEP 478 Key Derivation Function API (Preview)</li>
  <li>JEP 483 Ahead-of-Time Class Loading &amp; Linking</li>
  <li>JEP 484 Class-File API</li>
  <li>JEP 485 Stream Gatherers</li>
  <li>JEP 486 Permanently Disable the Security Manager</li>
  <li>JEP 487 Scoped Values (Fourth Preview)</li>
  <li>JEP 488 Primitive Types in Patterns, instanceof, and switch (Second Preview)</li>
  <li>JEP 489 Vector API (Ninth Incubator)</li>
  <li>JEP 490 ZGC: Remove the Non-Generational Mode</li>
  <li>JEP 491 Synchronize Virtual Threads without Pinning</li>
  <li>JEP 492 Flexible Constructor Bodies (Third Preview)</li>
  <li>JEP 494 Module Import Declarations (Second Preview)</li>
  <li>JEP 495 Simple Source Files and Instance Main Methods (Fourth Preview)</li>
  <li>JEP 496 Quantum-Resistant Module-Lattice-Based Key Encapsulation Mechanism</li>
  <li>JEP 497 Quantum-Resistant Module-Lattice-Based Digital Signature Algorithm</li>
  <li>JEP 498 Warn upon Use of Memory-Access Methods in sun.misc.Unsafe</li>
  <li>JEP 499 Structured Concurrency (Fourth Preview)</li>
</ul>

<p>The list of new features is also quite large. You can find more details
in the <a href="https://jdk.java.net/24/release-notes">release notes</a> and each
individual JEP.</p>

<h2 id="trino-perspective">Trino perspective</h2>

<p>From a Trino perspective we want to specifically take advantage of performance
improvements to MemorySegment (mismatch, copy, fill), “JEP 491 Synchronize
Virtual Threads without Pinning” and “JEP 475 Late Barrier Expansion for G1”. On
the other hand <a href="https://openjdk.org/jeps/486">JEP 486 Permanently Disable the Security
Manager</a> turned out to be the most impactful.</p>

<p>Since Trino and its connectors have a large footprint of dependencies there was
a high chance that some projects as not keeping up with the security manager
removal, although it was first deprecated with Java 17 in 2021.</p>

<p>At this stage the Kafka, Kudu, and Phoenix connectors are affected. The Kafka
project is planning to make a new compatible release available in time and we
will adopt that version.</p>

<p>The Kudu and Phoenix connectors however will be removed, since it is not
possible to use them with Java 24 as requirement. Both connectors are not
heavily used in our community as we learned from our communication with numerous
users, integrators, and the results from our <a href="https://trino.io/blog/2025/01/07/2024-and-beyond.html">user survey</a>. We are tracking progress for each removal in the
issues <a href="https://github.com/trinodb/trino/issues/24419">#24419 Phoenix connector</a>
and <a href="https://github.com/trinodb/trino/issues/24417">#24417 Kudu connector</a>. If
either of these communities ends up supporting Java 24, or a newer version as
required by Trino, in the future, we can potentially add the connectors back in
if community members contribute updated versions.</p>

<h2 id="release-plans">Release plans</h2>

<p>In terms of shipping the changes we follow our established pattern:</p>

<ul>
  <li>Clean up codebase and get it ready, specifically this include the removal of
the Kudu and Phoenix connectors.</li>
  <li>Cut a release that is completely ready to be used with Java 24, but does not
yet make it a hard requirement</li>
  <li>Allow for community testing and feedback using Java 24.</li>
  <li>Introduce Java 24 as hard requirement in another release.</li>
  <li>Adopt Java 24 features and bring the benefits to our users with following
releases.</li>
</ul>

<p>As you see, there is a bunch of work waiting, we we better back to it. As usual,
if you have questions or comments, chime in on the relevant issue or chat with
us on <a href="https://trino.io/slack.html">Trino Slack</a> in the <a href="https://trinodb.slack.com/messages/C07ABNN828M">core-dev
channel</a>.</p>
