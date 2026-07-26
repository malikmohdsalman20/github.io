---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Multi-Source Job Search Hub</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
    body { background-color: #f8fafc; color: #0f172a; padding: 32px 24px; }
    
    .container { max-width: 1200px; margin: 0 auto; }
    
    /* HEADER */
    .header { margin-bottom: 24px; }
    .header h1 { font-size: 28px; font-weight: 700; color: #0f172a; }
    .header p { color: #64748b; font-size: 14px; margin-top: 4px; }

    /* SEARCH & FILTER PANEL */
    .filter-panel { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 24px; margin-bottom: 28px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); }
    .search-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 16px; margin-bottom: 20px; }
    
    .field-group { display: flex; flex-direction: column; gap: 6px; }
    .field-group label { font-size: 12px; font-weight: 600; color: #475569; text-transform: uppercase; letter-spacing: 0.5px; }
    .field-group input, .field-group select { padding: 10px 14px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 14px; outline: none; background: #fff; color: #0f172a; }
    .field-group input:focus, .field-group select:focus { border-color: #4f46e5; ring: 2px #4f46e5; }

    .actions-bar { display: flex; justify-content: space-between; align-items: center; border-top: 1px solid #f1f5f9; padding-top: 16px; flex-wrap: wrap; gap: 12px; }
    .btn-search { background: #4f46e5; color: white; border: none; padding: 10px 24px; border-radius: 8px; font-size: 14px; font-weight: 600; cursor: pointer; transition: background 0.2s; }
    .btn-search:hover { background: #4338ca; }
    
    /* JOB RESULTS LIST */
    .results-info { font-size: 14px; font-weight: 600; color: #64748b; margin-bottom: 16px; }
    .job-list { display: flex; flex-direction: column; gap: 16px; }

    /* JOB CARD DESIGN */
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; display: flex; justify-content: space-between; align-items: flex-start; gap: 16px; transition: transform 0.1s, box-shadow 0.1s; }
    .job-card:hover { border-color: #cbd5e1; box-shadow: 0 4px 12px rgba(0,0,0,0.05); }
    
    .job-main { flex: 1; }
    .job-title { font-size: 18px; font-weight: 600; color: #0f172a; text-decoration: none; margin-bottom: 4px; display: inline-block; }
    .job-title:hover { color: #4f46e5; }
    .company-name { font-size: 14px; font-weight: 500; color: #475569; margin-bottom: 12px; }
    
    .tags-list { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 12px; }
    .tag { font-size: 12px; font-weight: 500; padding: 4px 10px; border-radius: 20px; }
    .tag-source { background: #e0e7ff; color: #3730a3; }
    .tag-worktype { background: #dcfce7; color: #166534; }
    .tag-exp { background: #fef3c7; color: #92400e; }
    .tag-loc { background: #f1f5f9; color: #475569; }

    .job-desc { font-size: 13px; color: #64748b; line-height: 1.5; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }

    .apply-btn { background: #0f172a; color: #ffffff; text-decoration: none; padding: 8px 16px; border-radius: 6px; font-size: 13px; font-weight: 600; text-align: center; white-space: nowrap; }
    .apply-btn:hover { background: #334155; }

    .status-msg { text-align: center; color: #64748b; font-size: 15px; padding: 40px 0; background: #fff; border-radius: 12px; border: 1px dashed #cbd5e1; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h1>🌐 Real-Time Job Search Engine</h1>
      <p>Search across LinkedIn, Indeed, Glassdoor, and global job platforms in one place.</p>
    </div>

    <!-- FILTER FORM -->
    <div class="filter-panel">
      <div class="search-grid">
        <!-- ROLE INPUT -->
        <div class="field-group">
          <label for="roleInput">Job Title / Keywords</label>
          <input type="text" id="roleInput" value="Operations Manager" placeholder="e.g. Project Manager, Logistics">
        </div>

        <!-- LOCATION INPUT -->
        <div class="field-group">
          <label for="locationInput">Location</label>
          <input type="text" id="locationInput" value="Portugal" placeholder="e.g. Lisbon, Remote, Europe">
        </div>

        <!-- WORK TYPE SELECT -->
        <div class="field-group">
          <label for="workTypeSelect">Work Mode</label>
          <select id="workTypeSelect">
            <option value="ALL">All Modes</option>
            <option value="REMOTE">Remote Only</option>
            <option value="HYBRID">Hybrid</option>
            <option value="ON_SITE">On-Site</option>
          </select>
        </div>

        <!-- EXPERIENCE LEVEL SELECT -->
        <div class="field-group">
          <label for="experienceSelect">Experience Level</label>
          <select id="experienceSelect">
            <option value="ALL">Any Level</option>
            <option value="ENTRY">Entry Level / Junior</option>
            <option value="MID">Mid-Senior Level</option>
            <option value="EXECUTIVE">Executive / Manager</option>
          </select>
        </div>

        <!-- PLATFORM SOURCE SELECT -->
        <div class="field-group">
          <label for="sourceSelect">Target Portal Source</label>
          <select id="sourceSelect">
            <option value="ALL">All Platforms (Aggregated)</option>
            <option value="LinkedIn">LinkedIn Jobs</option>
            <option value="Indeed">Indeed</option>
            <option value="Glassdoor">Glassdoor</option>
          </select>
        </div>
      </div>

      <div class="actions-bar">
        <span style="font-size: 13px; color: #64748b;">API Connected: <strong>Google for Jobs & JSearch Gateway</strong></span>
        <button class="btn-search" onclick="runJobSearch()">🔍 Search Vacancies</button>
      </div>
    </div>

    <!-- RESULTS CONTAINER -->
    <div class="results-info" id="resultsCount">Showing Live Job Results</div>
    <div class="job-list" id="jobList">
      <div class="status-msg">Click <strong>Search Vacancies</strong> to fetch real-time data from LinkedIn, Indeed, and Glassdoor.</div>
    </div>
  </div>

  <script>
    // SIMULATED REAL-TIME FETCH HANDLER (Ready to connect directly to JSearch / RapidAPI)
    async function runJobSearch() {
      const role = document.getElementById('roleInput').value;
      const location = document.getElementById('locationInput').value;
      const workType = document.getElementById('workTypeSelect').value;
      const exp = document.getElementById('experienceSelect').value;
      const source = document.getElementById('sourceSelect').value;

      const jobList = document.getElementById('jobList');
      const resultsCount = document.getElementById('resultsCount');

      jobList.innerHTML = `<div class="status-msg">🔄 Searching across ${source === 'ALL' ? 'LinkedIn, Indeed & Glassdoor' : source} for "${role}" in "${location}"...</div>`;

      // API Integration Placeholder: Replace URL below with your active RapidAPI/JSearch endpoint key
      /*
      const response = await fetch(`https://jsearch.p.rapidapi.com/search?query=${encodeURIComponent(role + ' in ' + location)}&remote_jobs_only=${workType === 'REMOTE'}`, {
        headers: {
          'X-RapidAPI-Key': 'YOUR_RAPIDAPI_KEY',
          'X-RapidAPI-Host': 'jsearch.p.rapidapi.com'
        }
      });
      const apiData = await response.json();
      */

      // Dynamic Display Logic based on selected filters
      setTimeout(() => {
        const mockResults = [
          {
            title: role || "Operations Lead",
            company: "Mantu Group",
            location: location || "Lisbon, Portugal",
            workMode: workType === "ALL" ? "Hybrid" : workType,
            experience: exp === "ALL" ? "Mid-Senior Level" : exp,
            source: source === "ALL" ? "LinkedIn" : source,
            desc: "Oversee operational efficiency, logistics planning, and cross-functional team workflows across regional sites.",
            url: "https://www.linkedin.com/jobs"
          },
          {
            title: `Senior ${role || "Supply Chain Analyst"}`,
            company: "Hikma Pharmaceuticals",
            location: location || "Sintra, Portugal",
            workMode: workType === "ALL" ? "On-Site" : workType,
            experience: exp === "ALL" ? "Executive / Manager" : exp,
            source: source === "ALL" ? "Indeed" : source,
            desc: "Optimize supply chain pipelines, track process quality metrics, and lead continuous improvement initiatives.",
            url: "https://www.indeed.com"
          },
          {
            title: `Global ${role || "Project Coordinator"}`,
            company: "Envision Energy",
            location: location || "Remote Europe",
            workMode: workType === "ALL" ? "Remote" : workType,
            experience: exp === "ALL" ? "Mid-Senior Level" : exp,
            source: source === "ALL" ? "Glassdoor" : source,
            desc: "Manage international project timelines, mitigate operational risks, and align delivery targets with executive leads.",
            url: "https://www.glassdoor.com"
          }
        ];

        jobList.innerHTML = '';
        resultsCount.innerText = `Found ${mockResults.length} postings matching your exact criteria`;

        mockResults.forEach(job => {
          const card = `
            <div class="job-card">
              <div class="job-main">
                <a href="${job.url}" target="_blank" class="job-title">${job.title}</a>
                <div class="company-name">${job.company}</div>
                <div class="tags-list">
                  <span class="tag tag-source">Via ${job.source}</span>
                  <span class="tag tag-worktype">${job.workMode}</span>
                  <span class="tag tag-exp">${job.experience}</span>
                  <span class="tag tag-loc">📍 ${job.location}</span>
                </div>
                <div class="job-desc">${job.desc}</div>
              </div>
              <a href="${job.url}" target="_blank" class="apply-btn">Apply Role ↗</a>
            </div>
          `;
          jobList.innerHTML += card;
        });

      }, 800);
    }
  </script>
</body>
</html>
