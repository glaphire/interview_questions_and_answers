# Redis VS Memcached

<table border=1 cellpadding=5>
    <tr>
        <th> </th>
        <th>Memcached</th>
        <th>Redis</th>
    </tr>
	<tr>
		<td>Created in</td>
		<td>2009</td>
		<td>2009</td>
	</tr>
	<tr>
		<td>Value cached in</td>
		<td>strings</td>
		<td>
			<ul>
				<li>strings</li>
				<li>lists</li>
				<li>hashes</li>
				<li>sets</li>
				<li>sorted sets</li>
				<li>Bitmaps</li>
				<li>HyperLogLogs</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>Complex data storing</td>
		<td>needs serialization/deserialization</td>
		<td>serialization isn't needed if suitable datatype exists</td>
	</tr>
	<tr>
		<td>Best for</td>
		<td>Saving small string values (e.g. small pieces for HTML code)</td>
		<td>All kind of data types (fine tuning for each case)</td>
	</tr>
	<tr>
		<td>Works as</td>
		<td>Multi-thread process</td>
		<td>Single-thread process</td>
	</tr>
	<tr>
		<td>Max key length</td>
		<td>250 bytes</td>
		<td>512 megabytes (same vof value)</td>
	</tr>
	<tr>
		<td>Binary protocol support</td>
		<td>Yes (if compiled with a flag)</td>
		<td>Yes</td>
	</tr>
	<tr>
		<td>Eviction policies' characteristics</td>
		<td>Memory defragmentation on fast changing data (affects performance).
		"Last recent used" policy isn't fully straightforward - old data with similar size is replaced by the new one.</td>
		<td>Several policies. <a href="https://redis.io/topics/lru-cache">Eviction policies</a></td>
	</tr>
	<tr>
		<td>RAM usage</td>
		<td>Relatively small due to short keys and values and only one data type</td>
		<td>Relatively big due to metadata for handling different data types</td>
	</tr>
	<tr>
		<td>Monitoring/Tuning</td>
		<td>Bad tuning due to limited configuration options. Mostly outdated monitoring tools</td>
		<td>Great support - fine tuning thanks to big list of commands, several tools for monitoring</td>
	</tr>
</table>

\* Eviction policies - политики вытеснения (данных в кеше)