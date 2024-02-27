
<body>
  <h1>Torrent Network Simulation</h1>

  <p>This project provides a simulation of a Torrent network with limited features and capabilities. It enables users to share and request files within a simulated peer-to-peer network environment.</p>

  <h2>Overview</h2>

  <p>The simulation includes a tracker component responsible for managing the network and facilitating file sharing among peers. Each peer client can either share a file or request one from the network. The tracker and peers maintain comprehensive logs, accessible via command line queries.</p>

  <h2>Usage</h2>

  <h3>Tracker</h3>

  <p>To run the tracker, execute the following command:</p>

  <pre><code>python Tracker.py trackerIp:trackerPort</code></pre>

  <p>Replace <code>trackerIp</code> and <code>trackerPort</code> with the desired IP address and port number for the tracker.</p>

  <h3>Peer</h3>

  <p>To share a file, use the following command format:</p>

  <pre><code>python Peer.py share file_name trackerIp:trackerPort sharingIP:sharingPort</code></pre>

  <p>Replace <code>file_name</code>, <code>trackerIp</code>, <code>trackerPort</code>, <code>sharingIP</code>, and <code>sharingPort</code> with the appropriate values.</p>

  <p>To request a file, use the following command format:</p>

  <pre><code>python Peer.py get file_name trackerIp:trackerPort gettingIP:gettingPort</code></pre>

  <p>Replace <code>file_name</code>, <code>trackerIp</code>, <code>trackerPort</code>, <code>gettingIP</code>, and <code>gettingPort</code> with the appropriate values.</p>

  <h3>Logging</h3>

  <p>Logs can be accessed via command line queries. For example:</p>

  <ul>
    <li>To view logs for a specific file: <code>&gt;file_logs&gt;file_name</code></li>
    <li>To request all logs: <code>&gt;request all logs</code></li>
  </ul>

  <h2>Workflow</h2>

  <ol>
    <li>Upon running <code>Peer.py get</code>, the peer sends a TCP request to the tracker.</li>
    <li>If the file exists in the tracker, it responds with the sharingIP and sharingPort.</li>
    <li>The first and second peers establish a connection for file sharing.</li>
    <li>The downloaded file is stored in the getter peer's directory.</li>
    <li>The getter peer transitions to a sharer peer, providing the downloaded file as a torrent file in the network for other peers to access.</li>
  </ol>

  <h2>License</h2>

  <p>This project is licensed under the MIT License - see the <a href="LICENSE">LICENSE</a> file for details.</p>
</body>
</html>
