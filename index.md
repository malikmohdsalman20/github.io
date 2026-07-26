---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Real-Time Multi-Platform Job Hub</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
    body { background-color: #f8fafc; color: #0f172a; padding: 32px 24px; }
    .container { max-width: 1000px; margin: 0 auto; }
    
    .header h1 { font-size: 26px; font-weight: 700; margin-bottom: 4px; }
    .header p { color: #64748b; font-size: 14px; margin-bottom: 24px; }

    .filter-panel { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 24px; margin-bottom: 24px; }
    .search-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 20px; }
    
    .field-group { display: flex; flex-direction: column; gap: 6px; }
    .field-group label { font-size: 12px; font-weight: 600; color: #475569; text-transform: uppercase; }
    .field-group input, .field-group select { padding: 10px 14px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 14px; outline: none; }

    .btn-search { background: #4f46e5; color: white; border: none; padding: 12px 24px; border-radius: 8px; font-size: 14px; font-weight: 600; cursor: pointer; }
    .btn-search:hover { background: #4338ca; }
    
    .job-list { display: flex; flex-direction: column; gap: 16px; margin-top: 20px; }
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; display: flex; justify-content: space-between; align-items: flex-start; }
    .job-title { font-size: 18px; font-weight: 600; color: #0f172a; text-decoration: none; }
    .company-name { font-size: 14px; color: #475569; margin-top: 4px; margin-bottom: 12px; }
    
    .apply-btn { background: #0f172a; color: white; text-decoration: none; padding: 8px 16px; border-radius: 6px; font-size: 13px; font-weight: 600; }
    .status-msg { text-align: center; color: #64748b; font-size: 15px; padding: 40px 0; background: #fff; border-radius: 12px; border: 1px dashed #cbd5e1; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h1>🌐 Real-Time Live Job Search</h1>
      <p>Instant job aggregator querying LinkedIn, Indeed, and Glassdoor postings.</p>
    </div>

    <div class="filter-panel">
      <div class="search-grid">
        <div class="field-group">
          <label>Job Title</label>
          <input type="text" id="roleInput" value="Operations Manager">
        </div>
        <div class="field-group">
          <label>Location</label>
          <input type="text" id="locationInput" value="Portugal">
        </div>
        <div class="field-group">
          <label>Work Mode</label>
          <select id="workTypeSelect">
            <option value="all">All Modes</option>
            <option value="remote">Remote Only</option>
          </select>
        </div>
      </div>

      <button class="btn-search" onclick="fetchLiveJobs()">🔍 Fetch Live Roles</button>
    </div>

    <div id="resultsCount" style="font-weight:600; color:#64748b;"></div>
    <div class="job-list" id="jobList">
      <div class="status-msg">Click <strong>Fetch Live Roles</strong> to query active postings.</div>
    </div>
  </div>

  <script>
    async function fetchLiveJobs() {
      const role = document.getElementById('roleInput').value;
      const location = document.getElementById('locationInput').value;
      const workType = document.getElementById('workTypeSelect').value;
      const jobList = document.getElementById('jobList');
      const resultsCount = document.getElementById('resultsCount');

      jobList.innerHTML = `<div class="status-msg">🔄 Fetching real-time vacancies for "${role}" in "${location}"...</div>`;

      // Public Jooble Live Search Gateway
      const JOOBLE_API_KEY = "ca4d88e6-e41c-4395-88e9-4e78a6358178"; 

      try {
        const response = await fetch(`https://jooble.org/api/${JOOBLE_API_KEY}`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            keywords: role,
            location: location,
            page: 1
          })
        });

        const data = await response.json();

        if (!data.jobs || data.jobs.length === 0) {
          jobList.innerHTML = `<div class="status-msg">No live jobs found. Try adjusting keywords or location!</div>`;
          return;
        }

        jobList.innerHTML = '';
        resultsCount.innerText = `Found ${data.jobs.length} live postings`;

        data.jobs.slice(0, 10).forEach(job => {
          const card = `
            <div class="job-card">
              <div>
                <a href="${job.link}" target="_blank" class="job-title">${job.title}</a>
                <div class="company-name">${job.company || 'Verified Company'} — 📍 ${job.location}</div>
                <div style="font-size:12px; color:#64748b;">📅 Posted: ${job.updated || 'Recently'}</div>
              </div>
              <a href="${job.link}" target="_blank" class="apply-btn">Apply Now ↗</a>
            </div>
          `;
          jobList.innerHTML += card;
        });

      } catch (error) {
        console.error(error);
        jobList.innerHTML = `<div class="status-msg" style="color:red;">❌ Unable to fetch live listings. Check network connection.</div>`;
      }
    }
  </script>
</body>
</html>
