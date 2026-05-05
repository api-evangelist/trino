---
title: "Introducing the NUMBER data type"
url: "https://trino.io/blog/2026/03/25/number-data-type.html"
date: "2026-03-25T00:00:00+00:00"
author: "Piotr Findeisen, Starburst Data"
feed_url: "https://trino.io/blog/feed.xml"
---
<p>One of Trino’s core strengths is breaking down data silos—enabling data
engineers to query diverse data sources through a single SQL interface. However,
when those sources use high-precision numeric types beyond Trino’s 38-digit
DECIMAL limit, that promise breaks down. Users faced an impossible choice: skip
the columns entirely and lose access to critical data, or accept lossy rounding
that compromises data integrity.</p>

<p>This challenge required a new approach: a dedicated data type for high-precision,
variable-scale decimals.</p>

<!--more-->

<p>Adding a new built-in data type to Trino is exceptionally rare. The last time we
introduced a new type was the UUID type in May 2019—nearly seven years ago.
Types are fundamental building blocks that touch many parts of the system, from
the type registry, through coercion rules to connectors, functions, and the protocol.
They require careful design and long-term commitment.</p>

<p>With Trino 480, we’re excited to introduce the NUMBER type—a high-precision
decimal type that breaks down these data silos and enables seamless access to
numeric data across diverse database systems. This addition is particularly
powerful for data engineers working with Oracle, PostgreSQL, MySQL, MariaDB, and
SingleStore, which support numeric precision beyond the traditional 38-digit
DECIMAL limit.</p>

<p>Let’s explore why NUMBER matters, how it works, and how it will simplify your
data integration workflows.</p>

<h2 id="the-challenge-precision-beyond-38-digits">The challenge: precision beyond 38 digits</h2>

<p>Trino’s DECIMAL type has long supported exact numeric values with precision up
to 38 decimal digits, which covers the vast majority of use cases. However,
many database systems support higher precision:</p>

<ul>
  <li><strong>Oracle NUMBER</strong>: when declared as <code class="language-plaintext highlighter-rouge">NUMBER(p, s)</code>, precision must be in [1, 38] and
scale in [-84, 127]. When declared as <code class="language-plaintext highlighter-rouge">NUMBER</code> without precision/scale, each value
can have different scale, and actual precision can reach 40 decimal digits. Oracle can
store values from 10^-130 to (but not including) 10^126.</li>
  <li><strong>PostgreSQL NUMERIC</strong>: supports precision and scale in range from -1000 to 1000;
supports very high precision numbers with up to 131,072 digits before the decimal point.
When declared without precision/scale constraints, each value can have different scale.</li>
  <li><strong>MySQL, MariaDB, SingleStore DECIMAL</strong>: up to 65 digits of precision (scale 0-30)</li>
</ul>

<p>Before Trino 480, accessing these high-precision numeric columns required
choosing between two unsatisfying options:</p>

<ol>
  <li><strong>Skip the columns entirely</strong> and lose access to potentially critical data.
This was the default behavior.</li>
  <li><strong>Accept lossy conversions</strong> - Use <code class="language-plaintext highlighter-rouge">decimal-mapping=ALLOW_OVERFLOW</code> with
<code class="language-plaintext highlighter-rouge">decimal-default-scale=S</code> to force values into <code class="language-plaintext highlighter-rouge">DECIMAL(38, S)</code>, losing precision
through rounding and failing for numbers greater than or equal to 10^(38-S).
For example, with scale 10, values ≥ 10^28 would fail.</li>
</ol>

<p>Neither option is ideal for data federation and warehousing scenarios where
preserving data fidelity is essential.</p>

<h2 id="enter-number-arbitrary-precision-decimals-in-trino">Enter NUMBER: arbitrary-precision decimals in Trino</h2>

