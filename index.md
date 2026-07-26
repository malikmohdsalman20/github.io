---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Global Job Hub & AI Application Suite</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
  <script src="https://accounts.google.com/gsi/client" async defer></script>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
    body { background-color: #f8fafc; color: #0f172a; padding: 24px; }
    .container { max-width: 1300px; margin: 0 auto; }

    /* Top Bar: Google Auth & Stats */
    .top-bar { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 16px 24px; margin-bottom: 24px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 16px; }
    .user-profile { display: flex; align-items: center; gap: 12px; }
    .user-avatar { width: 38px; height: 38px; border-radius: 50%; border: 2px solid #4f46e5; }
    
    .stats-container { display: flex; gap: 16px; }
    .stat-card { background: #f1f5f9; padding: 8px 16px; border-radius: 8px; text-align: center; }
    .stat-num { font-size: 18px; font-weight: 800; color: #4f46e5; }
    .stat-label { font-size: 11px; font-weight: 600; color: #64748b; text-transform: uppercase; }

    /* Search & Controls Panel */
    .filter-panel { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; margin-bottom: 24px; }
    .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 14px; margin-bottom: 14px; }
    .field-group { display: flex; flex-direction: column; gap: 4px; }
    .field-group label { font-size: 11px; font-weight: 700; color: #475569; text-transform: uppercase; }
    .field-group input, .field-group select, .field-group textarea { padding: 9px 12px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 13px; outline: none; background: #fff; }

    .upload-box { border: 2px dashed #cbd5e1; background: #f8fafc; border-radius: 8px; padding: 12px; text-align: center; cursor: pointer; }
    .upload-box:hover { border-color: #4f46e5; }
    .upload-box input { display: none; }
    .upload-label { font-size: 13px; font-weight: 600; color: #4f46e5; }

    .btn-search { background: #4f46e5; color: white; border: none; padding: 12px 20px; border-radius: 8px; font-size: 14px; font-weight: 600; cursor: pointer; width: 100%; margin-top: 12px; }
    .btn-search:hover { background: #4338ca; }
    .btn-search:disabled { background: #94a3b8; }

    /* Layout */
    .main-layout { display: grid; grid-template-columns: 1fr 1.25fr; gap: 24px; align-items: start; }
    @media (max-width: 900px) { .main-layout { grid-template-columns: 1fr; } }

    .job-list { display: flex; flex-direction: column; gap: 14px; max-height: 850px; overflow-y: auto; padding-right: 4px; }
    
    /* Card Design */
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 18px; cursor: pointer; transition: all 0.2s; position: relative; }
    .job-card:hover, .job-card.active { border-color: #4f46e5; box-shadow: 0 4px 12px rgba(79, 70, 229, 0.08); }
    .job-card.applied { border-left: 5px solid #16a34a; background: #f0fdf4; }
    
    .card-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 4px; }
    .job-card-title { font-size: 17px; font-weight: 700; color: #0f172a; }
    .job-card-company { font-size: 13px; font-weight: 500; color: #475569; margin-bottom: 10px; }
    
    .pills-container { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 12px; }
    .pill { font-size: 11px; font-weight: 500; padding: 3px 8px; border-radius: 12px; background: #f1f5f9; color: #475569; }
    
    /* Applied Toggle Switch */
    .apply-toggle-box { display: flex; align-items: center; gap: 6px; background: #ffffff; padding: 4px 10px; border-radius: 20px; border: 1px solid #cbd5e1; font-size: 11px; font-weight: 700; }
    .btn-toggle { padding: 3px 8px; border-radius: 12px; border: none; font-size: 10px; font-weight: 700; cursor: pointer; }
    .btn-yes { background: #dcfce7; color: #15803d; }
    .btn-no { background: #f1f5f9; color: #64748b; }

    .match-summary { background: #f8fafc; border: 1px solid #f1f5f9; border-radius: 8px; padding: 10px; display: flex; justify-content: space-between; align-items: center; }
    .skill-chips { display: flex; flex-wrap: wrap; gap: 4px; margin-top: 4px; }
    .skill-chip { font-size: 10px; background: #e0e7ff; color: #3730a3; padding: 2px 6px; border-radius: 10px; font-weight: 600; }
    .score-badge { font-size: 15px; font-weight: 800; color: #15803d; background: #dcfce7; padding: 4px 10px; border-radius: 16px; text-align: center; }

    /* Right Detail View */
    .detail-view { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 24px; position: sticky; top: 24px; }
    .desc-box { font-size: 13px; line-height: 1.6; color: #334155; max-height: 380px; overflow-y: auto; margin-top: 12px; white-space: pre-wrap; background: #f8fafc; padding: 12px; border-radius: 8px; font-family: inherit; }
    
    .btn-apply-main { display: block; width: 100%; text-align: center; background: #0a66c2; color: white; text-decoration: none; padding: 12px; border-radius: 8px; font-weight: 700; font-size: 14px; margin-top: 12px; }
    .btn-apply-main:hover { background: #004182; }

    .email-box { background: #eff6ff; border: 1px solid #bfdbfe; border-radius: 8px; padding: 14px; margin-top: 16px; }
    .email-content { font-size: 12px; color: #1e3a8a; line-height: 1.5; white-space: pre-wrap; font-family: monospace; background: #fff; padding: 10px; border-radius: 6px; border: 1px solid #dbeafe; max-height: 140px; overflow-y: auto; }

    .status-msg { text-align: center; padding: 40px; color: #64748b; font-size: 14px; }
  </style>
</head>
<body>

  <div class="container">
    
    <!-- Top Bar: Title, Stats & Google Login -->
    <div class="top-bar">
      <div>
        <h1 style="font-size:20px; font-weight:700;">🌐 Global Job Search & Application Tracker</h1>
        <p style="font-size:12px; color:#64748b;">Search worldwide job portals, match CV skills, and track applications.</p>
      </div>

      <div class="stats-container">
        <div class="stat-card">
          <div class="stat-num" id="statApplied">0</div>
          <div class="stat-label">Jobs Applied</div>
        </div>
        <div class="stat-card">
          <div class="stat-num" id="statTotal">0</div>
          <div class="stat-label">Total Found</div>
        </div>
      </div>

      <div id="googleAuthSection">
        <div id="g_id_onload"
             data-client_id="YOUR_GOOGLE_CLIENT_ID.apps.googleusercontent.com"
             data-callback="handleCredentialResponse">
        </div>
        <div class="g_id_signin" data-type="standard"></div>
      </div>

      <div id="userInfo" class="user-profile" style="display:none;">
        <img id="userImg" class="user-avatar" src="" alt="Profile">
        <div>
          <div id="userName" style="font-size:13px; font-weight:700;"></div>
          <div id="userEmail" style="font-size:11px; color:#64748b;"></div>
        </div>
      </div>
    </div>

    <!-- Search Controls (Blank Inputs) -->
    <div class="filter-panel">
      <div class="grid-4">
        <div class="field-group">
          <label>Job Title Search</label>
          <input type="text" id="roleInput" value="" placeholder="e.g. Operations Manager or Project Manager">
        </div>

        <div class="field-group">
          <label>Location</label>
          <input type="text" id="locInput" value="" placeholder="e.g. Portugal or Worldwide">
        </div>

        <div class="field-group">
          <label>Date Posted</label>
          <select id="dateFilter">
            <option value="pastWeek" selected>Past 7 Days</option>
            <option value="past24h">Past 24 Hours</option>
            <option value="pastMonth">Past 30 Days</option>
            <option value="anytime">Anytime</option>
          </select>
        </div>

        <div class="field-group">
          <label>Applicant Limit</label>
          <select id="applicantFilter">
            <option value="all">Any Applicant Count</option>
            <option value="10">Under 10 Applicants</option>
            <option value="25">Under 25 Applicants</option>
            <option value="50">Under 50 Applicants</option>
          </select>
        </div>
      </div>

      <!-- PDF CV Upload -->
      <div class="field-group">
        <label>📄 Upload PDF / Document CV (Dynamic Skill Matching)</label>
        <div class="upload-box" onclick="document.getElementById('cvFileInput').click()">
          <span class="upload-label" id="uploadStatus">📁 Click to Upload PDF CV or paste text below</span>
          <input type="file" id="cvFileInput" accept=".pdf,.txt,.doc,.docx" onchange="handlePDFUpload(event)">
        </div>
        <textarea id="cvTextInput" rows="2" style="margin-top:8px;" placeholder="Paste CV skills or text here..."></textarea>
      </div>

      <button id="searchBtn" class="btn-search" onclick="runLiveSearch()">🔍 Scrape Jobs & Perform CV Skill Match</button>
    </div>

    <!-- Results Split Grid -->
    <div class="main-layout">
      <div class="job-list" id="jobList">
        <div class="status-msg">Type search criteria above and click <strong>Scrape Jobs</strong>.</div>
      </div>

      <div class="detail-view" id="detailView">
        <div style="text-align:center; padding: 60px 0; color: #94a3b8;">
          👈 Select a job from the list to view details and apply directly.
        </div>
      </div>
    </div>
  </div>

  <script>
    const API_TOKEN = "apify_api_XZ7BbAkRfm3AulbaMb1TZw0Z8gTJgl04mQnl";
    const ACTOR_ID = "DYFzkdbYmMF6x7QMG";

    let currentJobs = [];
    let appliedJobs = new Set();

    // Google Sign-In Callback
    function handleCredentialResponse(response) {
      const payload = parseJwt(response.credential);
      document.getElementById('googleAuthSection').style.display = 'none';
      document.getElementById('userInfo').style.display = 'flex';
      document.getElementById('userName').innerText = payload.name;
      document.getElementById('userEmail').innerText = payload.email;
      document.getElementById('userImg').src = payload.picture;
    }

    function parseJwt(token) {
      var base64Url = token.split('.')[1];
      var base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
      var jsonPayload = decodeURIComponent(window.atob(base64).split('').map(function(c) {
          return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
      }).join(''));
      return JSON.parse(jsonPayload);
    }

    // Toggle Applied Status
    function toggleApplied(index, status) {
      const job = currentJobs[index];
      const jobKey = job.jobUrl || job.title;

      if (status === 'YES') appliedJobs.add(jobKey);
      else appliedJobs.delete(jobKey);

      updateStats();
      renderJobList();
      renderDetailView(index);
    }

    function updateStats() {
      document.getElementById('statApplied').innerText = appliedJobs.size;
      document.getElementById('statTotal').innerText = currentJobs.length;
    }

    // PDF Parser
    async function handlePDFUpload(event) {
      const file = event.target.files[0];
      if (!file) return;

      document.getElementById('uploadStatus').innerText = `📄 Uploading: ${file.name}`;

      if (file.type === "application/pdf") {
        const fileReader = new FileReader();
        fileReader.onload = async function() {
          const typedarray = new Uint8Array(this.result);
          pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js';
          
          const pdf = await pdfjsLib.getDocument(typedarray).promise;
          let fullText = "";
          for (let i = 1; i <= pdf.numPages; i++) {
            const page = await pdf.getPage(i);
            const textContent = await page.getTextContent();
            fullText += textContent.items.map(item => item.str).join(" ") + " ";
          }
          document.getElementById('cvTextInput').value = fullText;
          document.getElementById('uploadStatus').innerText = `✅ PDF Loaded: ${file.name}`;
        };
        fileReader.readAsArrayBuffer(file);
      } else {
        const reader = new FileReader();
        reader.onload = e => document.getElementById('cvTextInput').value = e.target.result;
        reader.readAsText(file);
        document.getElementById('uploadStatus').innerText = `✅ File Loaded: ${file.name}`;
      }
    }

    // Dynamic Skill Matcher
    function extractCVWords(cvText) {
      if (!cvText || cvText.trim().length === 0) return [];
      const stopWords = new Set(["with", "from", "that", "this", "have", "been", "were", "their", "about", "which", "will", "would", "there", "management", "experience"]);
      const words = cvText.toLowerCase().replace(/[^a-z0-9\s]/g, ' ').split(/\s+/).filter(w => w.length > 3 && !stopWords.has(w));
      return Array.from(new Set(words));
    }

    function calculateDynamicMatch(cvWords, jobDesc) {
      if (!cvWords || cvWords.length === 0 || !jobDesc) return { score: 0, matched: [] };
      const descLower = jobDesc.toLowerCase();
      const matched = [];

      cvWords.forEach(word => {
        if (descLower.includes(word)) {
          matched.push(word.charAt(0).toUpperCase() + word.slice(1));
        }
      });

      const matchRatio = matched.length / Math.min(cvWords.length, 25);
      const score = Math.min(Math.round(matchRatio * 100), 98);
      return { score, matched };
    }

    async function runLiveSearch() {
      const role = document.getElementById('roleInput').value.trim();
      const location = document.getElementById('locInput').value.trim();
      const datePosted = document.getElementById('dateFilter').value;
      const maxApps = document.getElementById('applicantFilter').value;
      const btn = document.getElementById('searchBtn');
      const listContainer = document.getElementById('jobList');

      if (!role) {
        alert("Please enter a job title to search!");
        return;
      }

      btn.disabled = true;
      listContainer.innerHTML = `<div class="status-msg">🚀 Scraping live jobs for "${role}"...</div>`;

      try {
        const payload = {
          searchTerm: role,
          location: location || undefined,
          datePosted: datePosted !== 'anytime' ? datePosted : undefined,
          sites: ["linkedin", "indeed", "glassdoor"],
          maxResults: 50
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
        while (!isFinished && attempts < 40) {
          attempts++;
          listContainer.innerHTML = `<div class="status-msg">⏳ Fetching live vacancies from portals... (${attempts * 3}s)</div>`;
          await new Promise(r => setTimeout(r, 3000));

          const checkRes = await fetch(`https://api.apify.com/v2/actor-runs/${runId}?token=${API_TOKEN}`);
          const checkData = await checkRes.json();
          if (checkData.data.status === 'SUCCEEDED') isFinished = true;
        }

        const datasetRes = await fetch(`https://api.apify.com/v2/datasets/${datasetId}/items?token=${API_TOKEN}`);
        currentJobs = await datasetRes.json();

        if (maxApps !== 'all') {
          const cap = parseInt(maxApps);
          currentJobs = currentJobs.filter(j => {
            const apps = j.applicantCount || j.applicants || 0;
            return apps === 0 || apps <= cap;
          });
        }

        updateStats();
        renderJobList();
        renderDetailView(0);

      } catch (err) {
        listContainer.innerHTML = `<div class="status-msg" style="color:red;">❌ Error: ${err.message}</div>`;
      } finally {
        btn.disabled = false;
      }
    }

    function renderJobList() {
      const listContainer = document.getElementById('jobList');
      const cvText = document.getElementById('cvTextInput').value.trim();
      const cvWords = extractCVWords(cvText);

      listContainer.innerHTML = `<div style="font-size:13px; font-weight:700; color:#475569;">Found ${currentJobs.length} live vacancies</div>`;

      currentJobs.forEach((job, index) => {
        const jobKey = job.jobUrl || job.title;
        const isApplied = appliedJobs.has(jobKey);
        const fullDesc = job.description || job.snippet || job.title;
        const analysis = calculateDynamicMatch(cvWords, fullDesc);
        const targetUrl = job.jobUrl || job.url || job.link || '#';

        listContainer.innerHTML += `
          <div class="job-card ${isApplied ? 'applied' : ''}" id="card-${index}" onclick="renderDetailView(${index})">
            <div class="card-top">
              <div>
                <div class="job-card-title">${job.title || job.jobTitle}</div>
                <div class="job-card-company">${job.company || 'Employer'} — 📍 ${job.location || 'Location'}</div>
              </div>

              <div class="apply-toggle-box" onclick="event.stopPropagation();">
                <span>Applied?</span>
                <button class="btn-toggle ${isApplied ? 'btn-yes' : 'btn-no'}" onclick="toggleApplied(${index}, 'YES')">YES</button>
                <button class="btn-toggle ${!isApplied ? 'btn-yes' : 'btn-no'}" onclick="toggleApplied(${index}, 'NO')">NO</button>
              </div>
            </div>

            <div class="pills-container">
              <span class="pill">Portal: ${job.site || 'LinkedIn'}</span>
              <span class="pill">💼 ${job.employmentType || 'Full-time'}</span>
              <span class="pill">👥 ${job.applicantCount || job.applicants || 'Active'} applicants</span>
            </div>

            <div class="match-summary">
              <div>
                <span style="font-size:11px; font-weight:600; color:#166534;">Matched Skills</span>
                <div class="skill-chips">
                  ${analysis.matched.length > 0 
                    ? analysis.matched.slice(0, 4).map(s => `<span class="skill-chip">${s}</span>`).join('')
                    : '<span style="font-size:10px; color:#94a3b8;">Upload CV above to calculate match</span>'}
                </div>
              </div>

              <div style="display:flex; align-items:center; gap:8px;">
                <a href="${targetUrl}" target="_blank" rel="noopener noreferrer" style="font-size:11px; font-weight:700; color:#0a66c2; text-decoration:none;" onclick="event.stopPropagation();">
                  Apply on ${job.site || 'LinkedIn'} ↗
                </a>
                <div class="score-badge">${analysis.score}%</div>
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
      if (!job) return;

      const jobKey = job.jobUrl || job.title;
      const isApplied = appliedJobs.has(jobKey);
      const cvText = document.getElementById('cvTextInput').value.trim();
      const cvWords = extractCVWords(cvText);

      const fullNativeDesc = job.description || job.snippet || "Full description text available on original portal listing.";
      const analysis = calculateDynamicMatch(cvWords, fullNativeDesc);
      const targetUrl = job.jobUrl || job.url || job.link || '#';
      const companyClean = (job.company || 'Company').replace(/[^a-zA-Z0-9]/g, '').toLowerCase();

      const emailSubject = encodeURIComponent(`Application for ${job.title || 'Role'} - Mohammad Salman`);
      const emailBody = `Dear Hiring Team at ${job.company || 'the Company'},\n\nI am writing to express my strong interest in the ${job.title || 'open role'} position. With over 12 years of experience leading operations, project management, and supply chain execution across Portugal and internationally, I bring proven expertise in driving operational efficiency.\n\nKey Matched Skills:\n- ${analysis.matched.slice(0, 4).join('\n- ')}\n\nBest regards,\nMohammad Salman\nmalikmohdsalman20@gmail.com | +351 926 363 916`;

      const detailContainer = document.getElementById('detailView');
      detailContainer.innerHTML = `
        <div style="font-size:12px; font-weight:600; color:#6366f1;">Sourced from ${job.site || 'LinkedIn'}</div>
        <div style="font-size:20px; font-weight:700; color:#0f172a; margin-top:2px;">${job.title || job.jobTitle}</div>
        <div style="font-size:14px; font-weight:500; color:#475569; margin-bottom:12px;">${job.company || 'Employer'} — 📍 ${job.location || 'Location'}</div>

        <!-- Direct Portal Application Link Button -->
        <a href="${targetUrl}" target="_blank" rel="noopener noreferrer" class="btn-apply-main">
          🚀 Apply Directly on ${job.site || 'LinkedIn'} ↗
        </a>

        <div style="display:flex; justify-content:space-between; align-items:center; background:${isApplied ? '#dcfce7' : '#f8fafc'}; padding:10px; border-radius:8px; margin-top:12px;">
          <div style="font-size:13px; font-weight:700; color:${isApplied ? '#166534' : '#0f172a'};">
            ${isApplied ? '✅ Status: Marked as Applied' : '⏳ Status: Not Applied Yet'}
          </div>
          <button style="padding:5px 10px; font-size:11px; font-weight:700; border-radius:6px; border:none; cursor:pointer; background:${isApplied ? '#166534' : '#0f172a'}; color:white;"
                  onclick="toggleApplied(${index}, '${isApplied ? 'NO' : 'YES'}')">
            Mark as ${isApplied ? 'Not Applied' : 'Applied'}
          </button>
        </div>

        <h3 style="font-size:13px; font-weight:700; color:#0f172a; margin-top:16px;">Job Description (Original Language)</h3>
        <div class="desc-box">${fullNativeDesc}</div>

        <!-- Auto Email Draft Box -->
        <div class="email-box">
          <div style="font-size:13px; font-weight:700; color:#1e40af; margin-bottom:6px;">✉️ Generated HR Cover Letter Draft</div>
          <div class="email-content">${emailBody}</div>
          <a href="mailto:careers@${companyClean}.com?subject=${emailSubject}&body=${encodeURIComponent(emailBody)}" 
             style="display:inline-block; margin-top:10px; background:#2563eb; color:white; text-decoration:none; padding:8px 14px; border-radius:6px; font-size:12px; font-weight:600;">
            📧 Launch Email Client
          </a>
        </div>
      `;
    }
  </script>
</body>
</html>
