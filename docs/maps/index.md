---
layout: page
title: Map Viewer
permalink: /maps/
---

<link rel="stylesheet" href="{{ '/assets/css/maps.css' | relative_url }}">

Browse campaign maps. Use the arrows or your keyboard's ← → keys to step through. To see every map on one page in chronological order, go to the [Timeline]({{ '/timeline/' | relative_url }}).

<div id="map-viewer">
  <div class="map-info">
    <h2 id="map-title">Loading…</h2>
    <p id="map-meta"></p>
  </div>
  <div class="map-frame">
    <div class="map-canvas">
      <a id="prev-link" class="map-arrow map-arrow-prev" href="#" aria-label="Previous map">←</a>
      <img id="map-image" src="" alt="" />
      <a id="next-link" class="map-arrow map-arrow-next" href="#" aria-label="Next map">→</a>
    </div>
  </div>
  <p class="map-counter"><span id="map-pos">–</span> of <span id="map-total">–</span></p>
</div>

{% include maps-data.html %}

<script>
(function() {
  const maps = JSON.parse(document.getElementById('maps-data').textContent);
  const games = window.__campaignGames || [];
  const gameOrder = games.map(g => g.key);
  const gameMeta = {};
  games.forEach(g => { gameMeta[g.key] = g; });

  maps.forEach(m => {
    m.sessionNum = parseInt((m.session || '').replace(/^Session/i, ''), 10) || 0;
    m.gameIdx = gameOrder.indexOf(m.filename_game);
    if (m.gameIdx === -1) m.gameIdx = 999;
  });
  maps.sort((a, b) => (a.gameIdx - b.gameIdx) || (a.sessionNum - b.sessionNum));

  const titleEl = document.getElementById('map-title');
  const metaEl  = document.getElementById('map-meta');
  const imgEl   = document.getElementById('map-image');
  const prevEl  = document.getElementById('prev-link');
  const nextEl  = document.getElementById('next-link');
  const posEl   = document.getElementById('map-pos');
  const totalEl = document.getElementById('map-total');

  if (maps.length === 0) {
    titleEl.textContent = 'No maps yet';
    metaEl.textContent  = 'Drop image files into docs/assets/maps/ using the naming convention Game_SessionN_Year (e.g. CK3_Session5_1134.png).';
    imgEl.style.display = 'none';
    prevEl.style.visibility = 'hidden';
    nextEl.style.visibility = 'hidden';
    posEl.textContent = '0';
    totalEl.textContent = '0';
    return;
  }

  totalEl.textContent = maps.length;

  function formatYear(calendar, year) {
    if (calendar === 'AUC') {
      const auc = parseInt(year, 10);
      if (!isNaN(auc) && auc >= 1 && auc <= 753) {
        return 'AUC ' + auc + ' (' + (754 - auc) + ' BC)';
      }
      if (!isNaN(auc) && auc >= 754) {
        return 'AUC ' + auc + ' (' + (auc - 753) + ' AD)';
      }
    }
    return year + ' ' + calendar;
  }

  function findIndex(hash) {
    if (!hash) return 0;
    const exact = maps.findIndex(m => m.id === hash);
    if (exact !== -1) return exact;
    const game = maps.findIndex(m => m.filename_game === hash);
    if (game !== -1) return game;
    return 0;
  }

  function render(idx) {
    const m = maps[idx];
    const meta = gameMeta[m.filename_game] || { name: m.filename_game, calendar: 'CE' };
    const yearStr = formatYear(meta.calendar, m.year);
    const altText = meta.name + ' — Session ' + m.sessionNum + ' — ' + yearStr;
    imgEl.src = m.src;
    imgEl.alt = altText;
    titleEl.textContent = meta.name + ' — Session ' + m.sessionNum;
    metaEl.textContent  = yearStr;
    posEl.textContent   = idx + 1;

    const prevIdx = (idx - 1 + maps.length) % maps.length;
    const nextIdx = (idx + 1) % maps.length;
    prevEl.href = '#' + maps[prevIdx].id;
    nextEl.href = '#' + maps[nextIdx].id;

    const desiredHash = '#' + m.id;
    if (location.hash !== desiredHash) {
      history.replaceState(null, '', desiredHash);
    }
  }

  function showFromHash() {
    const hash = decodeURIComponent(location.hash.replace(/^#/, ''));
    render(findIndex(hash));
  }

  window.addEventListener('hashchange', showFromHash);
  document.addEventListener('keydown', function(e) {
    if (e.target && (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA')) return;
    if (e.key === 'ArrowLeft')  { e.preventDefault(); prevEl.click(); }
    if (e.key === 'ArrowRight') { e.preventDefault(); nextEl.click(); }
  });

  showFromHash();
})();
</script>