<p>The NUMBER type solves this problem by supporting floating-point decimal numbers
of high precision and flexible scale. In practice, NUMBER supports values with
up to 200 digits of precision – far exceeding what most database workloads require.
Each value can have a different scale, allowing for values as small as 10^-16000
(or even smaller) and as large as 10^16000 (or even larger) within the same column.</p>

<p>Here’s what NUMBER looks like in action:</p>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- High-precision literal (50+ digits)</span>
<span class="k">SELECT</span> <span class="n">NUMBER</span> <span class="s1">'3.1415926535897932384626433832795028841971693993751'</span><span class="p">;</span>
</code></pre></div></div>

<div class="language-text highlighter-rouge"><div class="highlight"><pre class="highlight"><code> 3.1415926535897932384626433832795028841971693993751
</code></pre></div></div>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- Scientific notation with extreme precision</span>
<span class="k">SELECT</span> <span class="n">NUMBER</span> <span class="s1">'12345678901234567890123456789012345678901234567890e30'</span><span class="p">;</span>
</code></pre></div></div>

<div class="language-text highlighter-rouge"><div class="highlight"><pre class="highlight"><code> 1.234567890123456789012345678901234567890123456789E+79
</code></pre></div></div>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- Verify the type</span>
<span class="k">SELECT</span> <span class="n">typeof</span><span class="p">(</span><span class="n">NUMBER</span> <span class="s1">'123.456'</span><span class="p">);</span>
</code></pre></div></div>

<div class="language-text highlighter-rouge"><div class="highlight"><pre class="highlight"><code> number
</code></pre></div></div>

<h3 id="special-values">Special values</h3>

<p>NUMBER also supports special values similar to IEEE 754 floating-point types:</p>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="k">SELECT</span>
  <span class="n">NUMBER</span> <span class="s1">'Infinity'</span> <span class="k">as</span> <span class="n">positive_infinity</span><span class="p">,</span>
  <span class="n">NUMBER</span> <span class="s1">'-Infinity'</span> <span class="k">as</span> <span class="n">negative_infinity</span><span class="p">,</span>
  <span class="n">NUMBER</span> <span class="s1">'NaN'</span> <span class="k">as</span> <span class="n">not_a_number</span><span class="p">;</span>
</code></pre></div></div>

<div class="language-text highlighter-rouge"><div class="highlight"><pre class="highlight"><code> positive_infinity | negative_infinity | not_a_number
-------------------+-------------------+--------------
 +Infinity         | -Infinity         | NaN
</code></pre></div></div>

<p>These special values follow intuitive comparison and ordering semantics that
follow DOUBLE behavior. <code class="language-plaintext highlighter-rouge">NaN</code> compares as inequal to all values, including
itself. Any comparison with <code class="language-plaintext highlighter-rouge">NaN</code> returns false. When sorting, values are
ordered as follows: <code class="language-plaintext highlighter-rouge">-Infinity</code>, all finite values, <code class="language-plaintext highlighter-rouge">+Infinity</code> followed by <code class="language-plaintext highlighter-rouge">NaN</code>.</p>

<p>The special values are particularly useful for handling edge cases in source data.
In particular, PostgreSQL’s NUMERIC type can represent <code class="language-plaintext highlighter-rouge">NaN</code> and <code class="language-plaintext highlighter-rouge">Infinity</code>, and
these values are now seamlessly mapped to NUMBER when queried through the PostgreSQL
connector.</p>

<h2 id="seamless-connector-integration">Seamless connector integration</h2>

<p>The real power of NUMBER becomes apparent when querying external databases. Five
connectors now automatically map high-precision numeric types to NUMBER,
requiring <strong>no configuration changes</strong>:</p>

<h3 id="oracle-connector">Oracle connector</h3>

<p>Oracle’s NUMBER type supports variable precision and scale. The Oracle connector
now maps:</p>

