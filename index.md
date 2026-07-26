---
layout: default
title: AI Job Hunter Platform
---

<style>
  :root { --primary: #4f46e5; --bg: #f8fafc; --card: #ffffff; --text: #0f172a; }
  body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background: var(--bg); color: var(--text); }
  .nav-tabs { display: flex; gap: 10px; margin-bottom: 20px; border-bottom: 2px solid #e2e8f0; padding-bottom: 10px; }
  .tab-btn { background: none; border: none; font-size: 16px; font-weight: 600; padding: 8px 16px; cursor: pointer; color: #64748b; border-radius: 6px; }
  .tab-btn.active { background: var(--primary); color: white; }
  .tab-content { display: none; }
  .tab-content.active { display: block; }
  
  .grid-container { display: grid; grid-template-columns: 2fr 1fr; gap: 20px; }
  .card { background: var(--card); border: 1px solid #e2e8f0; border-radius: 10px; padding: 20px; margin-bottom: 15px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); }
  .badge { background: #dcfce7; color: #166534; padding: 4px 10px; border-radius: 20px; font-weight: bold; font-size: 12px; }
  .btn-primary { background: var(--primary); color: white; border: none; padding: 10px 18px; border-radius: 6px; cursor: pointer; font-weight: 600; width: 100%; margin-top: 10px; }
  .btn-primary:hover { opacity: 0.9; }
  input, textarea, select { width: 100%; padding: 10px; margin-top: 5px; margin-bottom: 15px; border: 1px solid #cbd5e1; border-radius: 6px; box-sizing: border-box; }
</style>

# 🤖 AI Job Hunter & Application Autopilot

<div class="nav-tabs">
  <button class="tab-btn active" onclick="switchTab('jobs')">🔍 Live Job Matcher</button>
  <button class="tab-btn" onclick="switchTab('tailor')">✨ AI CV & Cover Letter</button>
  <button class="tab-btn" onclick="switchTab('tracker')">📊 Application Tracker</button>
</div>

<!-- TAB 1: JOB MATCHER -->
<div id="jobs" class="tab-content active">
  <div class="grid-container">
    <div>
      <h3>Filtered AI Matches</h3>
      <div id="jobCards">
        <!-- Rendered via JS -->
      </div>
    </div>
    <div>
      <div class="card">
        <h3>Target Search Profile</h3>
        <label>Job Title / Role</label>
        <input type="text" id="targetRole" value="Operations & Supply Chain Manager">
        <label>Location</label>
        <input type="text" id="targetLoc" value="Europe / Remote">
        <label>Minimum AI Match Score</label>
        <select id="minScore">
          <option value="80">80% and above</option>
          <option value="90">90% and above</option>
        </select>
        <button class="btn-primary" onclick="searchJobs()">Run AI Scraper & Matcher</button>
      </div>
    </div>
  </div>
</div>

<!-- TAB 2: AI CV TAILOR -->
<div id="tailor" class="tab-content">
  <div class="card">
    <h3>Custom AI Cover Letter & Email Generator</h3>
    <label>Job Description</label>
    <textarea id="jdText" rows="4" placeholder="Paste the recruiter's job description here..."></textarea>
    <button class="btn-primary" onclick="generateTailoredEmail()">Generate Personalised Recruiter Outreach</button>
    <div id="aiOutput" style="margin-top: 15px; white-space: pre-wrap; background: #f1f5f9; padding: 15px; border-radius: 6px; display: none;"></div>
  </div>
</div>

<!-- TAB 3: APPLICATION TRACKER -->
<div id="tracker" class="tab-content">
  <div class="card">
    <h3>Outreach & Callback Pipeline</h3>
    <table style="width:100%; text-align:left; border-collapse: collapse;">
      <thead>
        <tr style="border-bottom: 2px solid #cbd5e1;">
          <th style="padding: 8px;">Company</th>
          <th>Role</th>
          <th>Match</th>
          <th>Status</th>
          <th>Recruiter Follow-up</th>
        </tr>
      </thead>
      <tbody id="trackerTable">
        <!-- Rendered via JS -->
      </tbody>
    </table>
  </div>
</div>

<script>
  function switchTab(tabId) {
    document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
    document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
    document.getElementById(tabId).classList.add('active');
    event.currentTarget.classList.add('active');
  }

  const mockJobs = [
    { id: 1, title: "Operations & Production Coordinator", company: "Batches Electronics Partner", location: "Lisbon / Hybrid", match: 95, status: "Applied" },
    { id: 2, title: "Supply Chain Analyst", company: "EuroLogistics Tech", location: "Remote Europe", match: 88, status: "Follow-up Sent" },
    { id: 3, title: "Logistics Optimization Lead", company: "Global Transit", location: "Europe", match: 82, status: "Saved" }
  ];

  function renderJobs() {
    const container = document.getElementById('jobCards');
    container.innerHTML = mockJobs.map(job => `
      <div class="card">
        <div style="display:flex; justify-between; align-items:center;">
          <h4 style="margin:0;">${job.title}</h4>
          <span class="badge">${job.match}% Match</span>
        </div>
        <p style="margin:5px 0; color:#64748b;">${job.company} • ${job.location}</p>
        <button class="btn-primary" style="width:auto; padding:6px 12px;" onclick="switchTab('tailor')">Tailor & Apply with AI</button>
      </div>
    `).join('');
  }

  function renderTracker() {
    const table = document.getElementById('trackerTable');
    table.innerHTML = mockJobs.map(job => `
      <tr style="border-bottom: 1px solid #e2e8f0;">
        <td style="padding:10px 0;"><strong>${job.company}</strong></td>
        <td>${job.title}</td>
        <td><span class="badge">${job.match}%</span></td>
        <td>${job.status}</td>
        <td>Auto-scheduled (+2 days)</td>
      </tr>
    `).join('');
  }

  function generateTailoredEmail() {
    const output = document.getElementById('aiOutput');
    output.style.display = 'block';
    output.innerText = "Generating custom email using AI...\n\nSubject: Application for Operations Lead - Mohammad Salman\n\nDear Hiring Team,\n\nI am writing to express my interest in the Operations position. With my background in production operations and process optimization..."
  }

  renderJobs();
  renderTracker();
</script>
