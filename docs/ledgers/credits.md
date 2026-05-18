---
layout: page
title: Credits Ledger
permalink: /ledgers/credits/
---

Current credit balance per player. Source: `docs/_data/credits.csv` — edit that file to update this page.

<table>
  <thead>
    <tr>
      <th>Player</th>
      <th>Balance</th>
      <th>Last Updated</th>
    </tr>
  </thead>
  <tbody>
    {% for row in site.data.credits %}
    <tr>
      <td>{{ row.player }}</td>
      <td>{{ row.balance }}</td>
      <td>{{ row.last_updated }}</td>
    </tr>
    {% endfor %}
  </tbody>
</table>
