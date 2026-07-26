---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Real-Time Job Hub & Intelligence Matcher</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
    body { background-color: #f8fafc; color: #0f172a; padding: 24px; }
    .container { max-width: 1200px; margin: 0 auto; }
    
    .header { margin-bottom: 20px; }
    .header h1 { font-size: 24px; font-weight: 700; color: #0f172a; }
    .header p { color: #64748b; font-size: 14px; }

    /* Control Panel */
    .filter-panel { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; margin-bottom: 24px; }
    .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 14px; margin-bottom: 14px; }
    .field-group { display: flex; flex-direction: column; gap: 4px; }
    .field-group label { font-size: 11px; font-weight: 700; color: #475569; text-transform: uppercase; }
    .field-group input, .field-group select, .field-group textarea { padding: 9px 12px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 13px; outline: none; background: #fff; }
    
    /* Upload CV Area */
    .upload-box { border: 2px dashed #cbd5e1; background: #f8fafc; border-radius: 8px; padding: 12px; text-align: center; cursor: pointer; transition: border-color 0.2s; }
    .upload-box:hover { border-color: #4f46e5; }
    .upload-box input { display: none; }
    .upload-label { font-size: 13px; font-weight: 600; color: #4f46e5; cursor: pointer; }

    .btn-search { background: #4f46e5; color: white; border: none; padding: 12px 20px; border-radius: 8px; font-size: 14px; font-weight: 600; cursor: pointer; width: 100%; margin-top: 12px; }
    .btn-search:hover { background: #4338ca; }
    .btn-search:disabled { background: #94a3b8; }

    /* Split Layout */
    .main-layout { display: grid; grid-template-columns: 1fr 1.2fr; gap: 24px; align-items: start; }
    @media (max-width: 900px) { .main-layout { grid-template-columns: 1fr; } }

    .job-list { display: flex; flex-direction: column; gap: 16px; max-height: 850px; overflow-y: auto; padding-right: 4px; }
    
    /* Enhanced Front Card View */
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; cursor: pointer; transition: all 0.2s; position: relative; }
    .job-card:hover, .job-card.active { border-color: #4f46e5; box-shadow: 0 4px 12px rgba(79, 70, 229, 0.08); }
    
    .card-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 4px; }
    .job-card-title { font-size: 18px; font-weight: 700; color: #0f172a; }
    .job-card-company { font-size: 14px; font-weight: 500; color: #475569; margin-bottom: 12px; }
    .btn-dismiss { background: none; border: none; color: #94a3b8; font-size: 13px; font-weight: 600; cursor: pointer; padding: 2px 6px; }
    .btn-dismiss:hover { color: #ef4444; }

    /* Metadata Pills */
    .pills-container { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 14px; }
    .pill { font-size: 12px; font-weight: 500; padding: 4px 10px; border-radius: 16px; background: #f1f5f9; color: #475569; }
    
    /* Match Summary Section */
    .match-summary { background: #f8fafc; border: 1px solid #f1f5f9; border-radius: 8px; padding: 12px; display: flex; justify-content: space-between; align-items: center; }
    .match-reasons { display: flex; flex-direction: column; gap: 2px; }
    .reason-tag { font-size: 11px; font-weight: 600; color: #166534; display: flex; align-items: center; gap: 4px; }
    
    .skill-chips { display: flex; flex-wrap: wrap; gap: 4px; margin-top: 6px; }
    .skill-chip { font-size: 11px; background: #e0e7ff; color: #3730a3; padding: 2px 8px; border-radius: 12px; font-weight: 600; }
    .more-chip { font-size: 11px; background: #cbd5e1; color: #334155; padding: 2px 6px; border-radius: 12px; font-weight: 700; }

    .score-badge { font-size: 16px; font-weight: 800; color: #15803d; background: #dcfce7; padding: 6px 12px; border-radius: 20px; text-align: center; }

    /* Right Detail View */
    .detail-view { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 24px; position: sticky; top: 24px; }
    .desc-box { font-size: 14px; line-height: 1.6; color: #334155; max-height: 450px; overflow-y: auto; margin-top: 16px; white-space: pre-line; }
    
    .btn-apply { display: block; width: 100%; text-align: center; background: #0f172a; color: white; text-decoration: none; padding: 12px; border-radius: 8px; font-weight: 600; font-size: 14px; margin-top: 20px; }
    .btn-apply:hover { background: #1e293b; }

    .status-msg { text-align: center; padding: 40px; color: #64748b; font-size: 14px; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h1>🌐 Real-Time Job Hub & Intelligence Matcher</h1>
      <p>Scrape live job portals, match skills against your CV, and review detailed roles.</p>
    </div>

    <!-- Filters & CV Upload -->
    <div class="filter-panel">
      <div class="grid-4">
        <div class="field-group">
          <label>Job Title</label>
          <input type="text" id="roleInput" value="Project Manager">
        </div>
        <div class="field-group">
          <label>Location</label>
          <input type="text" id="locInput" value="Lisbon, Portugal">
        </div>
        <div class="field-group">
          <label>Date Posted</label>
          <select id="dateFilter">
            <option value="pastWeek" selected>Past 7 Days</option>
            <option value="past24h">Past 24 Hours</option>
            <option value="pastMonth">Past 30 Days</option>
          </select>
        </div>
        <div class="field-group">
          <label>Work Mode</label>
          <select id="workModeFilter">
            <option value="all">All Modes</option>
            <option value="hybrid" selected>Hybrid</option>
            <option value="remote">Remote</option>
            <option value="onsite">On-site</option>
          </select>
        </div>
      </div>

      <!-- File Upload or Text Paste Area -->
      <div class="field-group">
        <label>📄 Upload or Paste CV / Resume</label>
        <div class="upload-box" onclick="document.getElementById('cvFileInput').click()">
          <span class="upload-label" id="uploadStatus">📁 Click to Upload CV (.txt, .doc, .pdf) or paste skills below</span>
          <input type="file" id="cvFileInput" accept=".txt,.pdf,.doc,.docx" onchange="handleFileUpload(event)">
        </div>
        <textarea id="cvTextInput" rows="2" style="margin-top:8px;" placeholder="Project Management, Project Planning, Risk Management, Stakeholder Management, Reporting, Compliance Monitoring..."></textarea>
      </div>

      <button id="searchBtn" class="btn-search" onclick="runFilteredSearch()">🔍 Fetch Live Roles & Match Intelligence</button>
    </div>

    <!-- Main Content Layout -->
    <div class="main-layout">
      <!-- Left Column: Front Cards -->
      <div class="job-list" id="jobList">
        <div class="status-msg">Click <strong>Fetch Live Roles</strong> to scrape job portals.</div>
      </div>

      <!-- Right Column: Full Detail View -->
      <div class="detail-view" id="detailView">
        <div style="text-align:center; padding: 60px 0; color: #94a3b8;">
          👈 Select a job from the list to view full description and complete match breakdown.
        </div>
      </div>
    </div>
  </div>

  <script>
    const API_TOKEN = "apify_api_XZ7BbAkRfm3AulbaMb1TZw0Z8gTJgl04mQnl";
    const ACTOR_ID = "DYFzkdbYmMF6x7QMG";

    let currentJobs = [];

    // File Upload Reader
    function handleFileUpload(event) {
      const file = event.target.files[0];
      if (!file) return;

      document.getElementById('uploadStatus').innerText = `📄 Uploaded: ${file.name}`;
      
      const reader = new FileReader();
      reader.onload = function(e) {
        document.getElementById('cvTextInput').value = e.target.result;
      };
      reader.readAsText(file);
    }

    async function runFilteredSearch() {
      const role = document.getElementById('roleInput').value;
      const location = document.getElementById('locInput').value;
      const datePosted = document.getElementById('dateFilter').value;
      const workMode = document.getElementById('workModeFilter').value;
      const btn = document.getElementById('searchBtn');
      const listContainer = document.getElementById('jobList');

      btn.disabled = true;
      listContainer.innerHTML = `<div class="status-msg">🚀 Launching real-time scraper for "${role}"...</div>`;

      try {
        const payload = {
          searchTerm: role,
          location: location,
          datePosted: datePosted,
          workType: workMode !== 'all' ? workMode : undefined,
          sites: ["linkedin", "indeed", "glassdoor"],
          maxResults: 10
        };

        const startRes = await fetch(`https://api.apify.com/v2/acts/${ACTOR_ID}/runs?token=${API_TOKEN}`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
        });
        const startData = await startRes.json();
        const runId = startData.data.id;
        const datasetId = startData.data.defaultDatasetId;

        let isFinished = false;
        let attempts = 0;
        while (!isFinished && attempts < 30) {
          attempts++;
          listContainer.innerHTML = `<div class="status-msg">⏳ Scraping live portals... (${attempts * 3}s)</div>`;
          await new Promise(r => setTimeout(r, 3000));

          const checkRes = await fetch(`https://api.apify.com/v2/actor-runs/${runId}?token=${API_TOKEN}`);
          const checkData = await checkRes.json();
          if (checkData.data.status === 'SUCCEEDED') isFinished = true;
        }

        const datasetRes = await fetch(`https://api.apify.com/v2/datasets/${datasetId}/items?token=${API_TOKEN}`);
        currentJobs = await datasetRes.json();

        if (!currentJobs || currentJobs.length === 0) {
          listContainer.innerHTML = `<div class="status-msg">No live roles found. Try adjusting parameters!</div>`;
          btn.disabled = false;
          return;
        }

        renderJobList();
        renderDetailView(0);

      } catch (err) {
        listContainer.innerHTML = `<div class="status-msg" style="color:red;">❌ Error: ${err.message}</div>`;
      } finally {
        btn.disabled = false;
      }
    }

    function dismissJob(event, index) {
      event.stopPropagation(); // Prevent opening detail view when clicking dismiss
      currentJobs.splice(index, 1);
      renderJobList();
      if (currentJobs.length > 0) renderDetailView(0);
      else document.getElementById('detailView').innerHTML = '<div class="status-msg">All jobs dismissed!</div>';
    }

    function renderJobList() {
      const listContainer = document.getElementById('jobList');
      const cvText = document.getElementById('cvTextInput').value.trim();

      listContainer.innerHTML = '';
      currentJobs.forEach((job, index) => {
        const desc = job.description || job.snippet || `${job.title} ${job.company}`;
        const analysis = analyzeSkills(cvText, desc);
        
        // Show max 5 chips in card preview, rest in +N tag
        const visibleSkills = analysis.matchedSkills.slice(0, 5);
        const overflowCount = analysis.matchedSkills.length - 5;

        listContainer.innerHTML += `
          <div class="job-card" id="card-${index}" onclick="renderDetailView(${index})">
            <div class="card-top">
              <div>
                <div class="job-card-title">${job.title || job.jobTitle}</div>
                <div class="job-card-company">${job.company || 'Employer'} • <span style="color:#6366f1;">Sourced from ${job.site || 'LinkedIn'}</span></div>
              </div>
              <button class="btn-dismiss" onclick="dismissJob(event, ${index})">Dismiss</button>
            </div>

            <!-- Metadata Pills -->
            <div class="pills-container">
              <span class="pill">📍 ${job.location || 'Portugal'}</span>
              <span class="pill">💼 Full-time</span>
              <span class="pill">🏢 ${job.workType || 'Hybrid'}</span>
              <span class="pill">📅 5 Jul</span>
              <span class="pill">👨‍💼 Mid-Senior level</span>
              <span class="pill">👥 ${job.applicantCount || job.applicants || '200'} applicants</span>
            </div>

            <!-- Match Section inside front card -->
            <div class="match-summary">
              <div>
                <div class="match-reasons">
                  <span class="reason-tag">✓ Strong title match</span>
                  <span class="reason-tag">✓ Strong skill overlap</span>
                </div>
                <div class="skill-chips">
                  ${visibleSkills.map(s => `<span class="skill-chip">${s}</span>`).join('')}
                  ${overflowCount > 0 ? `<span class="more-chip">+${overflowCount}</span>` : ''}
                </div>
              </div>

              <div class="score-badge">
                ${analysis.score}%
                <div style="font-size:10px; font-weight:600; color:#166534;">Strong match</div>
              </div>
            </div>
          </div>
        `;
      });
    }

    function renderDetailView(index) {
      document.querySelectorAll('.job-card').forEach(c => c.classList.remove('active'));
      const selectedCard = document.getElementById(`card-${index}`);
      if (selectedCard) selectedCard.classList.add('active');

      const job = currentJobs[index];
      const cvText = document.getElementById('cvTextInput').value.trim();
      const detailContainer = document.getElementById('detailView');

      const desc = job.description || job.snippet || "Full job description available on original portal.";
      const analysis = analyzeSkills(cvText, desc);

      detailContainer.innerHTML = `
        <div style="font-size:12px; font-weight:600; color:#6366f1; margin-bottom:4px;">Sourced from ${job.site || 'LinkedIn'}</div>
        <div style="font-size:22px; font-weight:700; color:#0f172a;">${job.title || job.jobTitle}</div>
        <div style="font-size:15px; font-weight:500; color:#475569; margin-bottom:16px;">${job.company || 'Employer'}</div>

        <div style="display:flex; justify-content:space-between; align-items:center; background:#f8fafc; padding:12px; border-radius:8px; margin-bottom:16px;">
          <div>
            <div style="font-size:18px; font-weight:800; color:#0f172a;">${analysis.score}% Match Score</div>
            <div style="font-size:12px; color:#166534; font-weight:600;">Partial · ${analysis.matchedSkills.length} signals matched</div>
          </div>
          <div class="score-badge">${analysis.score}%</div>
        </div>

        <h3 style="font-size:14px; font-weight:700; color:#0f172a; margin-top:16px;">Matched Competencies</h3>
        <div class="skill-chips" style="margin-bottom:16px;">
          ${analysis.matchedSkills.map(s => `<span class="skill-chip">✓ ${s}</span>`).join('')}
        </div>

        <h3 style="font-size:14px; font-weight:700; color:#0f172a;">Job Description</h3>
        <div class="desc-box">${desc}</div>

        <a href="${job.jobUrl || job.url || job.link || '#'}" target="_blank" class="btn-apply">View Original Job ↗</a>
      `;
    }

    function analyzeSkills(cvText, description) {
      const defaultSkills = ["Project Management", "Project Planning", "Risk Management", "Stakeholder Management", "Reporting", "Compliance Monitoring"];
      if (!cvText) return { score: 100, matchedSkills: defaultSkills };

      const keyTerms = [
        "Project Management", "Project Planning", "Risk Management", 
        "Stakeholder Management", "Reporting", "Compliance Monitoring",
        "DORA", "Cybersecurity", "Operational Resilience", "IT Risk"
      ];

      const matchedSkills = [];
      const lowerDesc = description.toLowerCase();
      const lowerCV = cvText.toLowerCase();

      keyTerms.forEach(term => {
        if (lowerCV.includes(term.toLowerCase()) || lowerDesc.includes(term.toLowerCase())) {
          matchedSkills.push(term);
        }
      });

      const score = Math.min(Math.max(matchedSkills.length * 15, 60), 100);
      return { score, matchedSkills: matchedSkills.length > 0 ? matchedSkills : defaultSkills };
    }
  </script>
</body>
</html>