<ul>
  <li><code class="language-plaintext highlighter-rouge">NUMBER(p, s)</code> where p &gt; 38 → Trino <code class="language-plaintext highlighter-rouge">NUMBER</code></li>
  <li><code class="language-plaintext highlighter-rouge">NUMBER</code> without precision/scale → Trino <code class="language-plaintext highlighter-rouge">NUMBER</code></li>
  <li><code class="language-plaintext highlighter-rouge">NUMBER</code> with extreme scale values → Trino <code class="language-plaintext highlighter-rouge">NUMBER</code></li>
</ul>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- Query an Oracle table with high-precision columns</span>
<span class="k">SELECT</span> <span class="n">order_id</span><span class="p">,</span> <span class="n">unit_price</span><span class="p">,</span> <span class="n">extended_price</span>
<span class="k">FROM</span> <span class="n">oracle</span><span class="p">.</span><span class="n">sales</span><span class="p">.</span><span class="n">orders</span>
<span class="k">WHERE</span> <span class="n">extended_price</span> <span class="o">&gt;</span> <span class="n">NUMBER</span> <span class="s1">'1000000000000000000000000'</span><span class="p">;</span>
</code></pre></div></div>

<h3 id="postgresql-connector">PostgreSQL connector</h3>

<p>PostgreSQL’s NUMERIC type supports very high precision and even “unconstrained”
precision. The connector automatically handles:</p>

<ul>
  <li><code class="language-plaintext highlighter-rouge">NUMERIC(p, s)</code> where p &gt; 38 → Trino <code class="language-plaintext highlighter-rouge">NUMBER</code></li>
  <li><code class="language-plaintext highlighter-rouge">NUMERIC</code> without precision/scale → Trino <code class="language-plaintext highlighter-rouge">NUMBER</code></li>
</ul>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- Access PostgreSQL scientific data without precision loss</span>
<span class="k">SELECT</span> <span class="n">measurement_id</span><span class="p">,</span> <span class="n">precise_value</span> <span class="c1">-- a NUMERIC column</span>
<span class="k">FROM</span> <span class="n">postgresql</span><span class="p">.</span><span class="n">lab</span><span class="p">.</span><span class="n">measurements</span>
</code></pre></div></div>

<h3 id="mysql-mariadb-and-singlestore-connectors">MySQL, MariaDB, and SingleStore connectors</h3>

<p>These MySQL-compatible databases support DECIMAL precision up to 65 digits. The
connectors now map:</p>

<ul>
  <li><code class="language-plaintext highlighter-rouge">DECIMAL(p, s)</code> where p &gt; 38 → Trino <code class="language-plaintext highlighter-rouge">NUMBER</code></li>
</ul>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- Join across different databases with high precision</span>
<span class="k">SELECT</span>
  <span class="n">m</span><span class="p">.</span><span class="n">account_id</span><span class="p">,</span>
  <span class="n">m</span><span class="p">.</span><span class="n">balance</span> <span class="k">as</span> <span class="n">mysql_balance</span><span class="p">,</span>
  <span class="n">o</span><span class="p">.</span><span class="n">balance</span> <span class="k">as</span> <span class="n">oracle_balance</span>
<span class="k">FROM</span> <span class="n">mysql</span><span class="p">.</span><span class="n">banking</span><span class="p">.</span><span class="n">accounts</span> <span class="n">m</span>
<span class="k">JOIN</span> <span class="n">oracle</span><span class="p">.</span><span class="n">banking</span><span class="p">.</span><span class="n">accounts</span> <span class="n">o</span> <span class="k">ON</span> <span class="n">m</span><span class="p">.</span><span class="n">account_id</span> <span class="o">=</span> <span class="n">o</span><span class="p">.</span><span class="n">account_id</span>
<span class="k">WHERE</span> <span class="k">abs</span><span class="p">(</span><span class="n">m</span><span class="p">.</span><span class="n">balance</span> <span class="o">-</span> <span class="n">o</span><span class="p">.</span><span class="n">balance</span><span class="p">)</span> <span class="o">&gt;</span> <span class="n">NUMBER</span> <span class="s1">'0.01'</span><span class="p">;</span>
</code></pre></div></div>

