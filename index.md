---
layout: default
title: AI Job Search & Portfolio
---

<style>
  .search-container { background: #f4f6f9; padding: 20px; border-radius: 10px; margin-bottom: 25px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); }
  .search-grid { display: grid; grid-template-columns: 2fr 1fr 1fr auto; gap: 10px; margin-bottom: 15px; }
  .search-grid input, .search-grid select { padding: 10px; border: 1px solid #ccc; border-radius: 6px; font-size: 14px; }
  .btn-search { background: #0066cc; color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; }
  .btn-search:hover { background: #004c99; }
  .job-card { background: white; border: 1px solid #e1e4e8; padding: 18px; border-radius: 8px; margin-bottom: 15px; display: flex; justify-content: space-between; align-items: center; }
  .match-badge { background: #e6f4ea; color: #137333; font-weight: bold; padding: 5px 10px; border-radius: 20px; font-size: 13px; }
  .status-tag { display: inline-block; padding: 3px 8px; font-size: 12px; border-radius: 4px; background: #eee; }
</style>

# 🔍 AI Job Search & Career Tracker

<div class="search-container">
  <h3>Automated Job Search & Match Engine</h3>
  <div class="search-grid">
    <input type="text" id="jobTitle" placeholder="Job Title or Keyword (e.g., Operations Manager)...">
    <input type="text" id="jobLocation" placeholder="Location (e.g., Europe, Remote)...">
    <select id="matchFilter">
      <option value="0">All Match Scores</option>
      <option value="80">80%+ Match Only</option>
      <option value="90">90%+ High Match</option>
    </select>
    <button class="btn-search" onclick="filterJobs()">Search Jobs</button>
  </div>
</div>

## 🎯 Matching Job Opportunities

<div id="jobList">
  <!-- Dynamic Job Cards will render here -->
</div>

<script>
  const sampleJobs = [
    { title: "Operations & Logistics Manager", company: "Global Supply Solutions", location: "Lisbon, Portugal / Hybrid", match: 94, link: "#" },
    { title: "Supply Chain Analyst", company: "TechLogistics Europe", location: "Remote", match: 88, link: "#" },
    { title: "Production Operations Coordinator", company: "Batches Electronics Partner", location: "Europe", match: 82, link: "#" },
    { title: "Process Improvement Specialist", company: "Euro Distribution", location: "Remote", match: 75, link: "#" }
  ];

  function renderJobs(jobs) {
    const container = document.getElementById('jobList');
    container.innerHTML = '';
    
    if(jobs.length === 0) {
      container.innerHTML = '<p>No matching roles found. Try adjusting your search filters.</p>';
      return;
    }

    jobs.forEach(job => {
      container.innerHTML += `
        <div class="job-card">
          <div>
            <h3 style="margin: 0 0 5px 0;">${job.title}</h3>
            <p style="margin: 0; color: #555;"><strong>${job.company}</strong> • ${job.location}</p>
          </div>
          <div style="text-align: right;">
            <span class="match-badge">${job.match}% AI Match</span><br><br>
            <a href="${job.link}" target="_blank" style="background:#24292e; color:white; padding:6px 12px; border-radius:4px; text-decoration:none; font-size:13px;">View Job</a>
          </div>
        </div>
      `;
    });
  }

  function filterJobs() {
    const titleInput = document.getElementById('jobTitle').value.toLowerCase();
    const locationInput = document.getElementById('jobLocation').value.toLowerCase();
    const minMatch = parseInt(document.getElementById('matchFilter').value);

    const filtered = sampleJobs.filter(job => {
      const matchesTitle = job.title.toLowerCase().includes(titleInput) || job.company.toLowerCase().includes(titleInput);
      const matchesLocation = job.location.toLowerCase().includes(locationInput);
      const matchesScore = job.match >= minMatch;
      return matchesTitle && matchesLocation && matchesScore;
    });

    renderJobs(filtered);
  }

  // Initial Render
  renderJobs(sampleJobs);
</script>
