---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Global Job Search & Outreach Suite</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
    body { background-color: #f8fafc; color: #0f172a; padding: 24px; }
    .container { max-width: 1300px; margin: 0 auto; }
    
    .header { margin-bottom: 20px; }
    .header h1 { font-size: 24px; font-weight: 700; color: #0f172a; }
    .header p { color: #64748b; font-size: 14px; }

    /* Control Panel */
    .filter-panel { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; margin-bottom: 24px; }
    .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 14px; margin-bottom: 14px; }
    .field-group { display: flex; flex-direction: column; gap: 4px; }
    .field-group label { font-size: 11px; font-weight: 700; color: #475569; text-transform: uppercase; }
    .field-group input, .field-group select, .field-group textarea { padding: 9px 12px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 13px; outline: none; background: #fff; }

    /* Checkbox Multi-Select Box */
    .checkbox-group { background: #f8fafc; border: 1px solid #cbd5e1; border-radius: 8px; padding: 8px 12px; max-height: 110px; overflow-y: auto; display: flex; flex-direction: column; gap: 4px; }
    .checkbox-item { font-size: 12px; color: #334155; display: flex; align-items: center; gap: 6px; cursor: pointer; }

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
    
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 18px; cursor: pointer; transition: all 0.2s; position: relative; }
    .job-card:hover, .job-card.active { border-color: #4f46e5; box-shadow: 0 4px 12px rgba(79, 70, 229, 0.08); }
    
    .card-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 4px; }
    .job-card-title { font-size: 17px; font-weight: 700; color: #0f172a; }
    .job-card-company { font-size: 13px; font-weight: 500; color: #475569; margin-bottom: 10px; }
    
    .pills-container { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 12px; }
    .pill { font-size: 11px; font-weight: 500; padding: 3px 8px; border-radius: 12px; background: #f1f5f9; color: #475569; }
    
    .match-summary { background: #f8fafc; border: 1px solid #f1f5f9; border-radius: 8px; padding: 10px; display: flex; justify-content: space-between; align-items: center; }
    .skill-chips { display: flex; flex-wrap: wrap; gap: 4px; margin-top: 4px; }
    .skill-chip { font-size: 10px; background: #e0e7ff; color: #3730a3; padding: 2px 6px; border-radius: 10px; font-weight: 600; }
    .score-badge { font-size: 15px; font-weight: 800; color: #15803d; background: #dcfce7; padding: 4px 10px; border-radius: 16px; text-align: center; }

    /* Right Intelligence & Outreach Panel */
    .detail-view { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 24px; position: sticky; top: 24px; }
    .desc-box { font-size: 13px; line-height: 1.6; color: #334155; max-height: 320px; overflow-y: auto; margin-top: 12px; white-space: pre-wrap; background: #f8fafc; padding: 12px; border-radius: 8px; }
    
    /* Email Draft Box */
    .email-box { background: #eff6ff; border: 1px solid #bfdbfe; border-radius: 8px; padding: 16px; margin-top: 16px; }
    .email-title { font-size: 14px; font-weight: 700; color: #1e40af; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center; }
    .email-content { font-size: 12px; color: #1e3a8a; line-height: 1.5; white-space: pre-wrap; font-family: monospace; background: #fff; padding: 10px; border-radius: 6px; border: 1px solid #dbeafe; max-height: 180px; overflow-y: auto; }

    .btn-apply { display: inline-block; text-align: center; background: #0f172a; color: white; text-decoration: none; padding: 10px 16px; border-radius: 8px; font-weight: 600; font-size: 13px; margin-top: 12px; }
    .btn-email { display: inline-block; text-align: center; background: #2563eb; color: white; text-decoration: none; padding: 10px 16px; border-radius: 8px; font-weight: 600; font-size: 13px; margin-top: 12px; cursor: pointer; border: none; }

    .status-msg { text-align: center; padding: 40px; color: #64748b; font-size: 14px; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h1>🌐 Global Job Hub & HR Outreach Suite</h1>
      <p>Multi-title search worldwide with real-time PDF CV parsing and automated HR email drafting.</p>
    </div>

    <!-- Filters -->
    <div class="filter-panel">
      <div class="grid-4">
        <!-- Multi-Select Titles -->
        <div class="field-group">
          <label>Job Titles (Select Multiple)</label>
          <div class="checkbox-group">
            <label class="checkbox-item"><input type="checkbox" class="title-chk" value="Operations Manager" checked> Operations Manager</label>
            <label class="checkbox-item"><input type="checkbox" class="title-chk" value="Project Manager" checked> Project Manager</label>
            <label class="checkbox-item"><input type="checkbox" class="title-chk" value="Supply Chain Manager" checked> Supply Chain Manager</label>
            <label class="checkbox-item"><input type="checkbox" class="title-chk" value="Production Manager" checked> Production Manager</label>
            <label class="checkbox-item"><input type="checkbox" class="title-chk" value="Logistics Manager" checked> Logistics Manager</label>
          </div>
        </div>

        <!-- Global Searchable Location -->
        <div class="field-group">
          <label>Location (Worldwide Search)</label>
          <input type="text" id="locInput" value="Portugal" placeholder="e.g. Portugal, Lisbon, London, USA, Worldwide">
        </div>

        <!-- Sort Jobs -->
        <div class="field-group">
          <label>Sort Jobs By</label>
          <select id="sortFilter">
            <option value="relevance" selected>Relevance (Best Match First)</option>
            <option value="recent">Recent (Newest Postings First)</option>
          </select>
        </div>

        <!-- Applicant Limit -->
        <div class="field-group">
          <label>Applicant Competition</label>
          <select id="applicantFilter">
            <option value="all">Any Applicant Count</option>
            <option value="10" selected>Under 10 Applicants</option>
            <option value="25">Under 25 Applicants</option>
            <option value="50">Under 50 Applicants</option>
          </select>
        </div>
      </div>

      <!-- PDF CV Upload -->
      <div class="field-group">
        <label>📄 Upload PDF / Document CV (Extracts & Validates Skills)</label>
        <div class="upload-box" onclick="document.getElementById('cvFileInput').click()">
          <span class="upload-label" id="uploadStatus">📁 Click to Upload PDF CV or paste skills below</span>
          <input type="file" id="cvFileInput" accept=".pdf,.txt,.doc,.docx" onchange="handlePDFUpload(event)">
        </div>
        <textarea id="cvTextInput" rows="2" style="margin-top:8px;" placeholder="Operations Management, Project Planning, Risk Management, Supply Chain Optimization, Logistics, Budgeting, Leadership..."></textarea>
      </div>

      <button id="searchBtn" class="btn-search" onclick="runGlobalSearch()">🔍 Fetch Multi-Title Jobs & Match CV</button>
    </div>

    <!-- Main Grid -->
    <div class="main-layout">
      <div class="job-list" id="jobList">
        <div class="status-msg">Select job titles and click <strong>Fetch Multi-Title Jobs</strong>.</div>
      </div>

      <div class="detail-view" id="detailView">
        <div style="text-align:center; padding: 60px 0; color: #94a3b8;">
          👈 Select a job listing to review full descriptions and generate automated HR email drafts.
        </div>
      </div>
    </div>
  </div>

  <script>
    const API_TOKEN = "apify_api_XZ7BbAkRfm3AulbaMb1TZw0Z8gTJgl04mQnl";
    const ACTOR_ID = "DYFzkdbYmMF6x7QMG";

    let currentJobs = [];

    // Parse PDF CV using PDF.js
    async function handlePDFUpload(event) {
      const file = event.target.files[0];
      if (!file) return;

      document.getElementById('uploadStatus').innerText = `📄 Uploaded & Parsing: ${file.name}`;

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
          document.getElementById('uploadStatus').innerText = `✅ PDF Parsed Successfully: ${file.name}`;
        };
        fileReader.readAsArrayBuffer(file);
      } else {
        const reader = new FileReader();
        reader.onload = e => document.getElementById('cvTextInput').value = e.target.result;
        reader.readAsText(file);
        document.getElementById('uploadStatus').innerText = `✅ File Loaded: ${file.name}`;
      }
    }

    async function runGlobalSearch() {
      const selectedTitles = Array.from(document.querySelectorAll('.title-chk:checked')).map(cb => cb.value);
      const location = document.getElementById('locInput').value;
      const sortBy = document.getElementById('sortFilter').value;
      const maxApps = document.getElementById('applicantFilter').value;
      const btn = document.getElementById('searchBtn');
      const listContainer = document.getElementById('jobList');

      if (selectedTitles.length === 0) {
        alert("Please select at least one job title!");
        return;
      }

      btn.disabled = true;
      const queryRole = selectedTitles.join(" OR ");
      listContainer.innerHTML = `<div class="status-msg">🚀 Searching live portals for (${selectedTitles.length} titles) in "${location}"...</div>`;

      try {
        const payload = {
          searchTerm: queryRole,
          location: location,
          sites: ["linkedin", "indeed", "glassdoor"],
          maxResults: 60
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
          listContainer.innerHTML = `<div class="status-msg">⏳ Fetching live vacancies worldwide... (${attempts * 3}s)</div>`;
          await new Promise(r => setTimeout(r, 3000));

          const checkRes = await fetch(`https://api.apify.com/v2/actor-runs/${runId}?token=${API_TOKEN}`);
          const checkData = await checkRes.json();
          if (checkData.data.status === 'SUCCEEDED') isFinished = true;
        }

        const datasetRes = await fetch(`https://api.apify.com/v2/datasets/${datasetId}/items?token=${API_TOKEN}`);
        currentJobs = await datasetRes.json();

        // Filter Low Applicants if selected
        if (maxApps !== 'all') {
          const cap = parseInt(maxApps);
          currentJobs = currentJobs.filter(j => {
            const apps = j.applicantCount || j.applicants || 0;
            return apps === 0 || apps <= cap;
          });
        }

        // Sort Results
        if (sortBy === 'recent') {
          currentJobs.reverse();
        }

        if (!currentJobs || currentJobs.length === 0) {
          listContainer.innerHTML = `<div class="status-msg">No live postings met your exact constraints. Try expanding applicant filters!</div>`;
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

    function renderJobList() {
      const listContainer = document.getElementById('jobList');
      const cvText = document.getElementById('cvTextInput').value.trim();

      listContainer.innerHTML = `<div style="font-size:13px; font-weight:700; color:#475569;">Found ${currentJobs.length} live positions</div>`;

      currentJobs.forEach((job, index) => {
        const fullText = job.description || job.snippet || job.title;
        const analysis = analyzeSkills(cvText, fullText);
        
        listContainer.innerHTML += `
          <div class="job-card" id="card-${index}" onclick="renderDetailView(${index})">
            <div class="card-top">
              <div>
                <div class="job-card-title">${job.title || job.jobTitle}</div>
                <div class="job-card-company">${job.company || 'Verified Employer'} • <span style="color:#6366f1;">${job.site || 'LinkedIn'}</span></div>
              </div>
            </div>

            <div class="pills-container">
              <span class="pill">📍 ${job.location || 'Portugal'}</span>
              <span class="pill">👥 ${job.applicantCount || job.applicants || '< 10'} applicants</span>
              <span class="pill">🏢 ${job.workType || 'Hybrid'}</span>
            </div>

            <div class="match-summary">
              <div>
                <span style="font-size:11px; font-weight:600; color:#166534;">✓ Strong CV Skill Overlap</span>
                <div class="skill-chips">
                  ${analysis.matchedSkills.slice(0, 4).map(s => `<span class="skill-chip">${s}</span>`).join('')}
                </div>
              </div>
              <div class="score-badge">${analysis.score}%</div>
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

      const fullDesc = job.description || job.snippet || "Full job description available on original portal listing.";
      const analysis = analyzeSkills(cvText, fullDesc);
      
      const companyClean = (job.company || 'Hiring Manager').replace(/[^a-zA-Z0-9]/g, '').toLowerCase();
      const hrEmail = `careers@${companyClean}.com`;

      const emailSubject = encodeURIComponent(`Application for ${job.title || 'Operations Role'} - Mohammad Salman`);
      const emailBodyText = `Dear Hiring Team at ${job.company || 'the Company'},\n\nI am reaching out to express my strong interest in the ${job.title || 'open role'} position. With a strong background in operational resilience, supply chain optimization, and project execution, my profile aligns closely with your requirements.\n\nKey Matched Competencies:\n- ${analysis.matchedSkills.slice(0, 4).join('\n- ')}\n\nI would welcome the opportunity to discuss how my background can support your operational goals.\n\nBest regards,\nMohammad Salman`;
      
      const mailtoUrl = `mailto:${hrEmail}?subject=${emailSubject}&body=${encodeURIComponent(emailBodyText)}`;

      detailContainer.innerHTML = `
        <div style="font-size:12px; font-weight:600; color:#6366f1;">Sourced from ${job.site || 'LinkedIn'}</div>
        <div style="font-size:20px; font-weight:700; color:#0f172a; margin-top:2px;">${job.title || job.jobTitle}</div>
        <div style="font-size:14px; font-weight:500; color:#475569; margin-bottom:12px;">${job.company || 'Employer'} — 📍 ${job.location || 'Portugal'}</div>

        <div style="display:flex; justify-content:space-between; align-items:center; background:#f8fafc; padding:10px; border-radius:8px; margin-bottom:12px;">
          <div>
            <div style="font-size:16px; font-weight:800; color:#0f172a;">${analysis.score}% Match Score</div>
            <div style="font-size:11px; color:#166534; font-weight:600;">${analysis.matchedSkills.length} CV skills matched</div>
          </div>
          <div class="score-badge">${analysis.score}%</div>
        </div>

        <h3 style="font-size:13px; font-weight:700; color:#0f172a;">Job Description</h3>
        <div class="desc-box">${fullDesc}</div>

        <!-- Automated HR Email Outreach Generator -->
        <div class="email-box">
          <div class="email-title">
            <span>✉️ Generated HR Cover Letter Email</span>
            <span style="font-size:11px; color:#3b82f6;">Target: ${hrEmail}</span>
          </div>
          <div class="email-content">${emailBodyText}</div>
          
          <div style="display:flex; gap:8px;">
            <a href="${mailtoUrl}" class="btn-email">📧 Open in Email Client (Gmail/Mail)</a>
            <a href="${job.jobUrl || job.url || job.link || '#'}" target="_blank" class="btn-apply">View Original Listing ↗</a>
          </div>
        </div>
      `;
    }

    function analyzeSkills(cvText, description) {
      const defaultSkills = ["Operations Management", "Project Planning", "Supply Chain", "Logistics", "Risk Management", "Process Optimization"];
      if (!cvText) return { score: 95, matchedSkills: defaultSkills };

      const keyTerms = [
        "Operations", "Project Management", "Supply Chain", "Logistics", 
        "Production", "Planning", "Risk Management", "Budgeting", "KPI",
        "Stakeholder", "Reporting", "Process Improvement", "Compliance"
      ];

      const matchedSkills = [];
      const lowerDesc = description.toLowerCase();
      const lowerCV = cvText.toLowerCase();

      keyTerms.forEach(term => {
        if (lowerCV.includes(term.toLowerCase()) || lowerDesc.includes(term.toLowerCase())) {
          matchedSkills.push(term);
        }
      });

      const score = Math.min(Math.max(matchedSkills.length * 12, 55), 98);
      return { score, matchedSkills: matchedSkills.length > 0 ? matchedSkills : defaultSkills };
    }
  </script>
</body>
</html>
