<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Websiter - The Better Search</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600&family=Roboto&display=swap" rel="stylesheet">
  <link rel="icon" href="https://www.google.com/favicon.ico" />

  <!-- Google Custom Search Engine API -->
  <script async src="https://cse.google.com/cse.js?cx=d1098bbbe05e44c2b"></script>
  <div class="gcse-search"></div>

  <style>
    :root {
      --bg-color: #121212;
      --text-color: #e0e0e0;
      --accent-color: #00e5ff;
      --card-bg: rgba(255, 255, 255, 0.05);
      --glass-blur: blur(20px);
      --transition: 0.3s ease-in-out;
      --shadow: 0 8px 32px rgba(0, 0, 0, 0.25);
    }

    body {
      margin: 0;
      font-family: 'Roboto', sans-serif;
      background-color: var(--bg-color);
      color: var(--text-color);
      transition: var(--transition);
      overflow-x: hidden;
    }

    header {
      text-align: center;
      padding: 2rem;
      background: linear-gradient(135deg, #000, #222);
      box-shadow: var(--shadow);
      border-bottom: 2px solid var(--accent-color);
    }

    header h1 {
      font-family: 'Orbitron', sans-serif;
      color: var(--accent-color);
      font-size: 3rem;
      margin: 0;
    }

    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem;
      max-width: 1200px;
      margin: auto;
    }

    .gcse-search {
      width: 100%;
      max-width: 800px;
      background: var(--card-bg);
      padding: 2rem;
      border-radius: 12px;
      backdrop-filter: var(--glass-blur);
      box-shadow: var(--shadow);
    }

    .search-tips {
      margin-top: 2rem;
      font-size: 0.9rem;
      opacity: 0.8;
      max-width: 800px;
    }

    /* Sidebar for buttons */
    .sidebar {
      position: fixed;
      top: 10%;
      left: 0;
      padding: 1rem;
      background-color: rgba(0, 0, 0, 0.5);
      box-shadow: var(--shadow);
      border-radius: 10px 0 0 10px;
      z-index: 999;
      width: 180px;
    }

    .sidebar button {
      width: 100%;
      padding: 0.8rem;
      margin-bottom: 1rem;
      font-size: 1rem;
      text-align: left;
      color: #fff;
      background-color: var(--accent-color);
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: var(--transition);
    }

    .sidebar button:hover {
      background-color: #00b8d4;
    }

    /* Settings Modal */
    .settings-modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      z-index: 1000;
      padding: 2rem;
      overflow: auto;
    }

    .settings-content {
      background: var(--card-bg);
      padding: 2rem;
      border-radius: 8px;
      box-shadow: var(--shadow);
      width: 100%;
      max-width: 400px;
      margin: auto;
    }

    .settings-content select, .settings-content input {
      margin-bottom: 1rem;
      width: 100%;
      padding: 0.5rem;
      border-radius: 8px;
      border: 1px solid #ddd;
    }

    footer {
      margin-top: 4rem;
      padding: 2rem;
      text-align: center;
      font-size: 0.9rem;
      color: #aaa;
    }
  </style>
