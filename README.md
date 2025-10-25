   <!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Вакансии в Азербайджане</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; background: #f9f9f9; }
    h1 { text-align: center; color: #333; }
    ul { list-style-type: none; padding: 0; }
    li { margin-bottom: 10px; background: #fff; padding: 10px; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
    a { text-decoration: none; color: #007BFF; }
    a:hover { text-decoration: underline; }
    small { color: #555; margin-left: 10px; font-size: 0.9em; }
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
        console.warn('Ошибка загрузки RSS:', url);
        return [];
      }
    }

    async function loadAll() {
      const list = document.getElementById('jobList');
      list.innerHTML = 'Загрузка вакансий...';

      const allItems = await Promise.all(rssLinks.map(url => loadRSS(url)));
      const flatItems = allItems.flat();

      if (flatItems.length === 0) {
        list.innerHTML = 'Вакансии пока не найдены.';
        return;
      }

      // Сортировка по дате публикации (новые сверху)
      flatItems.sort((a, b) => new Date(b.pubDate) - new Date(a.pubDate));

      list.innerHTML = '';
      flatItems.forEach(item => {
        const li = document.createElement('li');
        li.innerHTML = `<a href="${item.link}" target="_blank">${item.title}</a>
                        <small>${item.pubDate ? new Date(item.pubDate).toLocaleDateString() : ''}</small>`;
        list.appendChild(li);
      });
    }

    loadAll();
  </script>
</body>
</html>   'https://www.azjobs.az/rss',
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
