---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Real-Time Multi-Board Job Search</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
    body { background-color: #f8fafc; color: #0f172a; padding: 32px 24px; }
    .container { max-width: 900px; margin: 0 auto; }
    .header h1 { font-size: 26px; font-weight: 700; margin-bottom: 4px; }
    .header p { color: #64748b; font-size: 14px; margin-bottom: 24px; }

    .search-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; margin-bottom: 24px; display: flex; gap: 12px; }
    .search-input { flex: 1; padding: 10px 14px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 14px; outline: none; }
    .search-btn { background: #4f46e5; color: white; border: none; padding: 10px 20px; border-radius: 8px; font-weight: 600; cursor: pointer; }
    .search-btn:hover { background: #4338ca; }
    .search-btn:disabled { background: #94a3b8; cursor: not-allowed; }

    .job-list { display: flex; flex-direction: column; gap: 16px; }
    .job-card { background: #ffffff; border: 1px solid #e2e8f0; border-radius: 12px; padding: 20px; display: flex; justify-content: space-between; align-items: flex-start; }
    .job-title { font-size: 18px; font-weight: 600; color: #0f172a; text-decoration: none; }
    .company-name { font-size: 14px; color: #475569; margin-top: 4px; margin-bottom: 8px; }
    .tag { font-size: 11px; font-weight: 600; padding: 3px 8px; border-radius: 12px; background: #e0e7ff; color: #3730a3; text-transform: capitalize; }
    .apply-btn { background: #0f172a; color: white; text-decoration: none; padding: 8px 16px; border-radius: 6px; font-size: 13px; font-weight: 600; }
    .status-box { text-align: center; padding: 40px; color: #64748b; font-size: 15px; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h1>⚡ Live Job Search (Direct Apify API)</h1>
      <p>Triggers real-time cloud scraping across LinkedIn, Indeed, Glassdoor, and Google Jobs.</p>
    </div>

    <div class="search-card">
      <input type="text" id="roleInput" class="search-input" value="Operations Manager" placeholder="Job Title">
      <input type="text" id="locInput" class="search-input" value="Portugal" placeholder="Location">
      <button id="searchBtn" class="search-btn" onclick="startRealtimeSearch()">🔍 Search Jobs Live</button>
    </div>

    <div class="job-list" id="jobList">
      <div class="status-box">Click <strong>Search Jobs Live</strong> to scrape jobs in real time.</div>
    </div>
  </div>

  <script>
    const API_TOKEN = "apify_api_XZ7BbAkRfm3AulbaMb1TZw0Z8gTJgl04mQnl";
    const ACTOR_ID = "DYFzkdbYmMF6x7QMG"; // Multi Job Board Scraper

    async function startRealtimeSearch() {
      const role = document.getElementById('roleInput').value;
      const location = document.getElementById('locInput').value;
      const btn = document.getElementById('searchBtn');
      const container = document.getElementById('jobList');

      btn.disabled = true;
      container.innerHTML = `<div class="status-box">🚀 Launching Apify Cloud Scraper for "${role}" in "${location}"...</div>`;

      try {
        // Step 1: Start Actor Run (Asynchronous)
        const startResponse = await fetch(`https://api.apify.com/v2/acts/${ACTOR_ID}/runs?token=${API_TOKEN}`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            searchTerm: role,
            location: location,
            sites: ["linkedin", "indeed", "glassdoor", "google"],
            maxResults: 10
          })
        });

        const startData = await startResponse.json();
        const runId = startData.data.id;
        const datasetId = startData.data.defaultDatasetId;

        // Step 2: Poll status until finished
        let isFinished = false;
        let attempts = 0;

        while (!isFinished && attempts < 30) {
          attempts++;
          container.innerHTML = `<div class="status-box">⏳ Scraping active job boards on Apify cloud... (${attempts * 3}s elapsed)</div>`;
          
          await new Promise(resolve => setTimeout(resolve, 3000)); // wait 3 seconds

          const checkResponse = await fetch(`https://api.apify.com/v2/actor-runs/${runId}?token=${API_TOKEN}`);
          const checkData = await checkResponse.json();
          
          if (checkData.data.status === 'SUCCEEDED') {
            isFinished = true;
          } else if (['FAILED', 'ABORTED', 'TIMED-OUT'].includes(checkData.data.status)) {
            throw new Error(`Scraper run failed with status: ${checkData.data.status}`);
          }
        }

        // Step 3: Fetch Dataset Items
        container.innerHTML = `<div class="status-box">✅ Scrape complete! Fetching dataset...</div>`;
        const datasetResponse = await fetch(`https://api.apify.com/v2/datasets/${datasetId}/items?token=${API_TOKEN}`);
        const jobs = await datasetResponse.json();

        if (!jobs || jobs.length === 0) {
          container.innerHTML = `<div class="status-box">No jobs found for this search. Try different keywords!</div>`;
          btn.disabled = false;
          return;
        }

        // Step 4: Render Jobs
        container.innerHTML = '';
        jobs.forEach(job => {
          container.innerHTML += `
            <div class="job-card">
              <div>
                <a href="${job.jobUrl || job.url || job.link || '#'}" target="_blank" class="job-title">${job.title || job.jobTitle || 'Job Listing'}</a>
                <div class="company-name">🏢 ${job.company || job.companyName || 'Verified Employer'} — 📍 ${job.location || location}</div>
                <span class="tag">Source: ${job.site || job.source || 'Job Board'}</span>
              </div>
              <a href="${job.jobUrl || job.url || job.link || '#'}" target="_blank" class="apply-btn">Apply Now ↗</a>
            </div>
          `;
        });

      } catch (error) {
        console.error(error);
        container.innerHTML = `<div class="status-box" style="color:red;">❌ Error: ${error.message}</div>`;
      } finally {
        btn.disabled = false;
      }
    }
  </script>
</body>
</html>
