---
layout: page
title: Attendance Ledger
permalink: /ledgers/attendance/
---

Session attendance log. Source: `docs/_data/attendance.csv` — edit that file to update this page.

<table>
  <thead>
    <tr>
      <th>Session</th>
      <th>Date</th>
      <th>Game</th>
      <th>Attended</th>
    </tr>
  </thead>
  <tbody>
    {% for row in site.data.attendance %}
    <tr>
      <td>{{ row.session }}</td>
      <td>{{ row.date }}</td>
      <td>{{ row.game }}</td>
      <td>{{ row.attended }}</td>
    </tr>
    {% endfor %}
  </tbody>
</table>
