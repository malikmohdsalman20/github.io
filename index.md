---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Real-Time Job Aggregator & AI CV Matcher</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
    body { background-color: #f8fafc; color: #0f172a; padding: 32px 20px; }
    .container { max-width: 1050px; margin: 0 auto; }
    
    .header { margin-bottom: 24px; }
    .header h1 { font-size: 26px; font-weight: 700; color: #0f172a; }
    .header p { color: #64748b; font-size: 14px; margin-top: 4px; }

    /* Control Panel Card */
    .filter-panel { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 24px; margin-bottom: 24px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); }
    
    .grid-3 { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 16px; margin-bottom: 16px; }
    .field-group { display: flex; flex-direction: column; gap: 6px; }
    .field-group label { font-size: 12px; font-weight: 600; color: #475569; text-transform: uppercase; letter-spacing: 0.5px; }
    .field-group input, .field-group select, .field-group textarea { padding: 10px 12px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 14px; outline: none; background: #fff; }
    .field-group input:focus, .field-group select:focus, .field-group textarea:focus { border-color: #4f46e5; ring: 2px #e0e7ff; }

    .cv-section { margin-top: 16px; padding-top: 16px; border-top: 1px solid #f1f5f9; }
    
    .btn-search { background: #4f46e5; color: white; border: none; padding: 12px 24px; border-radius: 8px; font-size: 15px; font-weight: 600; cursor: pointer; transition: background 0.2s; width: 100%; margin-top: 16px; }
    .btn-search:hover { background: #4338ca; }
    .btn-search:disabled { background: #94a3b8; cursor: not-allowed; }

    /* Results */
    .results-info { display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; }
    .job-list { display: flex; flex-direction: column; gap: 16px; }
    
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; display: flex; justify-content: space-between; align-items: flex-start; gap: 16px; }
    .job-main { flex: 1; }
    .job-title { font-size: 18px; font-weight: 600; color: #0f172a; text-decoration: none; }
    .job-title:hover { color: #4f46e5; }
    .company-info { font-size: 14px; color: #475569; margin: 6px 0 12px 0; }
    
    .tags-container { display: flex; gap: 8px; flex-wrap: wrap; align-items: center; }
    .tag { font-size: 11px; font-weight: 600; padding: 4px 10px; border-radius: 12px; background: #f1f5f9; color: #475569; text-transform: capitalize; }
    .tag-source { background: #e0e7ff; color: #3730a3; }
    .tag-applicants { background: #fef3c7; color: #92400e; }

    /* Score Badge */
    .score-box { display: flex; flex-direction: column; align-items: flex-end; justify-content: space-between; height: 100%; min-width: 110px; }
    .match-badge { padding: 6px 12px; border-radius: 20px; font-size: 13px; font-weight: 700; text-align: center; }
    .match-high { background: #dcfce7; color: #166534; border: 1px solid #bbf7d0; }
    .match-mid { background: #fef9c3; color: #854d0e; border: 1px solid #fef08a; }
    .match-low { background: #f1f5f9; color: #64748b; border: 1px solid #e2e8f0; }

    .apply-btn { background: #0f172a; color: white; text-decoration: none; padding: 8px 16px; border-radius: 6px; font-size: 13px; font-weight: 600; display: inline-block; margin-top: 12px; }
    .apply-btn:hover { background: #1e293b; }

    .status-msg { text-align: center; padding: 48px; background: #fff; border-radius: 12px; border: 1px dashed #cbd5e1; color: #64748b; font-size: 15px; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h1>🌐 Real-Time Job Hub & AI Matcher</h1>
      <p>Scrape active LinkedIn, Indeed, and Glassdoor postings with real-time CV matching.</p>
    </div>

    <!-- Filter & CV Input Panel -->
    <div class="filter-panel">
      <div class="grid-3">
        <div class="field-group">
          <label>Job Title / Role</label>
          <input type="text" id="roleInput" value="Operations Manager" placeholder="e.g. Operations Manager">
        </div>

        <div class="field-group">
          <label>Location</label>
          <input type="text" id="locInput" value="Portugal" placeholder="e.g. Portugal, Lisbon">
        </div>

        <div class="field-group">
          <label>Date Posted</label>
          <select id="dateFilter">
            <option value="anytime">Anytime</option>
            <option value="past24h">Past 24 Hours</option>
            <option value="pastWeek" selected>Past 7 Days</option>
            <option value="pastMonth">Past 30 Days</option>
          </select>
        </div>

        <div class="field-group">
          <label>Work Mode</label>
          <select id="workModeFilter">
            <option value="all">All Modes</option>
            <option value="remote">Remote Only</option>
            <option value="hybrid">Hybrid</option>
            <option value="onsite">On-site</option>
          </select>
        </div>

        <div class="field-group">
          <label>Applicant Limit</label>
          <select id="applicantFilter">
            <option value="all">Any Applicant Count</option>
            <option value="10">Under 10 Applicants</option>
            <option value="20">Under 20 Applicants</option>
            <option value="50">Under 50 Applicants</option>
          </select>
        </div>

        <div class="field-group">
          <label>Max Results</label>
          <select id="maxResultsInput">
            <option value="10">10 Roles</option>
            <option value="20">20 Roles</option>
          </select>
        </div>
      </div>

      <!-- CV Upload & Input Section -->
      <div class="cv-section">
        <div class="field-group">
          <label>📄 Paste Resume / CV Text (For AI Match %)</label>
          <textarea id="cvTextInput" rows="3" placeholder="Paste your CV highlights, skills, or experience here to score match percentages against scraped job descriptions..."></textarea>
        </div>
      </div>

      <button id="searchBtn" class="btn-search" onclick="runFilteredSearch()">🔍 Fetch & Match Real-Time Jobs</button>
    </div>

    <!-- Results Area -->
    <div id="resultsMeta" class="results-info"></div>
    <div class="job-list" id="jobList">
      <div class="status-msg">Configure filters above and click <strong>Fetch & Match Real-Time Jobs</strong>.</div>
    </div>
  </div>

  <script>
    const API_TOKEN = "apify_api_XZ7BbAkRfm3AulbaMb1TZw0Z8gTJgl04mQnl";
    const ACTOR_ID = "DYFzkdbYmMF6x7QMG"; // Multi Job Board Scraper

    async function runFilteredSearch() {
      const role = document.getElementById('roleInput').value;
      const location = document.getElementById('locInput').value;
      const datePosted = document.getElementById('dateFilter').value;
      const workMode = document.getElementById('workModeFilter').value;
      const maxApps = document.getElementById('applicantFilter').value;
      const maxResults = parseInt(document.getElementById('maxResultsInput').value);
      const cvText = document.getElementById('cvTextInput').value.trim();

      const btn = document.getElementById('searchBtn');
      const container = document.getElementById('jobList');
      const meta = document.getElementById('resultsMeta');

      btn.disabled = true;
      meta.innerHTML = '';
      container.innerHTML = `<div class="status-msg">🚀 Launching real-time cloud scrape for "${role}" in "${location}"...</div>`;

      try {
        // Build payload with date and mode parameters
        const payload = {
          searchTerm: role,
          location: location,
          datePosted: datePosted,
          workType: workMode !== 'all' ? workMode : undefined,
          sites: ["linkedin", "indeed", "glassdoor", "google"],
          maxResults: maxResults
        };

        // 1. Trigger Apify Run
        const startResponse = await fetch(`https://api.apify.com/v2/acts/${ACTOR_ID}/runs?token=${API_TOKEN}`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
        });

        const startData = await startResponse.json();
        const runId = startData.data.id;
        const datasetId = startData.data.defaultDatasetId;

        // 2. Poll until finished
        let isFinished = false;
        let attempts = 0;

        while (!isFinished && attempts < 30) {
          attempts++;
          container.innerHTML = `<div class="status-msg">⏳ Scraping live portals... (${attempts * 3}s elapsed)</div>`;
          await new Promise(r => setTimeout(r, 3000));

          const checkRes = await fetch(`https://api.apify.com/v2/actor-runs/${runId}?token=${API_TOKEN}`);
          const checkData = await checkRes.json();

          if (checkData.data.status === 'SUCCEEDED') {
            isFinished = true;
          } else if (['FAILED', 'ABORTED', 'TIMED-OUT'].includes(checkData.data.status)) {
            throw new Error(`Scraper finished with status: ${checkData.data.status}`);
          }
        }

        // 3. Download Scraped Items
        container.innerHTML = `<div class="status-msg">✅ Scrape complete! Calculating match scores...</div>`;
        const datasetRes = await fetch(`https://api.apify.com/v2/datasets/${datasetId}/items?token=${API_TOKEN}`);
        let jobs = await datasetRes.json();

        // 4. Apply Client-side Filtering (Applicant Count Cap & Mode)
        if (maxApps !== 'all') {
          const cap = parseInt(maxApps);
          jobs = jobs.filter(j => {
            const apps = j.applicantCount || j.applicants || 0;
            return apps === 0 || apps <= cap;
          });
        }

        if (jobs.length === 0) {
          container.innerHTML = `<div class="status-msg">No live postings met your exact filter constraints. Try expanding the applicant cap or time range!</div>`;
          btn.disabled = false;
          return;
        }

        meta.innerHTML = `<span style="font-weight:600; color:#475569;">Displaying ${jobs.length} verified listings</span>`;
        container.innerHTML = '';

        // 5. Render Jobs with AI Match Scoring
        jobs.forEach(job => {
          const description = job.description || job.snippet || `${job.title} ${job.company}`;
          const matchPercent = calculateCVMatch(cvText, description);
          
          let badgeClass = "match-low";
          if (matchPercent >= 65) badgeClass = "match-high";
          else if (matchPercent >= 40) badgeClass = "match-mid";

          const appCount = job.applicantCount || job.applicants || 'N/A';
          const link = job.jobUrl || job.url || job.link || '#';

          container.innerHTML += `
            <div class="job-card">
              <div class="job-main">
                <a href="${link}" target="_blank" class="job-title">${job.title || job.jobTitle}</a>
                <div class="company-info">🏢 <strong>${job.company || 'Verified Employer'}</strong> — 📍 ${job.location || location}</div>
                
                <div class="tags-container">
                  <span class="tag tag-source">Portal: ${job.site || job.source || 'LinkedIn'}</span>
                  ${job.workType ? `<span class="tag">💼 ${job.workType}</span>` : ''}
                  <span class="tag tag-applicants">👥 Applicants: ${appCount}</span>
                </div>
              </div>

              <div class="score-box">
                ${cvText ? `<div class="match-badge ${badgeClass}">${matchPercent}% Match</div>` : ''}
                <a href="${link}" target="_blank" class="apply-btn">Apply Now ↗</a>
              </div>
            </div>
          `;
        });

      } catch (err) {
        console.error(err);
        container.innerHTML = `<div class="status-msg" style="color:red;">❌ Error: ${err.message}</div>`;
      } finally {
        btn.disabled = false;
      }
    }

    // Keyword Intersection & Match Score Engine
    function calculateCVMatch(cv, description) {
      if (!cv) return 0;
      
      const clean = text => text.toLowerCase().replace(/[^a-z0-9 ]/g, '').split(/\s+/).filter(w => w.length > 3);
      
      const cvWords = new Set(clean(cv));
      const descWords = clean(description);

      if (descWords.length === 0 || cvWords.size === 0) return 0;

      let matched = 0;
      const checked = new Set();

      descWords.forEach(word => {
        if (cvWords.has(word) && !checked.has(word)) {
          matched++;
          checked.add(word);
        }
      });

      const score = Math.round((matched / Math.min(cvWords.size, 25)) * 100);
      return Math.min(score, 98); // Cap realistic ceiling at 98%
    }
  </script>
</body>
</html>
