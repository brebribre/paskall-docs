# Player Releases

Every Marien Player version, newest first. **Current** is what screens are given now. Install it as in [Set Up Your Device](setting-up-your-device.md#android-player).

<div id="releases" markdown>
<p><em>Loading the list…</em></p>
</div>

<noscript>
<p>Your browser has scripts turned off. <a href="https://api.marien.co.id/player/versions">See the list here instead.</a></p>
</noscript>

<script>
(function () {
  var API = 'https://api.marien.co.id';
  var box = document.getElementById('releases');
  function esc(s) { return String(s).replace(/[&<>"]/g, function (c) { return { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;' }[c]; }); }
  function date(iso) { try { return new Date(iso).toLocaleDateString(undefined, { day: 'numeric', month: 'short', year: 'numeric' }); } catch (e) { return iso; } }
  fetch(API + '/player/versions.json')
    .then(function (r) { if (!r.ok) throw new Error(r.status); return r.json(); })
    .then(function (list) {
      if (!list.length) { box.innerHTML = '<p>No player build has been published yet.</p>'; return; }
      var rows = list.map(function (r) {
        return '<tr><td><b>' + esc(r.version) + '</b>' + (r.is_current ? ' <span style="color:#7d7d7d">· current</span>' : '') + '</td>' +
          '<td>' + esc(date(r.published_at)) + '</td>' +
          '<td>' + (r.size_bytes / 1048576).toFixed(1) + ' MB</td>' +
          '<td><a class="md-button md-button--primary" href="' + API + esc(r.download_url) + '" download>Download</a></td></tr>';
      }).join('');
      box.innerHTML = '<table><thead><tr><th>Version</th><th>Published</th><th>Size</th><th></th></tr></thead><tbody>' + rows + '</tbody></table>';
    })
    .catch(function () {
      box.innerHTML = '<p>Could not load the list right now. <a href="' + API + '/player/versions">See it here instead.</a></p>';
    });
})();
</script>