<h2 id="backwards-compatibility-and-migration">Backwards compatibility and migration</h2>

<p>The NUMBER type integration is designed to be seamless and backward compatible:</p>

<h3 id="automatic-mapping">Automatic mapping</h3>

<p>If you previously relied on the default behavior (no <code class="language-plaintext highlighter-rouge">decimal-mapping</code>
configuration), your queries now automatically use NUMBER for high-precision
columns. No configuration changes needed.</p>

<h3 id="legacy-configurations-still-work">Legacy configurations still work</h3>

<p>If you explicitly configured <code class="language-plaintext highlighter-rouge">decimal-mapping=ALLOW_OVERFLOW</code> or
<code class="language-plaintext highlighter-rouge">decimal-mapping=STRICT</code>, your existing configuration continues to work. The
NUMBER mapping is disabled when these options are set, ensuring no surprises.</p>

<p>However, the <code class="language-plaintext highlighter-rouge">decimal-mapping</code> configuration and related session properties
(<code class="language-plaintext highlighter-rouge">decimal_mapping</code>, <code class="language-plaintext highlighter-rouge">decimal_default_scale</code>, <code class="language-plaintext highlighter-rouge">decimal_rounding_mode</code>) are now
<strong>deprecated</strong> and will be removed in a future Trino release. We recommend
migrating to NUMBER-based workflows:</p>

<p><strong>Before (with lossy conversion):</strong></p>
<div class="language-properties highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c"># catalog/postgresql.properties
</span><span class="py">connection-url</span><span class="p">=</span><span class="s">jdbc:postgresql://host:5432/database</span>
<span class="py">connection-user</span><span class="p">=</span><span class="s">user</span>
<span class="py">connection-password</span><span class="p">=</span><span class="s">password</span>
<span class="py">decimal-mapping</span><span class="p">=</span><span class="s">ALLOW_OVERFLOW</span>
<span class="py">decimal-default-scale</span><span class="p">=</span><span class="s">10</span>
<span class="py">decimal-rounding-mode</span><span class="p">=</span><span class="s">HALF_UP</span>
</code></pre></div></div>

<p><strong>After (lossless with NUMBER):</strong></p>
<div class="language-properties highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c"># catalog/postgresql.properties
</span><span class="py">connection-url</span><span class="p">=</span><span class="s">jdbc:postgresql://host:5432/database</span>
<span class="py">connection-user</span><span class="p">=</span><span class="s">user</span>
<span class="py">connection-password</span><span class="p">=</span><span class="s">password</span>
<span class="c"># No decimal-mapping needed - NUMBER is used automatically!
</span></code></pre></div></div>

<p>For Oracle, if you previously used <code class="language-plaintext highlighter-rouge">oracle.number.rounding-mode</code> to handle
high-precision NUMBER columns, you can now remove this configuration to enable
native NUMBER mapping.</p>

<h2 id="working-with-number">Working with NUMBER</h2>

<h3 id="type-conversions">Type conversions</h3>

<p>NUMBER integrates naturally with Trino’s type system:</p>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- Convert from other numeric types</span>
<span class="k">SELECT</span>
  <span class="k">CAST</span><span class="p">(</span><span class="nb">DECIMAL</span> <span class="s1">'123.45'</span> <span class="k">AS</span> <span class="n">NUMBER</span><span class="p">)</span> <span class="k">as</span> <span class="n">from_decimal</span><span class="p">,</span>
  <span class="k">CAST</span><span class="p">(</span><span class="mi">12345</span> <span class="k">AS</span> <span class="n">NUMBER</span><span class="p">)</span> <span class="k">as</span> <span class="n">from_integer</span><span class="p">,</span>
  <span class="k">CAST</span><span class="p">(</span><span class="mi">123</span><span class="p">.</span><span class="mi">45</span><span class="n">e0</span> <span class="k">AS</span> <span class="n">NUMBER</span><span class="p">)</span> <span class="k">as</span> <span class="n">from_double</span><span class="p">;</span>
