---
layout: page
permalink: /reading/
<<<<<<< HEAD
title: ""
---

<script>
$(document).ready(function(){

var rss = 'https://www.instapaper.com/starred/rss/3027971/BBAkGKpxHLpXeTJRFugszS4o9s';

=======
title: "Reading"
---

Recent articles I liked:

<script src="http://ajax.googleapis.com/ajax/libs/jquery/2.0.0/jquery.min.js"></script>
<script src="http://maps.googleapis.com/maps/api/js?key=AIzaSyCJnj2nWoM86eU8Bq2G4lSNz3udIkZT4YY&sensor=false"></script>

<p id="feed"></p>

<a href="https://www.instapaper.com/p/erikreinertsen">See more articles from my Instapaper...</a>

<script>
$(document).ready(function(){
var rss = 'https://www.instapaper.com/starred/rss/3027971/BBAkGKpxHLpXeTJRFugszS4o9s';
function hostname(url) {
  var matches = url.match(/^https?\:\/\/(www\.)?([^\/?#]+)(?:[\/?#]|$)/i);
  return matches[2];
}
>>>>>>> master
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
<<<<<<< HEAD
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
=======
})

(rss, function(feed){
    var entries = feed.entries, feed = '';
    for (var i = 0; i < entries.length; i++) {
        feed += '<p><b><a href="' + entries[i].link + '">"' + entries[i].title + '"</a></b> '
                + '<i>' + hostname(entries[i].link) + '</i></p>';
//                + entries[i].content + '</p>';
    }
    $('#feed').append(feed);
});
});
</script>
>>>>>>> master