</head>
<body>

  <!-- Sidebar for buttons -->
  <div class="sidebar">
    <button class="toggle-btn" onclick="toggleTheme()">Toggle Theme</button>
    <button class="voice-btn" onclick="startVoiceSearch()">Voice Search</button>
    <button class="settings-btn" onclick="openSettings()">Settings</button>
    <button class="ai-btn" onclick="startAISearch()">AI Search</button>
    <button class="google-btn" onclick="openGoogleDocs()">Google Docs</button>
    <button class="google-btn" onclick="openGoogleSheets()">Google Sheets</button>
    <button class="google-btn" onclick="openGoogleGmail()">Gmail</button>
  </div>

  <!-- Header -->
  <header class="fade-in">
    <h1>🌐 Websiter 🌐</h1>
    <p>The Better Search Engine, Powered by Google</p>
  </header>

  <!-- Main Container -->
  <div class="container fade-in">
    <div class="gcse-search"></div>

    <div class="search-tips">
      <h3>🔍 Pro Search Tips:</h3>
      <ul>
        <li>Use quotes to search exact phrases: <code>nik</code></li>
        <li>Exclude words with minus: e<code>
            xcfvghjklkijuhytredtyuiuydxdfyuhgfcxfu
        </code></li>
        <li>Site search: <code>site:github.com machine learning</code></li>
        <li>Wildcard search: <code>best * apps 2025</code></li>
      </ul>
    </div>

    <div class="ai-results" id="ai-results"></div>
  </div>

  <footer>
    &copy; <span id="year"></span> Websiter. Built with ☕ + 🤖
  </footer>

  <!-- Settings Modal -->
  <div class="settings-modal" id="settingsModal">
    <div class="settings-content">
      <h3>Settings</h3>
      <label for="font-select">Font:</label>
      <select id="font-select" onchange="changeFont(this)">
        <option value="Roboto">Roboto</option>
        <option value="Arial">Arial</option>
        <option value="Courier New">Courier New</option>
      </select>

      <label for="bg-select">Background Color:</label>
      <select id="bg-select" onchange="changeBackground(this)">
        <option value="#121212">Dark</option>
        <option value="#ffffff">Light</option>
        <option value="#e0e0e0">Gray</option>
      </select>

      <button onclick="closeSettings()">Close</button>
    </div>
  </div>

  <script>
    let darkMode = true;

    // Set dynamic year
    document.getElementById('year').textContent = new Date().getFullYear();

    // Toggle Theme
    function toggleTheme() {
      darkMode = !darkMode;
      document.body.style.setProperty('--bg-color', darkMode ? '#121212' : '#ffffff');
      document.body.style.setProperty('--text-color', darkMode ? '#e0e0e0' : '#111');
      document.body.style.setProperty('--card-bg', darkMode ? 'rgba(255, 255, 255, 0.05)' : 'rgba(0, 0, 0, 0.05)');
    }

    // Open Settings
    function openSettings() {
      document.getElementById('settingsModal').style.display = 'block';
    }

    // Close Settings
    function closeSettings() {
      document.getElementById('settingsModal').style.display = 'none';
    }

    // Change Font
    function changeFont(select) {
      document.body.style.fontFamily = select.value;
    }

    // Change Background
    function changeBackground(select) {
      document.body.style.backgroundColor = select.value;
    }

    // Start Voice Search (Placeholder)
    function startVoiceSearch() {
      alert('Voice search is under development.');
    }

    // Start AI Search
    function startAISearch() {
      const query = prompt('Ask your AI Search question:');
      if (query) {
        const aiResults = document.getElementById('ai-results');
        aiResults.innerHTML = `<p><strong>AI Answer:</strong> Searching for results related to "${query}"...</p>`;
        const geminiAPIKey = 'YOUR_GEMINI_API_KEY';  // Replace with your actual API key

        fetch('https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=' + geminiAPIKey, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            "contents": [{ "parts": [{ "text": query }] }]
          })
        })
        .then(response => response.json())
        .then(data => {
          const aiResponse = data.generatedContent[0].parts[0].text;
          aiResults.innerHTML = `<p><strong>AI Answer:</strong> ${aiResponse}</p>`;
        })
        .catch(error => {
          aiResults.innerHTML = '<p>AI search failed. Please try again.</p>';
        });
      }
    }

    // Open Google Docs
    function openGoogleDocs() {
      window.open('https://docs.google.com', '_blank');
    }

    // Open Google Sheets

    function openGoogleSheets() {
      window.open('https://sheets.google.com', '_blank');
    }

    // Open Gmail
    function openGoogleGmail() {
      window.open('https://mail.google.com', '_blank');
    }
  </script>

</body>
</html>
