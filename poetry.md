---
title: Poetry
permalink: /poetry/
hidden: true          # keeps it out of search engines; it is also absent from the top bar
description: Some poems.
---

<!--
This page is linked only from the About page — it is not in `site.nav` in _config.yml.

One poem per <article class="poem-entry"> block. Inside it, the <div class="poem">
keeps your line breaks exactly as typed, so you can paste a poem in as-is without
markdown mangling it. Leading and trailing blank lines inside that div are trimmed
automatically — keep the text flush-left, since indentation IS preserved.

To add a poem, copy one whole <article> block and replace the three parts.

A random poem is chosen on every page load. That has to happen in the browser: a
static site is built once, so anything decided at build time would show the same
poem to everyone until the next deploy. The script is at the bottom of this file.
With JavaScript off, every poem shows — the page degrades to a plain list.
-->

<script>document.documentElement.classList.add('js');</script>

<!-- <p class="muted">A few things I've written. Reload for another.</p> -->

<article class="poem-entry">
<h2 class="poem-title">Walking to Campus</h2>
<p class="poem-meta">2026</p>
<div class="poem">
They bounce between branches
With fluttering wings and
Playful morning flight
Chicadees laughing chirps
Muted through the window

On my walk over
Amongst the purring of engines and
Squealing of power tools
The faintest of buzzing alerts me
To the diligent bee returning to his flowers
</div>
</article>

<article class="poem-entry">
<h2 class="poem-title">Prepare the Cabin for Landing</h2>
<p class="poem-meta">2026</p>
<div class="poem">
Bright lights in strict pattern
Fasten your seat belts
The plane is landing soon

The constant stream
A fixed arc through the sky
Of people shuttled into the city

Controlled and ordered
Unlike each disembarking
The runway a boundary between order and chaos

Once on the ground
Human humdrum returns
So enjoy those final minutes
</div>
</article>

<!-- <article class="poem-entry">
<h2 class="poem-title">Lost in Translation</h2>
<p class="poem-meta">2026</p>
<div class="poem">
It was lost in translation
Between the flips of the page.
Words tossed in the air
Among dancers cross stage.

I sat hunched over a dictionary
Texts, translations, pen in hand
The music always stops
Mournful looks from the band

Many frozen messages
Between present and past
On my paper a muted melody
diminished a voice at last

I may finally understand
But now it's far too late
Any long winded reply
Will forever have to wait
</div>
</article> -->

<p class="poem-nav" hidden><a href="#" id="another-poem">another poem</a></p>

<script>
(function () {
  var entries = Array.prototype.slice.call(document.querySelectorAll('.poem-entry'));
  if (!entries.length) return;

  // The source puts the poem on its own lines, which pre-wrap would render as a
  // blank line above and below. Trim those, keep everything in between.
  entries.forEach(function (entry) {
    var body = entry.querySelector('.poem');
    if (body) body.textContent = body.textContent.replace(/^\n+/, '').replace(/\s+$/, '');
  });

  var titleOf = function (entry) {
    var h = entry.querySelector('.poem-title');
    return h ? h.textContent : '';
  };

  function show(avoid) {
    // Don't repeat the previous poem, unless there's only one to choose from.
    var pool = entries.filter(function (e) { return titleOf(e) !== avoid; });
    if (!pool.length) pool = entries;

    var pick = pool[Math.floor(Math.random() * pool.length)];
    entries.forEach(function (e) { e.classList.toggle('is-shown', e === pick); });

    try { sessionStorage.setItem('lastPoem', titleOf(pick)); } catch (e) {}
  }

  var last = '';
  try { last = sessionStorage.getItem('lastPoem') || ''; } catch (e) {}
  show(last);

  // Optional: delete this block and the .poem-nav paragraph above to drop the link.
  var nav = document.querySelector('.poem-nav');
  if (nav && entries.length > 1) {
    nav.hidden = false;
    nav.querySelector('a').addEventListener('click', function (ev) {
      ev.preventDefault();
      var current = entries.filter(function (e) { return e.classList.contains('is-shown'); })[0];
      show(current ? titleOf(current) : '');
    });
  }
})();
</script>
