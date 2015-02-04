---
layout: page
permalink: /reading/
title: ""
---

<script>
$(document).ready(function(){

var rss = 'https://www.instapaper.com/starred/rss/3027971/BBAkGKpxHLpXeTJRFugszS4o9s';

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
        feed += '<p><b><a href="' + entries[i].link + '">"' + entries[i].title + '"</a></b><br>'
                + entries[i].content + '</p>';
    }
    $('#feed').append(feed);
});


}); /* ready */
</script>

<h1>I Recently Liked...</h1>

<p id="feed">
</p>

<a href="https://www.instapaper.com/p/erikreinertsen">More articles...</a>
