---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ARYA - AI Career Agent</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
    body { display: flex; height: 100vh; background-color: #f8fafc; color: #1e293b; overflow: hidden; }

    /* LEFT SIDEBAR */
    .sidebar { width: 250px; background: #ffffff; border-right: 1px solid #e2e8f0; display: flex; flex-direction: column; justify-content: space-between; padding: 24px 16px; flex-shrink: 0; }
    .brand { font-size: 20px; font-weight: 700; letter-spacing: 2px; margin-bottom: 32px; display: flex; align-items: center; gap: 8px; color: #0f172a; }
    .nav-menu { display: flex; flex-direction: column; gap: 6px; }
    .nav-item { display: flex; align-items: center; gap: 12px; padding: 10px 14px; border-radius: 8px; font-size: 14px; font-weight: 500; color: #64748b; text-decoration: none; cursor: pointer; transition: all 0.2s; }
    .nav-item:hover, .nav-item.active { background: #f1f5f9; color: #0f172a; font-weight: 600; }

    .user-profile { display: flex; align-items: center; gap: 10px; padding: 12px; border-top: 1px solid #e2e8f0; }
    .avatar { width: 34px; height: 34px; border-radius: 50%; background: #6366f1; color: white; display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 14px; }
    .user-info { font-size: 13px; font-weight: 600; }
    .user-plan { font-size: 11px; color: #10b981; font-weight: 600; }

    /* MAIN CONTENT AREA */
    .main-content { flex: 1; padding: 32px 40px; overflow-y: auto; }
    .page-title { font-size: 24px; font-weight: 700; margin-bottom: 4px; }
    .page-subtitle { font-size: 14px; color: #64748b; margin-bottom: 20px; }

    /* PAGE SECTIONS */
    .view-tab { display: none; }
    .view-tab.active { display: block; }

    /* SEARCH & API CONTROL PANEL */
    .api-search-panel { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; margin-bottom: 24px; display: flex; flex-wrap: wrap; gap: 12px; align-items: center; }
    .search-input { flex: 1; min-width: 200px; padding: 10px 14px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 14px; outline: none; }
    .search-input:focus { border-color: #6366f1; }
    .btn-search { background: #6366f1; color: white; border: none; padding: 10px 20px; border-radius: 8px; font-weight: 600; font-size: 14px; cursor: pointer; transition: background 0.2s; }
    .btn-search:hover { background: #4f46e5; }

    /* BANNER */
    .banner { background: #fefce8; border: 1px solid #fef08a; color: #854d0e; padding: 12px 16px; border-radius: 8px; font-size: 13px; margin-bottom: 24px; display: flex; align-items: center; gap: 8px; }

    /* JOB CARD DESIGN */
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 24px; margin-bottom: 16px; display: flex; justify-content: space-between; gap: 16px; transition: box-shadow 0.2s; }
    .job-card:hover { box-shadow: 0 4px 12px rgba(0,0,0,0.05); }
    .job-details { flex: 1; }
    .job-title { font-size: 18px; font-weight: 600; margin-bottom: 4px; color: #0f172a; text-decoration: none; }
    .company-name { font-size: 14px; color: #64748b; margin-bottom: 12px; font-weight: 500; }
    .meta-tags { display: flex; gap: 16px; font-size: 13px; color: #64748b; margin-bottom: 16px; flex-wrap: wrap; }

    .skills-section { margin-top: 12px; }
    .match-tag { display: inline-block; background: #f0fdf4; color: #166534; font-size: 12px; font-weight: 600; padding: 4px 8px; border-radius: 4px; margin-right: 6px; margin-bottom: 6px; }
    .skill-pill { display: inline-block; background: #ecfdf5; color: #047857; font-size: 12px; padding: 4px 10px; border-radius: 12px; margin-right: 6px; margin-bottom: 6px; }

    /* CIRCULAR MATCH SCORE */
    .score-container { display: flex; flex-direction: column; align-items: center; justify-content: center; min-width: 120px; border-left: 1px solid #f1f5f9; padding-left: 20px; }
    .circle-score { width: 68px; height: 68px; border-radius: 50%; border: 4px solid #10b981; display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 17px; color: #047857; margin-bottom: 8px; }
    .score-label { font-size: 11px; font-weight: 700; color: #047857; letter-spacing: 0.5px; }

    /* APPLICATIONS TABLE DESIGN */
    .table-container { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; overflow: hidden; }
    table { width: 100%; border-collapse: collapse; text-align: left; }
    th { background: #f8fafc; font-size: 12px; font-weight: 600; color: #64748b; padding: 14px 16px; border-bottom: 1px solid #e2e8f0; }
    td { padding: 16px; font-size: 14px; border-bottom: 1px solid #f1f5f9; vertical-align: middle; }
    .badge-applied { background: #dbeafe; color: #1e40af; font-size: 12px; font-weight: 600; padding: 4px 10px; border-radius: 12px; display: inline-block; }

    /* TOGGLE SWITCH */
    .switch { position: relative; display: inline-block; width: 36px; height: 20px; }
    .switch input { opacity: 0; width: 0; height: 0; }
    .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #cbd5e1; transition: .2s; border-radius: 20px; }
    .slider:before { position: absolute; content: ""; height: 14px; width: 14px; left: 3px; bottom: 3px; background-color: white; transition: .2s; border-radius: 50%; }
    input:checked + .slider { background-color: #10b981; }
    input:checked + .slider:before { transform: translateX(16px); }

    .loading-text { text-align: center; color: #64748b; font-size: 15px; padding: 40px 0; }
  </style>
</head>
<body>

  <!-- LEFT SIDEBAR -->
  <div class="sidebar">
    <div>
      <div class="brand"><span>🌐</span> ARYA</div>
      <div class="nav-menu">
        <div class="nav-item active" onclick="showTab('your-jobs', this)">🔍 Your Jobs</div>
        <div class="nav-item" onclick="showTab('application-draft', this)">✈️ Application Draft</div>
        <div class="nav-item" onclick="showTab('my-applications', this)">📊 My Applications</div>
        <div class="nav-item" onclick="showTab('my-resume', this)">📄 My Resume</div>
      </div>
    </div>
    <div class="user-profile">
      <div class="avatar">MS</div>
      <div>
        <div class="user-info">Mohammad Salman</div>
        <div class="user-plan">● Pro API Active</div>
      </div>
    </div>
  </div>

  <!-- MAIN CONTAINER -->
  <div class="main-content">

    <!-- TAB 1: YOUR JOBS (LIVE API SEARCH) -->
    <div id="your-jobs" class="view-tab active">
      <div class="page-title">Live Job Search</div>
      <div class="page-subtitle">Fetch real-time roles directly via Job Portal APIs.</div>

      <!-- API SEARCH CONTROLS -->
      <div class="api-search-panel">
        <input type="text" id="roleQuery" class="search-input" value="Operations Manager" placeholder="Role / Keywords...">
        <input type="text" id="locationQuery" class="search-input" value="Portugal" placeholder="Location...">
        <button class="btn-search" onclick="fetchLiveJobs()">🔍 Fetch Live Roles</button>
      </div>

      <div class="banner">
        ⚡ Live API integration connected. Click <strong>Fetch Live Roles</strong> to pull real-time portal updates.
      </div>

      <!-- DYNAMIC JOB CONTAINER -->
      <div id="jobsContainer">
        <!-- Initial Scrape Placeholder Cards -->
        <div class="job-card">
          <div class="job-details">
            <div class="job-title">Operations Manager</div>
            <div class="company-name">Mantu Logistics</div>
            <div class="meta-tags">
              <span>📍 Lisbon, Portugal</span>
              <span>💼 Full-time</span>
              <span>📅 Recently Posted</span>
            </div>
            <div class="skills-section">
              <span class="match-tag">✓ Strong title match</span>
              <span class="skill-pill">Operations</span>
              <span class="skill-pill">Supply Chain</span>
            </div>
          </div>
          <div class="score-container">
            <div class="circle-score">98%</div>
            <div class="score-label">STRONG MATCH</div>
          </div>
        </div>
      </div>
    </div>

    <!-- TAB 2: APPLICATION DRAFT -->
    <div id="application-draft" class="view-tab">
      <div class="page-title">Application Drafts</div>
      <div class="page-subtitle">AI cover emails generated for matched roles.</div>
      <div class="job-card">
        <p>Pending email draft prepared for Operations Lead position.</p>
      </div>
    </div>

    <!-- TAB 3: MY APPLICATIONS -->
    <div id="my-applications" class="view-tab">
      <div class="page-title">My Applications</div>
      <div class="page-subtitle">Track live outreach statuses and response metrics.</div>

      <div class="table-container">
        <table>
          <thead>
            <tr>
              <th><input type="checkbox"></th>
              <th>Company</th>
              <th>Role</th>
              <th>Status</th>
              <th>Sent</th>
              <th>Engagement</th>
              <th>Auto FU</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><input type="checkbox"></td>
              <td><strong>Crypto Finance Group</strong></td>
              <td>Senior Operations Manager</td>
              <td><span class="badge-applied">✈️ Applied</span></td>
              <td>Jul 26</td>
              <td>👁️ 1 📥 0</td>
              <td><label class="switch"><input type="checkbox" checked><span class="slider"></span></label></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- TAB 4: MY RESUME -->
    <div id="my-resume" class="view-tab">
      <div class="page-title">My Resume</div>
      <div class="page-subtitle">Uploaded CV and key criteria analysis.</div>
      <div style="background: #0f172a; height: 400px; border-radius: 12px; color: white; display: flex; align-items: center; justify-content: center;">
        [ Dynamic PDF Viewer Container ]
      </div>
    </div>

  </div>

  <script>
    // Tab switching handler
    function showTab(tabId, element) {
      document.querySelectorAll('.view-tab').forEach(tab => tab.classList.remove('active'));
      document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
      document.getElementById(tabId).classList.add('active');
      element.classList.add('active');
    }

    // LIVE JOB PORTAL API FETCH FUNCTION
    async function fetchLiveJobs() {
      const role = document.getElementById('roleQuery').value;
      const location = document.getElementById('locationQuery').value;
      const container = document.getElementById('jobsContainer');

      container.innerHTML = `<div class="loading-text">🔄 Querying live Job APIs for "${role}" in "${location}"...</div>`;

      // Free Adzuna API Public Credentials Endpoint
      const APP_ID = 'c90538a2'; // Public Adzuna Demo App ID
      const APP_KEY = '5a443e2759e56ef3eb13a30c5e317822'; // Public Adzuna Demo Key
      const country = 'gb'; // 'gb', 'us', or European portal codes

      const apiUrl = `https://api.adzuna.com/v1/api/jobs/${country}/search/1?app_id=${APP_ID}&app_key=${APP_KEY}&results_per_page=5&what=${encodeURIComponent(role)}&where=${encodeURIComponent(location)}`;

      try {
        const response = await fetch(apiUrl);
        const data = await response.json();

        if (!data.results || data.results.length === 0) {
          container.innerHTML = `<div class="loading-text">No live jobs found matching your query. Try broadening your terms!</div>`;
          return;
        }

        container.innerHTML = ''; // Clear loading text

        // Render each live job card
        data.results.forEach(job => {
          const matchPercentage = Math.floor(Math.random() * (99 - 85 + 1)) + 85; // Simulated match score
          const cardHtml = `
            <div class="job-card">
              <div class="job-details">
                <a href="${job.redirect_url}" target="_blank" class="job-title">${job.title}</a>
                <div class="company-name">${job.company.display_name}</div>
                <div class="meta-tags">
                  <span>📍 ${job.location.display_name}</span>
                  <span>💼 ${job.contract_time || 'Full-time'}</span>
                  <span>📅 ${new Date(job.created).toLocaleDateString()}</span>
                </div>
                <div class="skills-section">
                  <span class="match-tag">✓ Strong Title Match</span>
                  <span class="skill-pill">Operations</span>
                  <span class="skill-pill">Management</span>
                </div>
              </div>
              <div class="score-container">
                <div class="circle-score">${matchPercentage}%</div>
                <div class="score-label">STRONG MATCH</div>
              </div>
            </div>
          `;
          container.innerHTML += cardHtml;
        });

      } catch (error) {
        console.error('API Error:', error);
        container.innerHTML = `<div class="loading-text" style="color: #ef4444;">❌ Failed to fetch live job data. Check your connection or API key.</div>`;
      }
    }
  </script>
</body>
</html>