</code></pre></div></div>

<div class="language-text highlighter-rouge"><div class="highlight"><pre class="highlight"><code> from_decimal | from_integer | from_double
--------------+--------------+-------------
 123.45       | 12345        | 123.45
</code></pre></div></div>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- Convert NUMBER to other types</span>
<span class="k">SELECT</span>
  <span class="k">CAST</span><span class="p">(</span><span class="n">NUMBER</span> <span class="s1">'123.456'</span> <span class="k">AS</span> <span class="nb">BIGINT</span><span class="p">)</span> <span class="k">as</span> <span class="n">to_bigint</span><span class="p">,</span>
  <span class="k">CAST</span><span class="p">(</span><span class="n">NUMBER</span> <span class="s1">'123.456'</span> <span class="k">AS</span> <span class="nb">DOUBLE</span><span class="p">)</span> <span class="k">as</span> <span class="n">to_double</span><span class="p">,</span>
  <span class="k">CAST</span><span class="p">(</span><span class="n">NUMBER</span> <span class="s1">'123.456'</span> <span class="k">AS</span> <span class="nb">DECIMAL</span><span class="p">(</span><span class="mi">10</span><span class="p">,</span><span class="mi">2</span><span class="p">))</span> <span class="k">as</span> <span class="n">to_decimal</span><span class="p">;</span>
</code></pre></div></div>

<div class="language-text highlighter-rouge"><div class="highlight"><pre class="highlight"><code> to_bigint | to_double | to_decimal
-----------+-----------+------------
 123       | 123.456   | 123.46
</code></pre></div></div>

<h3 id="aggregate-functions">Aggregate functions</h3>

<p>Common aggregate functions work naturally with NUMBER:</p>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- Aggregate high-precision values</span>
<span class="k">SELECT</span>
  <span class="n">department</span><span class="p">,</span>
  <span class="k">sum</span><span class="p">(</span><span class="n">revenue</span><span class="p">)</span> <span class="k">as</span> <span class="n">total_revenue</span><span class="p">,</span>
  <span class="k">avg</span><span class="p">(</span><span class="n">revenue</span><span class="p">)</span> <span class="k">as</span> <span class="n">average_revenue</span><span class="p">,</span>
  <span class="k">min</span><span class="p">(</span><span class="n">revenue</span><span class="p">)</span> <span class="k">as</span> <span class="n">min_revenue</span><span class="p">,</span>
  <span class="k">max</span><span class="p">(</span><span class="n">revenue</span><span class="p">)</span> <span class="k">as</span> <span class="n">max_revenue</span>
<span class="k">FROM</span> <span class="n">oracle</span><span class="p">.</span><span class="n">sales</span><span class="p">.</span><span class="n">transactions</span>
<span class="k">GROUP</span> <span class="k">BY</span> <span class="n">department</span><span class="p">;</span>
</code></pre></div></div>

<h3 id="creating-tables-with-number-columns">Creating tables with NUMBER columns</h3>

<p>The Oracle and PostgreSQL connectors support creating tables with NUMBER columns:</p>

<div class="language-sql highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c1">-- Create a PostgreSQL table with NUMBER column</span>
<span class="k">CREATE</span> <span class="k">TABLE</span> <span class="n">postgresql</span><span class="p">.</span><span class="k">schema</span><span class="p">.</span><span class="n">measurements</span> <span class="p">(</span>
  <span class="n">id</span> <span class="nb">BIGINT</span><span class="p">,</span>
  <span class="n">precise_value</span> <span class="n">NUMBER</span>
<span class="p">);</span>

