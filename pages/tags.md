---
description: Tags
---

<div class="tags-tags">
{{# tags }}
  <a id="{{ name }}" href="#{{ name }}-ref">{{name}}</a>
{{/ tags }}
</div>

<div class="tags-posts">
{{# tags }}
  <h3 id="{{ name }}-ref">{{ name }}</h3>
  {{# posts?to_posts }}
    <li><a href="{{url}}">{{title}}</a></li>
  {{/ posts?to_posts }}
{{/ tags }}
</div>

<script type="text/javascript">
function init(){
{{# tags }}
var sz = {{count}} + 15;
document.getElementById("{{ name }}").style.fontSize = sz.toString() + "px";
{{/ tags }}
}
</script>
