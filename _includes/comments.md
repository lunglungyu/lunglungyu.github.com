<section class="comment">
<div id="disqus_thread"></div>
<script type="text/javascript">
    /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
    var disqus_config = function () {
        this.page.url = '{{ site.url }}{{ page.url | remove:'index.html' }}';  // Replace PAGE_URL with your page's canonical URL variable
        this.page.identifier = '{{ site.url }}{{ page.url | remove:'index.html' }}'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() {  // REQUIRED CONFIGURATION VARIABLE: EDIT THE SHORTNAME BELOW
        var d = document, s = d.createElement('script');
        
        s.src = '//lunglungyu.disqus.com/embed.js';  // IMPORTANT: Replace EXAMPLE with your forum shortname!
        
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
</section>
