<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Simulating a Social Engineering Attack – Rona Playda</title>
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <style>
    body{background:#0d1117;color:#e6edf3;font-family:system-ui,Arial,sans-serif;padding:20px;line-height:1.6}
    h1{color:#58a6ff} a{color:#79c0ff}
    pre{background:#161b22;padding:12px;border-radius:8px;border:1px solid #30363d;overflow:auto}
    section{margin-top:18px}
    figure{margin:18px 0}
    img{max-width:100%;height:auto;border-radius:6px;border:1px solid #222}
    figcaption{font-size:0.9rem;color:#9fb7d6}
  </style>
</head>
<body>
  <h1>Simulating a Social Engineering Attack Using SET</h1>

  <p><strong>Objective:</strong> Demonstrate credential-harvesting techniques in a controlled Kali Linux lab environment and review mitigation strategies.</p>

  <section>
    <h2>Legal / Ethics &amp; Safety</h2>
    <p><strong>Warning:</strong> This exercise is strictly for educational, lab-only use. Do not run these techniques against systems you do not own or have explicit written permission to test. Unauthorized social-engineering attacks are illegal and unethical. All IP addresses, usernames, and credentials have been intentionally redacted from the examples and screenshots here.</p>
  </section>

  <section>
    <h2>Prerequisites &amp; Lab Setup</h2>
    <ul>
      <li><strong>Attacker VM:</strong> Kali Linux (latest), Social-Engineer Toolkit (setoolkit)</li>
      <li><strong>Victim VM:</strong> Windows 10 (or any browser-enabled VM) on the same isolated virtual network (Host-only or NAT with port forwarding).</li>
      <li><strong>Network mode:</strong> Use an isolated virtual network (no Internet access) to avoid accidentally exposing services.</li>
      <li><strong>Resources:</strong> 2–4 GB RAM per VM, snapshots enabled for rollback.</li>
    </ul>
  </section>

  <h2>Steps (summary)</h2>
  <ol>
    <li><strong>Stop Apache:</strong> <pre>service apache2 stop</pre></li>
    <li><strong>Start SET:</strong> <pre>setoolkit</pre> → choose Social-Engineering Attacks → Website Attack Vectors → Credential Harvester → Web Templates.</li>
    <li><strong>Get attacker IP (lab only):</strong> <pre>ifconfig eth0</pre> — <em>Do not publish real IPs.</em></li>
    <li>Paste the lab IP into SET when prompted and start the hosted page.</li>
    <li>On a test machine, open <code>http://[Your Attack IP Address]</code> and submit <strong>dummy</strong> credentials to test capture.</li>
    <li>Observe captured credentials in the SET terminal (lab only).</li>
  </ol>

  <section>
    <h2>Commands &amp; Expected Output</h2>
    <pre>service apache2 stop
setoolkit
# Follow menus: 1 → 2 → 3 → 1
ifconfig eth0  # copy the inet address, but DO NOT publish it</pre>
    <p>Example terminal output (redacted for public reports):</p>
    <pre>[*] Credentials captured:
Username: [REDACTED]
Password: [REDACTED]</pre>
  </section>

  <section>
    <h2>Evidence (redacted screenshots)</h2>

    <figure>
      <img src="../images/kali-attack Red Team simulation-2025-10-16-20-06-46-redacted.jpg" alt="SET main menu (redacted)" />
      <figcaption>SET main menu. Sensitive fields and IPs are blurred.</figcaption>
    </figure>

    <figure>
      <img src="../images/kali-attack Red Team simulation-2025-10-16-20-13-35-redacted.jpg" alt="SET web attack menu (redacted)" />
      <figcaption>Web attack selection menu (redacted).</figcaption>
    </figure>

    <figure>
      <img src="../images/kali-attack Red Team simulation-2025-10-16-20-14-02-redacted.jpg" alt="SET attack details (redacted)" />
      <figcaption>Details about the credential harvester and attack vectors (redacted).</figcaption>
    </figure>

    <figure>
      <img src="../images/kali-attack Red Team simulation-2025-10-16-20-14-38-redacted.jpg" alt="SET cloning process (redacted)" />
      <figcaption>Cloning a target site for credential harvesting (redacted).</figcaption>
    </figure>

    <figure>
      <img src="../images/kali-attack Red Team simulation-2025-10-16-20-22-04-redacted.jpg" alt="SET final messages (redacted)" />
      <figcaption>SET terminal showing final messages (redacted).</figcaption>
    </figure>

  </section>

  <section>
    <h2>Artifacts &amp; Evidence</h2>
    <ul>
      <li>SET terminal output (redacted)</li>
      <li>Apache access logs and timestamps</li>
      <li>PCAP of the test traffic (tcpdump -w capture.pcap)</li>
      <li>Hashes of any published logs (sha256sum)</li>
    </ul>
  </section>

  <section>
    <h2>Detection / SIEM Example</h2>
    <p>Example Kibana query to look for unusual POSTs to new endpoints:</p>
    <pre>event.module:apache AND http.request.method:POST AND NOT url.path:/login</pre>
    <p>Alert idea: trigger when &gt;10 POSTs to a non-auth endpoint from a single source within 5 minutes.</p>
  </section>

  <section>
    <h2>Mitigations</h2>
    <ul>
      <li>Enable multi-factor authentication (MFA).</li>
      <li>Run phishing simulations and user training.</li>
      <li>Deploy URL filtering and web-blocking for known phishing hosts.</li>
      <li>Monitor webserver logs and alert on unusual POST activity.</li>
    </ul>
  </section>

  <h2>Key Findings</h2>
  <ul>
    <li>Cloned pages can harvest credentials if users are tricked.</li>
    <li>Captured inputs appear in the SET terminal in the lab environment.</li>
  </ul>

  <p><em>Security note:</em> Real credentials and IPs are intentionally redacted from this public report. For authorized review contact <a href="mailto:ronaplayda4@gmail.com">ronaplayda4@gmail.com</a>.</p>

  <p><a href="../index.html">← Back to Home</a></p>
</body>
</html>
