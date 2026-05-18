---
layout: page
title: Reserve List
permalink: /ledgers/reserve-list/
---

Players currently on reserve, available for substitution and new-region placement. Source: `docs/_data/roster.yml`.

{% for player in site.data.roster.reserve %}
- **{{ player.username }}** — joined {{ player.joined }}{% if player.notes %}; {{ player.notes }}{% endif %}
{% endfor %}
