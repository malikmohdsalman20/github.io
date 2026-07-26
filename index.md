---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Real-Time Job Hub (Apify Powered)</title>
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
    .field-group input { padding: 10px 14px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 14px; outline: none; }

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
      <h1>🌐 Real-Time Apify Job Search</h1>
      <p>Queries live LinkedIn postings using your active Apify API key.</p>
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
      </div>

      <button class="btn-search" onclick="runApifyScraper()">🔍 Run Apify Scraper</button>
    </div>

    <div id="resultsCount" style="font-weight:600; color:#64748b;"></div>
    <div class="job-list" id="jobList">
      <div class="status-msg">Click <strong>Run Apify Scraper</strong> to start live data extraction.</div>
    </div>
  </div>

  <script>
    const APIFY_TOKEN = "apify_api_XZ7BbAkRfm3AulbaMb1TZw0Z8gTJgl04mQnl";

    async function runApifyScraper() {
      const role = document.getElementById('roleInput').value;
      const location = document.getElementById('locationInput').value;
      const jobList = document.getElementById('jobList');
      const resultsCount = document.getElementById('resultsCount');

      jobList.innerHTML = `<div class="status-msg">⏳ Triggering Apify cloud actor... Searching LinkedIn for "${role}" in "${location}"...</div>`;

      // Synchronous API call to execute the Actor and fetch results directly
      const actorUrl = `https://api.apify.com/v2/acts/crawlworks~linkedin-jobs-scraper/run-sync-get-dataset-items?token=${APIFY_TOKEN}`;

      // Payload containing required jobsToFetch parameter
      const payload = {
        title: role,
        location: location,
        jobsToFetch: 5
      };

      try {
        const response = await fetch(actorUrl, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(payload)
        });

        const jobs = await response.json();

        if (!Array.isArray(jobs) || jobs.length === 0) {
          jobList.innerHTML = `<div class="status-msg">No results returned or run timed out. Try refining your search terms!</div>`;
          return;
        }

        jobList.innerHTML = '';
        resultsCount.innerText = `Found ${jobs.length} live job postings`;

        jobs.forEach(job => {
          const card = `
            <div class="job-card">
              <div>
                <a href="${job.jobUrl || job.applyUrl || '#'}" target="_blank" class="job-title">${job.jobTitle || job.title || role}</a>
                <div class="company-name">${job.companyName || 'Verified Employer'} — 📍 ${job.location || location}</div>
              </div>
              <a href="${job.jobUrl || job.applyUrl || '#'}" target="_blank" class="apply-btn">View Listing ↗</a>
            </div>
          `;
          jobList.innerHTML += card;
        });

      } catch (error) {
        console.error(error);
        jobList.innerHTML = `<div class="status-msg" style="color:red;">❌ Error running Apify Actor. Check console logs.</div>`;
      }
    }
  </script>
</body>
</html>
