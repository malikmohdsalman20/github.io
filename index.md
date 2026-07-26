---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Live LinkedIn Job Feed</title>
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
    .apply-btn { background: #0f172a; color: white; text-decoration: none; padding: 8px 16px; border-radius: 6px; font-size: 13px; font-weight: 600; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h1>🌐 Real-Time Scraped LinkedIn Jobs</h1>
      <p>Live listings automatically updated via Apify Automation.</p>
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

        container.innerHTML = '';
        jobs.forEach(job => {
          container.innerHTML += `
            <div class="job-card">
              <div>
                <a href="${job.link}" target="_blank" class="job-title">${job.title}</a>
                <div class="company-name">🏢 ${job.company} — 📍 ${job.location}</div>
              </div>
              <a href="${job.link}" target="_blank" class="apply-btn">View Role ↗</a>
            </div>
          `;
        });
      } catch (e) {
        container.innerHTML = '<div style="text-align:center; color:red;">No jobs found yet. Run python3 apify_search.py to generate jobs.json!</div>';
      }
    }

    loadJobs();
  </script>
</body>
</html>
