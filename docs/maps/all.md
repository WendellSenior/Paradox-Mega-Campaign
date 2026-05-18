---
layout: page
title: All Maps
permalink: /maps/all/
---

<link rel="stylesheet" href="{{ '/assets/css/maps.css' | relative_url }}">

Every campaign map, in chain order. Return to the [single-map viewer]({{ '/maps/' | relative_url }}).

<div id="map-list"><p>Loading…</p></div>

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

  const list = document.getElementById('map-list');
  if (maps.length === 0) {
    list.innerHTML = '<p>No maps yet. Drop images into <code>docs/assets/maps/</code> using the naming convention <code>Game_SessionN_Year</code> (e.g. <code>CK3_Session5_1134.png</code>).</p>';
    return;
  }

  function escapeHtml(s) {
    return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
  }

  let currentGame = '';
  const html = [];
  maps.forEach(m => {
    const meta = gameMeta[m.filename_game] || { name: m.filename_game, calendar: 'CE' };
    if (m.filename_game !== currentGame) {
      currentGame = m.filename_game;
      html.push('<h2 id="game-' + escapeHtml(m.filename_game) + '">' + escapeHtml(meta.name) + '</h2>');
    }
    const altText = meta.name + ' — Session ' + m.sessionNum + ' — ' + meta.calendar + ' ' + m.year;
    html.push(
      '<figure class="map-entry" id="' + escapeHtml(m.id) + '">' +
        '<div class="map-frame"><img src="' + escapeHtml(m.src) + '" alt="' + escapeHtml(altText) + '" loading="lazy" /></div>' +
        '<figcaption>Session ' + m.sessionNum + ' — ' + escapeHtml(meta.calendar) + ' ' + escapeHtml(m.year) + '</figcaption>' +
      '</figure>'
    );
  });
  list.innerHTML = html.join('');

  if (location.hash) {
    const target = document.getElementById(decodeURIComponent(location.hash.replace(/^#/, '')));
    if (target) target.scrollIntoView({ behavior: 'auto', block: 'start' });
  }
})();
</script>
