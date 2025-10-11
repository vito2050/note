---
layout: default
title: 搜索
permalink: /search.html
---

# 搜索

<input id="q" type="search" placeholder="搜索标题、内容或标签..." style="width:100%; padding:.5rem; font-size:1rem;"/>
<div id="results" style="margin-top:1rem;"></div>

<script>
(function(){
  var input = document.getElementById('q');
  var results = document.getElementById('results');
  var indexUrl = '{{ "/search.json" | relative_url }}';
  var data = null;
  function escapeHtml(s){return s.replace(/[&<>"']/g, function(c){return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;','\'':'&#39;'}[c];});}
  function render(items, term){
    if(!items || items.length === 0){ results.innerHTML = '<p>无结果</p>'; return; }
    var html = '<ul style="padding-left:1rem;">' + items.map(function(it){
      var t = escapeHtml(it.title || '');
      var u = it.url;
      var snippet = (it.content || '').toLowerCase();
      var idx = term ? snippet.indexOf(term.toLowerCase()) : -1;
      var cut = idx > -1 ? Math.max(0, idx - 40) : 0;
      var excerpt = escapeHtml((it.content || '').substr(cut, 160));
      return '<li style="margin:.5rem 0;"><a href="'+u+'">'+t+'</a><div style="color:#666; font-size:.9rem;">'+excerpt+'...</div></li>';
    }).join('') + '</ul>';
    results.innerHTML = html;
  }
  function search(){
    var term = input.value.trim();
    if(!data){ return; }
    if(!term){ render(data.slice(0, 10)); return; }
    var items = data.filter(function(it){
      var hay = (it.title + ' ' + it.content + ' ' + (it.tags||[]).join(' ')).toLowerCase();
      return hay.indexOf(term.toLowerCase()) !== -1;
    }).slice(0, 25);
    render(items, term);
  }
  fetch(indexUrl).then(function(r){return r.json()}).then(function(j){ data = j; render(data.slice(0, 10)); });
  input.addEventListener('input', search);
})();
</script>
