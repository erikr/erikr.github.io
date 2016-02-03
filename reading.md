---
layout: page
permalink: /reading/
title: "Reading"
---

<script src="http://ajax.googleapis.com/ajax/libs/jquery/2.0.0/jquery.min.js"></script>

Recent articles I liked:

<div id="feed">

<script>
$(document).ready(function(){
var rss = 'http://feeds.delicious.com/v2/rss/ereinerts';

function hostname(url) {
  var matches = url.match(/^https?\:\/\/(www\.)?([^\/?#]+)(?:[\/?#]|$)/i);
  return matches[2];
}

(function(url, callback) {
    $.ajax({
        url: document.location.protocol
             + '//ajax.googleapis.com/ajax/services/feed/load?v=1.0&num=-1&callback=?&q='
             + encodeURIComponent(url),
        dataType: 'json',
        success: function(data) {
            callback(data.responseData.feed);
        }
    });
})

(rss, function(feed){
    var entries = feed.entries, feed = '';
    for (var i = 0; i < entries.length; i++) {
        feed += '<p><b><a href="' + entries[i].link + '">' + entries[i].title + '</a></b><br>'
                + '<i>' + hostname(entries[i].link) + '</i></p>';
    }
    $('#feed').append(feed);
});

});
</script>