<span class="c1">-- Create an Oracle table with NUMBER column</span>
<span class="k">CREATE</span> <span class="k">TABLE</span> <span class="n">oracle</span><span class="p">.</span><span class="k">schema</span><span class="p">.</span><span class="n">scientific_data</span> <span class="p">(</span>
  <span class="n">experiment_id</span> <span class="nb">VARCHAR</span><span class="p">(</span><span class="mi">50</span><span class="p">),</span>
  <span class="n">measurement</span> <span class="n">NUMBER</span>
<span class="p">);</span>
</code></pre></div></div>

<h2 id="technical-characteristics-and-limitations">Technical characteristics and limitations</h2>

<p>While NUMBER provides high precision, it’s important to understand its
characteristics:</p>

<h3 id="precision-and-scale">Precision and scale</h3>

<p>Trino’s NUMBER type characteristics:</p>

<ul>
  <li><strong>Supported precision</strong>: currently 200 decimal digits.
While we consider this an implementation detail that may change in future releases,
it is unlikely that maximum precision will be decreased.</li>
  <li><strong>Scale range</strong>: -16,384 to 16,383</li>
  <li><strong>Variable scale</strong>: each value can have a different scale, similar to
PostgreSQL NUMERIC and Oracle NUMBER</li>
  <li><strong>Special values</strong>: supports <code class="language-plaintext highlighter-rouge">NaN</code>, <code class="language-plaintext highlighter-rouge">Infinity</code>, and <code class="language-plaintext highlighter-rouge">-Infinity</code></li>
</ul>

<p>Comparison of decimal numeric types across database systems:</p>

<table>
  <thead>
    <tr>
      <th>Database</th>
      <th>Max Precision</th>
      <th>Scale Range</th>
      <th>Variable Scale</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Oracle NUMBER(p, s)</td>
      <td>38</td>
      <td>-84 to 127</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Oracle NUMBER</td>
      <td>40</td>
      <td>Approximately -130 to 126</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>PostgreSQL NUMERIC(p, s)</td>
      <td>38</td>
      <td>-1000 to 1000</td>
      <td>No</td>
    </tr>
    <tr>
      <td>PostgreSQL NUMERIC</td>
      <td>131,072</td>
      <td>-1000 to 1000</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>MySQL/MariaDB/SingleStore DECIMAL</td>
      <td>65</td>
      <td>0 to 30</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Trino DECIMAL</td>
      <td>38</td>
      <td>0 to 38</td>
      <td>No</td>
    </tr>
    <tr>
      <td><strong>Trino NUMBER</strong></td>
      <td><strong>200</strong></td>
      <td><strong>-16,384 to 16,383</strong></td>
      <td><strong>Yes</strong></td>
    </tr>
  </tbody>
</table>

<h3 id="storage-and-representation">Storage and representation</h3>

<p>NUMBER uses a variable-width binary format optimized for flexibility:</p>
<ul>
  <li>2-byte header encoding sign and scale</li>
  <li>Variable-length magnitude in big-endian format</li>
  <li>The binary format is considered unstable and may evolve in future releases to
enable optimizations and performance improvements</li>
</ul>

<p>This flexibility allows Trino to improve NUMBER’s internal representation over
time without breaking connector compatibility.
Trino SPI provides a stable API for connectors to read and write NUMBER values,
abstracting away the internal format.</p>

<h3 id="performance-considerations">Performance considerations</h3>

<p>NUMBER uses Java’s BigDecimal for arithmetic operations, which provides exact
precision at the cost of being slower than fixed-precision types like BIGINT,
DOUBLE or DECIMAL. For this reason, NUMBER is designed for scenarios where
precision is more important than computational speed:</p>

<ul>
  <li><strong>Best for</strong>: reading and storing high-precision data from source systems,
data federation, reporting, data warehousing</li>
  <li><strong>Not optimal for</strong>: computational heavy-lifting, complex mathematical
operations, high-performance analytics on numeric columns</li>
</ul>

