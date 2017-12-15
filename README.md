Nsk Google Developer Group webperf tools talk links, snippets and stuff

# Video
TBD

# Slides
[See slides folder](https://github.com/hom3chuk/webdev-meetup-5/tree/master/slides)

# Links
## Why?
- [Google article with some links to research](https://www.thinkwithgoogle.com/marketing-resources/experience-design/mobile-page-speed-load-time/)
- [This is Success: Why 1000/100/6/50ms?](https://docs.google.com/document/d/1bYMyE6NdiAupuwl7pWQfB-vOZBPSsXCv57hljLDMV8E/edit)
- [2018: 120fps and no jank](https://dassur.ma/things/120fps/)

## Tools
### Synthetic
- [Google PageSpeed](https://developers.google.com/speed/pagespeed/insights/) and here's a [shiny version for your boss](https://testmysite.withgoogle.com/intl/en-gb)
- [GTmetrix](https://gtmetrix.com/)
- [Pingdom](https://tools.pingdom.com/)
- [WebPageTest](https://www.webpagetest.org/) (WPT)
  - Built on top of WPT: [SpeedCurve](https://speedcurve.com/) and [MachMetrics](https://www.machmetrics.com/) (disc: me and MM do webperf case studies together)

### RUM (Real User Monitoring/Measurements)
- [Google Analytics](https://analytics.google.com/analytics/web/#report/content-site-speed-overview/)
- [New Relic](https://newrelic.com/)
- [AppOptics](https://www.appoptics.com/)
- [Taki](https://takiapp.com/) (disc: I'm on the team 🙋🏻)

## Other links
[User timing API](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API)
[Layout/Paint profiling](https://developers.google.com/web/fundamentals/performance/rendering/simplify-paint-complexity-and-reduce-paint-areas?hl=en)
[The whole web at maximum FPS: How WebRender gets rid of jank](https://hacks.mozilla.org/2017/10/the-whole-web-at-maximum-fps-how-webrender-gets-rid-of-jank/)

# Snippets
## Enable 100% sampling for RUM in GA
<pre>
// This is your generic GA snippet 
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
    (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
})(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

ga('create',
  'UA-xxxxxx', // Your Universal Analytics ID
<b>  {'siteSpeedSampleRate': 100} // <- this is the magic 🌝</b>
);
ga('send', 'pageview');
</pre>

## Insert non-critical CSS and JS on Request Animation Frame
```
<noscript id="deferred-resources">
  <!-- Here you place non-critical resources -->
  <link rel="stylesheet" href="/style.css">
  <script src="//cdn.example.com/javaskripte.js" integrity="sha384-HeyTC39"></script>
  <!-- Google Fonts can go here too -->
</noscript>

<script>
  // Here we load'em
  var loadDeferredStyles = function() {
  var addStylesNode = document.getElementById("deferred-resources");
  var replacement = document.createElement("div");
  replacement.innerHTML = addStylesNode.textContent;
  document.body.appendChild(replacement)
    addStylesNode.parentElement.removeChild(addStylesNode);
  };

  var raf = requestAnimationFrame || mozRequestAnimationFrame ||
    webkitRequestAnimationFrame || msRequestAnimationFrame;
  if (raf) raf(function() { window.setTimeout(loadDeferredStyles, 0); });
  else window.addEventListener("load", function() { window.setTimeout(loadDeferredStyles, 0)});
</script>
```

## Performance.marks to fun with in FF&Chrome
```
  <body>
    <script> requestAnimationFrame(function(){ performance.mark('raf:body'); }); </script>
  
    <script>performance.mark('resources:jquery:before');</script>
    <script defer src="https://code.jquery.com/jquery-3.2.1.slim.min.js"
            onload="performance.mark('resources:jquery:load')"></script>
    <script>performance.mark('resources:jquery:after');</script>
    <script> requestAnimationFrame(function(){ performance.mark('raf:after-jquery'); }); </script>
  
    <script>performance.mark('resources:main.css:before');</script>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css"
          onload="performance.mark('resources:main.css:load')">
    <script>performance.mark('resources:main.css:after');</script>
  
    <script> requestAnimationFrame(function(){ performance.mark('raf:after-css'); }); </script>
  
  </body>
```

# Reach out
- [Twitter](https://twitter.com/fast_wordpress)
- [Email](mailto:hom3chuk+perf@gmail.com)
- [Damn Fast WordPress: the Book](https://damnfastwordpress.com/the-book/)
- Telegram `hom3chuk`
