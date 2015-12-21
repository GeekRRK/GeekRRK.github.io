---
layout: post
title: Blog with Jekyll on Mac
---

###0. Installing Jekyll
`$ ruby --version` If the command not found, install ruby.  
`$ sudo gem install bundler`

###1. Setting up Jekyll
`$ jekyll new Blog`  
`$ cd Blog`  
`$ git init`  
`$ vim Gemfile` Add *gem 'github-pages'* to it.  
`$ bundle install`

###2. Running Jekyll
`$ bundle exec jekyll build`  
`$ bundle exec jekyll serve`  
Fire up a browser and go to *http://localhost:4000*  

###3. Pushing to GitHub
`$ vim _config.yml` Comment *markdown: kramdown* then add *markdown: redcarpet* to it and modify *baseurl: /Blog*.  
`$ git checkout --orphan gh-pages`  
`$ git remote add origin https://github.com/GeekRRK/Blog.git`  
`$ git add .`  
`$ git commit -m "Initial commit"`  
`$ git push origin gh-pages`  
Fire up a browser and go to *http://GeekRRK.github.io/Blog*

###4. Adding comments
Register *http://GeekRRK.github.io* on *http://disqus.com* and get the shortname *geekrrk*.  
`$ vim _config.yml` Add *disqusname: geekrrk* to it.  
`$ cd _includes`  
`$ vim comments.ext` Add the following content to it.  
{% highlight html %}
<div id="disqus_thread"></div>
<script type="text/javascript">
/* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
	var disqus_shortname = '{{site.disqusname}}'; // required: replace example with your forum shortname

/* * * DON'T EDIT BELOW THIS LINE * * */
	(function() {
		var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
		dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
		(document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
     })();
</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a>
</noscript>
<a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>
{% endhighlight %}
`$ cd ../_layouts`  
`$ vim post.html` Add *{% raw %}{% include comments.ext %}{% endraw %}* after *{% raw %}{{ content }}{% endraw %}*  
`$ cd ..`  
`$ git add .`  
`$ git commit -m "Add comments"`  
`$ push origin gh-pages`  
Fire up a browser and go to *http://GeekRRK.github.io/Blog*. If the comments don't appear, refresh the website after a few minutes.
