---
layout: page
permalink: /reading/
title: "Reading"
---

[Here are some of my favorite articles I read, via Instapaper](https://www.instapaper.com/p/erikreinertsen).

<script>
<!--
$(document).ready(function(){
var rss = 'https://www.instapaper.com/starred/rss/3027971/BBAkGKpxHLpXeTJRFugszS4o9s';
function hostname(url) {
  var matches = url.match(/^https?\:\/\/(www\.)?([^\/?#]+)(?:[\/?#]|$)/i);
  return matches[2];
}
(function(url, callback) {
    $.ajax({
        url: document.location.protocol
             + '//ajax.googleapis.com/ajax/services/feed/load?v=1.0&num=10&callback=?&q='
             + encodeURIComponent(url),
        dataType: 'json',
        success: function(data) {
            callback(data.responseData.feed);
        }
    });
})(rss, function(feed){
    var entries = feed.entries, feed = '';
    for (var i = 0; i < entries.length; i++) {
        feed += '<p><b><a href="' + entries[i].link + '">"' + entries[i].title + '"</a></b> '
                + '<i>' + hostname(entries[i].link) + '</i><br>'
                + entries[i].content + '</p>';
    }
    $('#feed').append(feed);
});
}); /* ready */
//-->
</script>