<p>If your workload involves extensive numeric computation, consider whether DECIMAL
(for up to 38 digits), DOUBLE (for approximate arithmetic), or BIGINT (for
integer arithmetic) might be more appropriate.</p>

<h3 id="function-support">Function support</h3>

<p>NUMBER supports essential operations:</p>
<ul>
  <li>Arithmetic: <code class="language-plaintext highlighter-rouge">+</code>, <code class="language-plaintext highlighter-rouge">-</code>, <code class="language-plaintext highlighter-rouge">*</code>, <code class="language-plaintext highlighter-rouge">/</code></li>
  <li>Aggregations: <code class="language-plaintext highlighter-rouge">sum()</code>, <code class="language-plaintext highlighter-rouge">avg()</code>, <code class="language-plaintext highlighter-rouge">min()</code>, <code class="language-plaintext highlighter-rouge">max()</code></li>
  <li>Rounding functions: <code class="language-plaintext highlighter-rouge">abs()</code>, <code class="language-plaintext highlighter-rouge">sign()</code>, <code class="language-plaintext highlighter-rouge">ceiling()</code>, <code class="language-plaintext highlighter-rouge">floor()</code>, <code class="language-plaintext highlighter-rouge">truncate()</code>,
<code class="language-plaintext highlighter-rouge">round()</code></li>
  <li>Special value checks: <code class="language-plaintext highlighter-rouge">is_nan()</code>, <code class="language-plaintext highlighter-rouge">is_finite()</code>, <code class="language-plaintext highlighter-rouge">is_infinite()</code></li>
</ul>

<p>Many advanced mathematical functions (trigonometric, logarithmic, etc.)
do not work with NUMBER directly and require explicit type conversions to DOUBLE or DECIMAL.</p>

<h2 id="whats-next">What’s next</h2>

<p>The NUMBER type support will continue to evolve. Additional connectors are
planned for future releases:</p>

<ul>
  <li><strong>ClickHouse</strong>: for Decimal256 type mapping</li>
  <li><strong>Apache Ignite</strong>: for high-precision numeric support</li>
</ul>

<p>We’re also exploring performance optimizations and expanding function support
based on community feedback.</p>

<h2 id="getting-started">Getting started</h2>

<p>NUMBER support is available now in Trino 480. To start using it:</p>

<ol>
  <li><strong>Upgrade to Trino 480</strong> - NUMBER is available out of the box</li>
  <li><strong>Remove deprecated configs</strong> - If you used <code class="language-plaintext highlighter-rouge">decimal-mapping</code> configurations,
consider removing them to enable automatic NUMBER mapping</li>
  <li><strong>Query your data</strong> - High-precision columns are now accessible without
configuration</li>
</ol>

<p>For detailed documentation, refer to:</p>
<ul>
  <li><a href="https://trino.io/docs/current/language/types.html">NUMBER type reference</a></li>
  <li><a href="https://trino.io/docs/current/connector/oracle.html">Oracle connector documentation</a></li>
  <li><a href="https://trino.io/docs/current/connector/postgresql.html">PostgreSQL connector documentation</a></li>
  <li><a href="https://trino.io/docs/current/connector/mysql.html">MySQL connector documentation</a></li>
  <li><a href="https://trino.io/docs/current/connector/mariadb.html">MariaDB connector documentation</a></li>
  <li><a href="https://trino.io/docs/current/connector/singlestore.html">SingleStore connector documentation</a></li>
</ul>

<p>Have questions or feedback? Join the discussion on the <a href="https://trino.io/slack.html">Trino community
Slack</a> in the <code class="language-plaintext highlighter-rouge">#dev</code> channel, or open an issue on
<a href="https://github.com/trinodb/trino/issues">GitHub</a>.</p>

<p>The NUMBER type represents a significant milestone in Trino’s evolution,
eliminating precision loss barriers and making high-precision numeric data from
diverse sources readily accessible for analytics and reporting. We’re excited to
see how the community uses this powerful new capability!</p>

<p>□</p>
