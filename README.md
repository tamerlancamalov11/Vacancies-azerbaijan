<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Вакансии в Азербайджане</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    h1 { text-align: center; }
    ul { list-style-type: none; padding: 0; }
    li { margin-bottom: 10px; }
    a { text-decoration: none; color: #007BFF; }
    a:hover { text-decoration: underline; }
  </style>
</head>
<body>
  <h1>Вакансии в Азербайджане</h1>
  <ul id="jobList">Загрузка вакансий...</ul>

  <script>
    const rssLinks = [
      'https://scalestack.ai/jobboard/remote/country/azerbaijan/all?format=rss',
      'https://az.jobisland.com/rss/jobs',
      'https://www.azjobs.az/rss',
      'https://www.jobsearch.az/rss',
      'https://www.jobportal.az/rss',
      'https://www.jobfinder.az/rss',
      'https://www.jobs.az/rss',
      'https://www.work.az/rss',
      'https://www.career.az/rss',
      'https://www.boss.az/rss'
    ];

    async function loadRSS(url) {
      try {
        const res = await fetch('https://api.rss2json.com/v1/api.json?rss_url=' + encodeURIComponent(url));
        const data = await res.json();
        return data.items || [];
      } catch (e) {
        console.log('Ошибка загрузки RSS:', url, e);
        return [];
      }
    }

    async function loadAll() {
      const list = document.getElementById('jobList');
      list.innerHTML = '';
      for (const url of rssLinks) {
        const items = await loadRSS(url);
        for (const item of items) {
          const li = document.createElement('li');
          li.innerHTML = `<a href="${item.link}" target="_blank">${item.title}</a>`;
          list.appendChild(li);
        }
      }
    }

    loadAll();
  </script>
</body>
</html>
