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
    .sidebar { width: 240px; background: #ffffff; border-right: 1px solid #e2e8f0; display: flex; flex-direction: column; justify-content: space-between; padding: 24px 16px; }
    .brand { font-size: 20px; font-weight: 700; letter-spacing: 2px; margin-bottom: 32px; display: flex; align-items: center; gap: 8px; color: #0f172a; }
    .nav-menu { display: flex; flex-direction: column; gap: 6px; }
    .nav-item { display: flex; align-items: center; gap: 12px; padding: 10px 14px; border-radius: 8px; font-size: 14px; font-weight: 500; color: #64748b; text-decoration: none; cursor: pointer; transition: all 0.2s; }
    .nav-item:hover, .nav-item.active { background: #f1f5f9; color: #0f172a; font-weight: 600; }

    .user-profile { display: flex; align-items: center; gap: 10px; padding: 10px; border-top: 1px solid #e2e8f0; }
    .avatar { width: 32px; height: 32px; border-radius: 50%; background: #cbd5e1; }
    .user-info { font-size: 13px; font-weight: 600; }
    .user-plan { font-size: 11px; color: #64748b; }

    /* MAIN CONTENT AREA */
    .main-content { flex: 1; padding: 32px 40px; overflow-y: auto; }
    .page-title { font-size: 24px; font-weight: 700; margin-bottom: 4px; }
    .page-subtitle { font-size: 14px; color: #64748b; margin-bottom: 20px; }

    /* PAGE SECTIONS (Tabs) */
    .view-tab { display: none; }
    .view-tab.active { display: block; }

    /* FILTERS BAR */
    .filters-bar { display: flex; gap: 8px; margin-bottom: 16px; flex-wrap: wrap; }
    .filter-pill { background: #ecfdf5; border: 1px solid #a7f3d0; color: #065f46; font-size: 13px; font-weight: 500; padding: 6px 14px; border-radius: 20px; cursor: pointer; }
    .banner { background: #fefce8; border: 1px solid #fef08a; color: #854d0e; padding: 12px 16px; border-radius: 8px; font-size: 13px; margin-bottom: 24px; display: flex; align-items: center; gap: 8px; }

    /* JOB CARD DESIGN */
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 24px; margin-bottom: 16px; display: flex; justify-content: space-between; position: relative; }
    .job-details { flex: 1; }
    .company-logo { width: 40px; height: 40px; border-radius: 8px; background: #6366f1; color: white; display: flex; align-items: center; justify-content: center; font-weight: 700; margin-bottom: 12px; }
    .job-title { font-size: 18px; font-weight: 600; margin-bottom: 4px; }
    .company-name { font-size: 14px; color: #64748b; margin-bottom: 12px; }
    .meta-tags { display: flex; gap: 16px; font-size: 13px; color: #64748b; margin-bottom: 16px; flex-wrap: wrap; }

    .skills-section { margin-top: 12px; }
    .match-tag { display: inline-block; background: #f0fdf4; color: #166534; font-size: 12px; font-weight: 600; padding: 4px 8px; border-radius: 4px; margin-right: 6px; margin-bottom: 6px; }
    .skill-pill { display: inline-block; background: #ecfdf5; color: #047857; font-size: 12px; padding: 4px 10px; border-radius: 12px; margin-right: 6px; margin-bottom: 6px; }

    /* CIRCULAR MATCH SCORE */
    .score-container { display: flex; flex-direction: column; align-items: center; justify-content: center; min-width: 120px; border-left: 1px solid #f1f5f9; padding-left: 20px; }
    .circle-score { width: 70px; height: 70px; border-radius: 50%; border: 4px solid #a7f3d0; display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 18px; color: #047857; margin-bottom: 8px; }
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

    /* RESUME VIEWER BOX */
    .pdf-viewer { width: 100%; height: 500px; background: #0f172a; border-radius: 12px; display: flex; align-items: center; justify-content: center; color: white; font-weight: 500; }
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
        <div class="nav-item" onclick="showTab('referrals', this)">🎁 Referrals</div>
      </div>
    </div>
    <div class="user-profile">
      <div class="avatar"></div>
      <div>
        <div class="user-info">Mohammad Salman</div>
        <div class="user-plan">● Mini plan</div>
      </div>
    </div>
  </div>

  <!-- MAIN CONTAINER -->
  <div class="main-content">

    <!-- TAB 1: YOUR JOBS -->
    <div id="your-jobs" class="view-tab active">
      <div class="page-title">Your Jobs</div>
      <div class="page-subtitle">Matched to your profile. Ready to apply.</div>

      <div class="filters-bar">
        <div class="filter-pill">Operations Manager +4</div>
        <div class="filter-pill">Portugal ▾</div>
        <div class="filter-pill">Work mode 2 ▾</div>
        <div class="filter-pill">Experience 1 ▾</div>
      </div>

      <div class="banner">
        🕒 You've hit today's apply limit — keep browsing, applying resumes at 12:00 AM.
      </div>

      <!-- JOB CARD 1 -->
      <div class="job-card">
        <div class="job-details">
          <div class="company-logo" style="background: #a855f7;">M</div>
          <div class="job-title">Project Manager</div>
          <div class="company-name">Mantu</div>
          <div class="meta-tags">
            <span>📍 Lisbon, Portugal</span>
            <span>💼 Full-time</span>
            <span>🏠 Hybrid</span>
            <span>📅 5 Jul</span>
            <span>👥 200 applicants</span>
          </div>
          <div class="skills-section">
            <span class="match-tag">✓ Strong title match</span>
            <span class="match-tag">✓ Strong skill overlap</span><br><br>
            <span class="skill-pill">Project Management</span>
            <span class="skill-pill">Project Planning</span>
            <span class="skill-pill">Risk Management</span>
            <span class="skill-pill">Stakeholder Management</span>
          </div>
        </div>
        <div class="score-container">
          <div class="circle-score">100%</div>
          <div class="score-label">STRONG MATCH</div>
        </div>
      </div>

      <!-- JOB CARD 2 -->
      <div class="job-card">
        <div class="job-details">
          <div class="company-logo" style="background: #ef4444;">H</div>
          <div class="job-title">Manager, Supply Chain Management - Process & Systems Lead</div>
          <div class="company-name">Hikma Pharmaceuticals</div>
          <div class="meta-tags">
            <span>📍 Sintra, Lisbon, Portugal</span>
            <span>💼 Full-time</span>
            <span>🏠 Hybrid</span>
            <span>📅 Today</span>
          </div>
          <div class="skills-section">
            <span class="match-tag">✓ Strong skill overlap</span>
            <span class="match-tag">✓ Strong experience fit</span>
            <span class="match-tag">✓ Strong title match</span><br><br>
            <span class="skill-pill">Operations Management</span>
            <span class="skill-pill">Risk Management</span>
            <span class="skill-pill">Continuous Improvement</span>
          </div>
        </div>
        <div class="score-container">
          <div class="circle-score">95%</div>
          <div class="score-label">STRONG MATCH</div>
        </div>
      </div>
    </div>

    <!-- TAB 2: APPLICATION DRAFT -->
    <div id="application-draft" class="view-tab">
      <div class="page-title">Application Draft</div>
      <div class="page-subtitle">Review AI generated cover emails before sending.</div>
      <div class="job-card">
        <p>Your pending email draft for <strong>Project Manager at Mantu</strong> is ready for review.</p>
      </div>
    </div>

    <!-- TAB 3: MY APPLICATIONS -->
    <div id="my-applications" class="view-tab">
      <div class="page-title">My Applications</div>
      <div class="page-subtitle">Track and manage your job applications</div>

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
              <th>AUTO FU</th>
              <th>Follow-ups</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><input type="checkbox"></td>
              <td><strong>Crypto Finance Group</strong></td>
              <td>Senior Product Manager</td>
              <td><span class="badge-applied">✈️ Applied</span></td>
              <td>Jul 26</td>
              <td>👁️ 0 📥 0</td>
              <td><label class="switch"><input type="checkbox" checked><span class="slider"></span></label></td>
              <td>0/2</td>
            </tr>
            <tr>
              <td><input type="checkbox"></td>
              <td><strong>Richemont</strong></td>
              <td>Procurement Category Manager</td>
              <td><span class="badge-applied">✈️ Applied</span></td>
              <td>Jul 26</td>
              <td>👁️ 1 📥 0</td>
              <td><label class="switch"><input type="checkbox" checked><span class="slider"></span></label></td>
              <td>0/2</td>
            </tr>
            <tr>
              <td><input type="checkbox"></td>
              <td><strong>Envision Energy</strong></td>
              <td>Senior Procurement Manager</td>
              <td><span class="badge-applied">✈️ Applied</span></td>
              <td>Jul 26</td>
              <td>👁️ 1 📥 0</td>
              <td><label class="switch"><input type="checkbox" checked><span class="slider"></span></label></td>
              <td>0/2</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- TAB 4: MY RESUME -->
    <div id="my-resume" class="view-tab">
      <div class="page-title">My Resume</div>
      <div class="page-subtitle">Your enhanced CV is ready.</div>
      <div class="pdf-viewer">
        [ Enhanced CV Preview Container - PAGE 1 / 2 ]
      </div>
    </div>

    <!-- TAB 5: REFERRALS -->
    <div id="referrals" class="view-tab">
      <div class="page-title">Referrals</div>
      <div class="page-subtitle">Invite colleagues to unlock additional application limits.</div>
    </div>

  </div>

  <script>
    function showTab(tabId, element) {
      // Hide all tabs
      document.querySelectorAll('.view-tab').forEach(tab => tab.classList.remove('active'));
      // Remove active status from all navigation items
      document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
      
      // Show targeted tab
      document.getElementById(tabId).classList.add('active');
      // Highlight current menu item
      element.classList.add('active');
    }
  </script>
</body>
</html>
