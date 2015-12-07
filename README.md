# Website Performance Optimization portfolio project

## Comments
<a href="https://www.udacity.com/course/viewer#!/c-nd001/l-2735848561/m-2686388535">Rubric</a>

### Optimizations Made
1. Optimizations to index.html
  1. I removed the link to the google font. This was, by far, the most time intensive rendering block.
    * Simply removing the link was a major improvement.
  1. I added a media query to the print.css link.
    * This is only needed when printing, so there is no need to block rending for this file.
  1. I inlined the css from style.css.
    * If I were using this css in multiple files I probably wouldn't do this, but the css seems small enough and gave me a big pagespeed insights boost.
  1. I removed a few seemingly unnecessary css properties and combined some values into the body property.
  1. I made the two javascript scripts async.
    * They had no bearing on the presentation of the page so can be loaded and run at leisure without blocking the rendering path.
  1. I optimized the images, drastically reducing the amount of bandwidth required to display the images.
    * pizzeria.jpg was initially 2.5mb and this was reduced to a few hundred kb.
1. Optimizations to views/js/main.js
  1. Reduced the number of pizzas randomly generated.
      * The initial number was 200. When looking at my 1920 X 1080px monitor I could only see 8 columns and 4 rows (32 pizzas) at any given time. I changed the procedure to calculate the appropriate number of rows based on the screen height divided by the number of pixels allocated for each row.
  1.  Inside the load event I created variables to save the list of moving pizzas and the random pizza container.
    * These two items are being used repeatedly and each time they were being used, the javascript was having to go query the DOM to get the elements.
    * These two items aren't changing during the life of the web page so there is no need to request the same data multiple times.
  1. Changed the document getter from querySelectorAll to getElementsbyClassName which seems to be much more efficient.
    * With the change above, this didn't make much of a difference, but why not have as many improvements as I can find?
  1. Inside the update positions function I made major modifications.
    1. Instead of requesting the list of items each time this function is called, the list of items is cached in the on document load function (as noted above).
    1. Instead of requesting the scrolltop value and dividing by 1250 each time running through the loop I made it it's own variable.
      * This prevents the javascript from requesting the value once for each pizza and dividing that number by 1250 for each pizza. (Originally it was looping 200 times per scrollbar move so 200 times to 1 time)
    1. There are only 5 different phases, so I changed the code to calculate the 5 phases once, and save those phases into an array, which should be easier to pull data out of.
      * The javascript was looping through the list of pizzas and calculating the phases for each item.
    1. Rather than "infinitly" adding one to the frame variable and checking to see if the value % 10 === 0, I set the frame value to 0.
    1. For every frame rendered the javascript was saving a time value into the times variable. The logAverageFrame function was using the time values to calculate the average render time of the last 10 frames. After calculating the average time I'm clearing out the list of times.
      * I'm not using times for anything else, so it's better to not let this grow too large.
    1.  Inside resizePizzas, the determineDx function was adjusted so that it was only called once.
      * It was initially being called once per pizza. Each pizza is using the same image, and is thus the same size. Therefore there's no need to call this once per pizza. Call it once and apply the results to all the pizzas.
1. Optimizations to views/css/style.css
  1. Added backface-visibility property to the .movers class.
    * This forces each randomly created pizza into it's own layer, which basically offloads it to the GPU.

### Outside Resources
* <a href="https://developers.google.com/speed/pagespeed/insights/">Pagespeed Insights</a>
* <a href="https://github.com/udacity/fend-office-hours/tree/master/Web%20Optimization/Effective%20Optimizations%20for%2060%20FPS">Udacity Office Hours suggestions</a>
* Udacity: Website Performance Optimization

##Project Description

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

To get started, check out the repository, inspect the code,

### Getting started

####Part 1: Optimize PageSpeed Insights score for index.html

Some useful tips to help you get started:

1. Check out the repository
1. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. Download and install [ngrok](https://ngrok.com/) to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ngrok 8080
  ```

1. Copy the public URL ngrok gives you and try running it through PageSpeed Insights! Optional: [More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

Profile, optimize, measure... and then lather, rinse, and repeat. Good luck!

####Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js.

You might find the FPS Counter/HUD Display useful in Chrome developer tools described here: [Chrome Dev Tools tips-and-tricks](https://developer.chrome.com/devtools/docs/tips-and-tricks).

### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>

### Sample Portfolios

Feeling uninspired by the portfolio? Here's a list of cool portfolios I found after a few minutes of Googling.

* <a href="http://www.reddit.com/r/webdev/comments/280qkr/would_anybody_like_to_post_their_portfolio_site/">A great discussion about portfolios on reddit</a>
* <a href="http://ianlunn.co.uk/">http://ianlunn.co.uk/</a>
* <a href="http://www.adhamdannaway.com/portfolio">http://www.adhamdannaway.com/portfolio</a>
* <a href="http://www.timboelaars.nl/">http://www.timboelaars.nl/</a>
* <a href="http://futoryan.prosite.com/">http://futoryan.prosite.com/</a>
* <a href="http://playonpixels.prosite.com/21591/projects">http://playonpixels.prosite.com/21591/projects</a>
* <a href="http://colintrenter.prosite.com/">http://colintrenter.prosite.com/</a>
* <a href="http://calebmorris.prosite.com/">http://calebmorris.prosite.com/</a>
* <a href="http://www.cullywright.com/">http://www.cullywright.com/</a>
* <a href="http://yourjustlucky.com/">http://yourjustlucky.com/</a>
* <a href="http://nicoledominguez.com/portfolio/">http://nicoledominguez.com/portfolio/</a>
* <a href="http://www.roxannecook.com/">http://www.roxannecook.com/</a>
* <a href="http://www.84colors.com/portfolio.html">http://www.84colors.com/portfolio.html</a>
