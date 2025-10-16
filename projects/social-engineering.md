<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Social Engineering Lab – Rona Playda</title>
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <style>
    body{background:#0d1117;color:#e6edf3;font-family:system-ui,Arial,sans-serif;padding:20px;line-height:1.6}
    h1{color:#58a6ff} a{color:#79c0ff}
    pre{background:#161b22;padding:12px;border-radius:8px;border:1px solid #30363d;overflow:auto}
  </style>
</head>
<body>
  <h1>Simulating a Social Engineering Attack Using SET</h1>

  <p><strong>Objective:</strong> Demonstrate credential-harvesting techniques in a controlled Kali Linux lab environment and review mitigation strategies.</p>

  <h2>Steps (summary)</h2>
  <ol>
    <li><strong>Stop Apache:</strong> <pre>service apache2 stop</pre></li>
    <li><strong>Start SET:</strong> <pre>setoolkit</pre> → choose Social-Engineering Attacks → Website Attack Vectors → Credential Harvester → Web Templates.</li>
    <li><strong>Get attacker IP (lab only):</strong> <pre>ifconfig eth0</pre> — <em>Do not publish real IPs.</em></li>
    <li>Paste the lab IP into SET when prompted and start the hosted page.</li>
    <li>On a test machine, open <code>http://[Your Attack IP Address]</code> and submit **dummy** credentials to test capture.</li>
    <li>Observe captured credentials in the SET terminal (lab only).</li>
  </ol>

  <h2>Key Findings</h2>
  <ul>
    <li>Cloned pages can harvest credentials if users are tricked.</li>
    <li>Captured inputs are displayed on the attacker terminal in the lab environment.</li>
  </ul>

  <h2>Mitigations</h2>
  <ul>
    <li>User training and phishing simulations</li>
    <li>Multi-factor authentication (MFA)</li>
    <li>Network monitoring and alerting</li>
  </ul>

  <p><em>Security note:</em> Real credentials and IPs are intentionally redacted from this public report. For an authorized copy, contact <a href="mailto:ronaplayda4@gmail.com">ronaplayda4@gmail.com</a>.</p>

  <p><a href="../index.html">← Back to Home</a></p>
</body>
</html>
