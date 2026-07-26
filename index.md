---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Live Multi-Board Job Search</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
    body { background-color: #f8fafc; color: #0f172a; padding: 32px 24px; }
    .container { max-width: 900px; margin: 0 auto; }
    .header h1 { font-size: 26px; font-weight: 700; margin-bottom: 4px; }
    .header p { color: #64748b; font-size: 14px; margin-bottom: 24px; }
    .job-list { display: flex; flex-direction: column; gap: 16px; }
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; display: flex; justify-content: space-between; align-items: flex-start; }
    .job-title { font-size: 18px; font-weight: 600; color: #0f172a; text-decoration: none; }
    .company-name { font-size: 14px; color: #475569; margin-top: 4px; margin-bottom: 8px; }
    .tag { font-size: 11px; font-weight: 600; padding: 3px 8px; border-radius: 12px; background: #e0e7ff; color: #3730a3; text-transform: capitalize; }
    .apply-btn { background: #0f172a; color: white; text-decoration: none; padding: 8px 16px; border-radius: 6px; font-size: 13px; font-weight: 600; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h1>🌐 Multi-Platform Live Job Feed</h1>
      <p>Aggregated from LinkedIn, Indeed, Glassdoor, and Google Jobs.</p>
    </div>

    <div class="job-list" id="jobList">
      <div style="text-align:center; padding:40px; color:#64748b;">🔄 Loading live listings...</div>
    </div>
  </div>

  <script>
    async function loadJobs() {
      const container = document.getElementById('jobList');
      try {
        const response = await fetch('./jobs.json');
        const jobs = await response.json();

        if (!jobs || jobs.length === 0) {
          container.innerHTML = '<div style="text-align:center; color:#64748b;">No listings found. Run python3 apify_search.py to generate data!</div>';
          return;
        }

        container.innerHTML = '';
        jobs.forEach(job => {
          container.innerHTML += `
            <div class="job-card">
              <div>
                <a href="${job.link}" target="_blank" class="job-title">${job.title}</a>
                <div class="company-name">🏢 ${job.company} — 📍 ${job.location}</div>
                <span class="tag">Source: ${job.site}</span>
              </div>
              <a href="${job.link}" target="_blank" class="apply-btn">View Role ↗</a>
            </div>
          `;
        });
      } catch (e) {
        container.innerHTML = '<div style="text-align:center; color:red;">Could not load jobs.json. Make sure you ran the script and pushed jobs.json to GitHub!</div>';
      }
    }

    loadJobs();
  </script>
</body>
</html>
