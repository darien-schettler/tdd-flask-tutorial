<h1>Flaskr - Intro to Flask, Test-Driven Development, and JavaScript</h1>

<p>As many of you know, Flaskr - a mini-blog-like-app - is the app you build for the official Flask <a href="http://flask.pocoo.org/docs/1.0/tutorial/" rel="nofollow">tutorial</a>. I've gone through the tutorial more times than I care to admit. Anyway, I wanted to take the tutorial a step further by adding test-driven development (TDD), a bit of JavaScript, and deployment. This post is that tutorial. Enjoy.</p>
<p>Also, if you are completely new to Flask and/or web development in general, it's important to grasp these basic fundamental concepts:</p>
<ol>
<li>The difference between GET and POST requests and how functions within the app handle each.</li>
<li>What "requests" and "responses" are.</li>
<li>How HTML pages are rendered and/or returned to the end user.</li>
</ol>
<h3><a id="user-content-what-youre-building" class="anchor" aria-hidden="true" href="https://www.google.com/url?sa=i&source=images&cd=&ved=2ahUKEwiAnNOs3M_iAhUMh-AKHW20BgwQjRx6BAgBEAU&url=https%3A%2F%2Fgithub.com%2Fgyc567%2Fflaskr-jquery-tdd&psig=AOvVaw2LgMH-arMrJ5OGVTrzpa89&ust=1559734071374708><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>What you're building</h3>
<p><a target="_blank" rel="noopener noreferrer" href="/mjhea0/flaskr-tdd/blob/master/flaskr-app.png"><img src="/mjhea0/flaskr-tdd/raw/master/flaskr-app.png" alt="flaskr app" style="max-width:100%;"></a></p>
<h3><a id="user-content-change-log" class="anchor" aria-hidden="true" href="#change-log"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Change Log</h3>
<p>This tutorial was last updated on October 7th, 2018:</p>
<ul>
<li><strong>10/07/2018</strong>: Updated to Python 3.7.0</li>
<li><strong>05/10/2018</strong>: Updated to Python 3.6.5, Flask 1.0.2, Bootstrap 4.1.1</li>
<li><strong>10/16/2017</strong>: Updated to Python 3.6.2</li>
<li><strong>10/16/2017</strong>: Updated to Bootstrap 4</li>
<li><strong>10/10/2017</strong>: Added a search feature</li>
<li><strong>07/03/2017</strong>: Updated to Python 3.6.1</li>
<li><strong>01/24/2016</strong>: Updated to Python 3! (v3.5.1)</li>
<li><strong>08/24/2014</strong>: PEP8 updates.</li>
<li><strong>02/25/2014</strong>: Upgraded to SQLAlchemy.</li>
<li><strong>02/20/2014</strong>: Completed AJAX.</li>
<li><strong>12/06/2013</strong>: Added Bootstrap 3 styles</li>
<li><strong>11/29/2013</strong>: Updated unit tests.</li>
<li><strong>11/19/2013</strong>: Fixed typo. Updated unit tests.</li>
<li><strong>11/11/2013</strong>: Added information on requests.</li>
</ul>
<h3><a id="user-content-contents" class="anchor" aria-hidden="true" href="#contents"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Contents</h3>
<ol>
<li><a href="#test-driven-development">Test Driven Development?</a></li>
<li><a href="#download-python">Download Python</a></li>
<li><a href="#project-setup">Project Setup</a></li>
<li><a href="#first-test">First Test</a></li>
<li><a href="#flaskr-setup">Flaskr Setup</a></li>
<li><a href="#second-test">Second Test</a></li>
<li><a href="#database-setup">Database Setup</a></li>
<li><a href="#templates-and-views">Templates and Views</a></li>
<li><a href="#add-some-color">Add Some Color</a></li>
<li><a href="#test">Test</a></li>
<li><a href="#jquery">jQuery</a></li>
<li><a href="#deployment">Deployment</a></li>
<li><a href="#test-again">Test (again!)</a></li>
<li><a href="#bootstrap">Bootstrap</a></li>
<li><a href="#sqlalchemy">SQLAlchemy</a></li>
<li><a href="#search-page">Search Page</a></li>
<li><a href="#conclusion">Conclusion</a></li>
</ol>
<h3><a id="user-content-requirements" class="anchor" aria-hidden="true" href="#requirements"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Requirements</h3>
<p>This tutorial utilizes the following requirements:</p>
<ol>
<li>Python v3.7.0</li>
<li>Flask v1.0.2</li>
<li>Flask-SQLAlchemy v2.3.2</li>
<li>gunicorn v19.9.0</li>
</ol>
<h2><a id="user-content-test-driven-development" class="anchor" aria-hidden="true" href="#test-driven-development"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Test Driven Development?</h2>
<p><a target="_blank" rel="noopener noreferrer" href="https://raw.githubusercontent.com/mjhea0/flaskr-tdd/master/tdd.png"><img src="https://raw.githubusercontent.com/mjhea0/flaskr-tdd/master/tdd.png" alt="tdd" style="max-width:100%;"></a></p>
<p>Test-Driven Development (TDD) is an iterative development cycle that emphasizes writing automated tests before writing the actual feature or function. Put another way, TDD combines building and testing. This process not only helps ensure correctness of the code - but also helps to indirectly evolve the design and architecture of the project at hand.</p>
<p>TDD usually follows the "Red-Green-Refactor" cycle, as shown in the image above:</p>
<ol>
<li>Write a test</li>
<li>Run the test (it should fail)</li>
<li>Write just enough code for the test to pass</li>
<li>Refactor code and retest, again and again (if necessary)</li>
</ol>
<h2><a id="user-content-download-python" class="anchor" aria-hidden="true" href="#download-python"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Download Python</h2>
<p>Before beginning make sure you have the latest version of <a href="https://www.python.org/downloads/release/python-370/" rel="nofollow">Python 3.7</a> installed, which you can download from <a href="http://www.python.org/download/" rel="nofollow">http://www.python.org/download/</a>.</p>
<blockquote>
<p><strong>NOTE</strong>: This tutorial uses Python v3.7.0.</p>
</blockquote>
<p>Along with Python, this also installed-</p>
<ul>
<li><a href="https://pip.pypa.io/en/stable/" rel="nofollow">pip</a> - a <a href="http://en.wikipedia.org/wiki/Package_management_system" rel="nofollow">package management</a> system for Python, similar to gem or npm for Ruby and Node, respectively.</li>
<li><a href="https://docs.python.org/3/library/venv.html" rel="nofollow">venv</a> - used to create isolated environments for development. This is standard practice. Always, always, ALWAYS utilize virtual environments. If you don't, you will eventually run into problems with dependency conflicts.</li>
</ul>
<h2><a id="user-content-project-setup" class="anchor" aria-hidden="true" href="#project-setup"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Project Setup</h2>
<ol>
<li>
<p>Create a new directory to store the project:</p>
<div class="highlight highlight-source-shell"><pre>$ mkdir flaskr-tdd
$ <span class="pl-c1">cd</span> flaskr-tdd</pre></div>
</li>
<li>
<p>Create and activate your virtual env:</p>
<div class="highlight highlight-source-shell"><pre>$ python3 -m venv env
$ <span class="pl-c1">source</span> env/bin/activate</pre></div>
<blockquote>
<p><strong>NOTE</strong>: You know that you are in a virtual environment as "env" is now showing before the $ in your terminal - (env)$. To exit the virtual environment, use the command <code>deactivate</code>. You can reactivate by navigating back to the project directory and running <code>source env/bin/activate</code>.</p>
</blockquote>
</li>
<li>
<p>Install Flask with pip:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ pip install flask==1.0.2</pre></div>
</li>
</ol>
<h2><a id="user-content-first-test" class="anchor" aria-hidden="true" href="#first-test"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>First Test</h2>
<p>Let's start with a simple "hello, world" app.</p>
<ol>
<li>
<p>Create a test file:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ touch app-test.py</pre></div>
<p>Open this file in your favorite text editor. (I use <a href="http://www.sublimetext.com/" rel="nofollow">Sublime</a>.) Add the following code:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">from</span> app <span class="pl-k">import</span> app

<span class="pl-k">import</span> unittest


<span class="pl-k">class</span> <span class="pl-en">BasicTestCase</span>(<span class="pl-e">unittest</span>.<span class="pl-e">TestCase</span>):

    <span class="pl-k">def</span> <span class="pl-en">test_index</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        tester <span class="pl-k">=</span> app.test_client(<span class="pl-c1">self</span>)
        response <span class="pl-k">=</span> tester.get(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>, <span class="pl-v">content_type</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>html/text<span class="pl-pds">'</span></span>)
        <span class="pl-c1">self</span>.assertEqual(response.status_code, <span class="pl-c1">200</span>)
        <span class="pl-c1">self</span>.assertEqual(response.data, <span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>Hello, World!<span class="pl-pds">'</span></span>)


<span class="pl-k">if</span> <span class="pl-c1">__name__</span> <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>__main__<span class="pl-pds">'</span></span>:
    unittest.main()</pre></div>
</li>
</ol>
<p>Essentially, we're testing whether the response that we get back is "200" and that "Hello, World!" is displayed.</p>
<ol>
<li>
<p>Run the test:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ python app-test.py</pre></div>
<p>If all goes well, this will fail:</p>
<div class="highlight highlight-source-shell"><pre>ModuleNotFoundError: No module named <span class="pl-s"><span class="pl-pds">'</span>app<span class="pl-pds">'</span></span></pre></div>
</li>
<li>
<p>Now add the code for this to pass.</p>
<div class="highlight highlight-source-shell"><pre>(env)$ touch app.py</pre></div>
<p>Code:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">from</span> flask <span class="pl-k">import</span> Flask

app <span class="pl-k">=</span> Flask(<span class="pl-c1">__name__</span>)


<span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>)
<span class="pl-k">def</span> <span class="pl-en">hello</span>():
    <span class="pl-k">return</span> <span class="pl-s"><span class="pl-pds">'</span>Hello, World!<span class="pl-pds">'</span></span>


<span class="pl-k">if</span> <span class="pl-c1">__name__</span> <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>__main__<span class="pl-pds">'</span></span>:
    app.run()</pre></div>
</li>
<li>
<p>Run the app:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ python app.py</pre></div>
<p>Then Navigate to <a href="http://localhost:5000/" rel="nofollow">http://localhost:5000/</a> in your browser of choice. You should see "Hello, World!" on your screen.</p>
<p>Return to the terminal. Kill the server with Ctrl+C.</p>
</li>
<li>
<p>Run the test again:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ python app-test.py
<span class="pl-c1">.</span>
----------------------------------------------------------------------
Ran 1 <span class="pl-c1">test</span> <span class="pl-k">in</span> 0.010s

OK</pre></div>
<p>Nice.</p>
</li>
</ol>
<h2><a id="user-content-flaskr-setup" class="anchor" aria-hidden="true" href="#flaskr-setup"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Flaskr Setup</h2>
<ol>
<li>
<p>Add structure</p>
<p>Add two folders, "static" and "templates", in the project root. Your file structure should now look like this:</p>
<div class="highlight highlight-source-shell"><pre>+-- app-test.py
+-- app.py
+-- static
+-- templates</pre></div>
</li>
<li>
<p>SQL Schema</p>
<p>Create a new file called <em>schema.sql</em> and add the following code:</p>
<div class="highlight highlight-source-sql"><pre><span class="pl-k">drop</span> <span class="pl-k">table</span> if exists entries;
<span class="pl-k">create</span> <span class="pl-k">table</span> <span class="pl-en">entries</span> (
  id <span class="pl-k">integer</span> <span class="pl-k">primary key</span> autoincrement,
  title <span class="pl-k">text</span> <span class="pl-k">not null</span>,
  <span class="pl-k">text</span> <span class="pl-k">text</span> <span class="pl-k">not null</span>
);</pre></div>
</li>
</ol>
<p>This will set up a single table with three fields - "id", "title", and "text". SQLite will be used for our RDMS since it's built in to the standard Python library and requires no configuration.</p>
<h2><a id="user-content-second-test" class="anchor" aria-hidden="true" href="#second-test"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Second Test</h2>
<p>Let's create the basic file for running our application. Before that though, we need to write a test.</p>
<ol>
<li>
<p>Simply alter <em>app-test.py</em> like so:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">from</span> app <span class="pl-k">import</span> app

<span class="pl-k">import</span> unittest


<span class="pl-k">class</span> <span class="pl-en">BasicTestCase</span>(<span class="pl-e">unittest</span>.<span class="pl-e">TestCase</span>):

    <span class="pl-k">def</span> <span class="pl-en">test_index</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        tester <span class="pl-k">=</span> app.test_client(<span class="pl-c1">self</span>)
        response <span class="pl-k">=</span> tester.get(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>, <span class="pl-v">content_type</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>html/text<span class="pl-pds">'</span></span>)
        <span class="pl-c1">self</span>.assertEqual(response.status_code, <span class="pl-c1">404</span>)


<span class="pl-k">if</span> <span class="pl-c1">__name__</span> <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>__main__<span class="pl-pds">'</span></span>:
    unittest.main()</pre></div>
<p>So, we are expecting a 404 error. Run the test. This will fail. Why? We are expecting a 404, but we actually get a 200 back since the route exists.</p>
</li>
<li>
<p>Update <em>app.py</em>:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-c"><span class="pl-c">#</span> imports</span>
<span class="pl-k">import</span> sqlite3
<span class="pl-k">from</span> flask <span class="pl-k">import</span> Flask, request, session, g, redirect, url_for, \
     abort, render_template, flash, jsonify


<span class="pl-c"><span class="pl-c">#</span> configuration</span>
<span class="pl-c1">DATABASE</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>flaskr.db<span class="pl-pds">'</span></span>
<span class="pl-c1">DEBUG</span> <span class="pl-k">=</span> <span class="pl-c1">True</span>
<span class="pl-c1">SECRET_KEY</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>my_precious<span class="pl-pds">'</span></span>
<span class="pl-c1">USERNAME</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>admin<span class="pl-pds">'</span></span>
<span class="pl-c1">PASSWORD</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>admin<span class="pl-pds">'</span></span>


<span class="pl-c"><span class="pl-c">#</span> create and initialize app</span>
app <span class="pl-k">=</span> Flask(<span class="pl-c1">__name__</span>)
app.config.from_object(<span class="pl-c1">__name__</span>)


<span class="pl-k">if</span> <span class="pl-c1">__name__</span> <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>__main__<span class="pl-pds">'</span></span>:
    app.run()</pre></div>
<p>Here, we add in all the required imports, create a configuration section for global variables, initialize the app, and then finally run the app.</p>
</li>
<li>
<p>Run it:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ python app.py</pre></div>
<p>Launch the server. You should see the 404 error because no routes or views are setup. Return to the terminal. Kill the server. Now run the test. It should pass.</p>
</li>
</ol>
<h2><a id="user-content-database-setup" class="anchor" aria-hidden="true" href="#database-setup"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Database Setup</h2>
<p>Essentially, we want to open a database connection, create the database based on the schema if it doesn't already exist, then close the connection each time a test is ran.</p>
<ol>
<li>
<p>How do we test for the existence of a file? Update <em>app-test.py</em>:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">import</span> unittest
<span class="pl-k">import</span> os
<span class="pl-k">from</span> app <span class="pl-k">import</span> app


<span class="pl-k">class</span> <span class="pl-en">BasicTestCase</span>(<span class="pl-e">unittest</span>.<span class="pl-e">TestCase</span>):

    <span class="pl-k">def</span> <span class="pl-en">test_index</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        tester <span class="pl-k">=</span> app.test_client(<span class="pl-c1">self</span>)
        response <span class="pl-k">=</span> tester.get(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>, <span class="pl-v">content_type</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>html/text<span class="pl-pds">'</span></span>)
        <span class="pl-c1">self</span>.assertEqual(response.status_code, <span class="pl-c1">404</span>)

    <span class="pl-k">def</span> <span class="pl-en">test_database</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        tester <span class="pl-k">=</span> os.path.exists(<span class="pl-s"><span class="pl-pds">"</span>flaskr.db<span class="pl-pds">"</span></span>)
        <span class="pl-c1">self</span>.assertTrue(tester)


<span class="pl-k">if</span> <span class="pl-c1">__name__</span> <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>__main__<span class="pl-pds">'</span></span>:
    unittest.main()</pre></div>
<p>Run it to make sure it fails, indicating that the database does not exist.</p>
</li>
<li>
<p>Now add the following code to <em>app.py</em>:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-c"><span class="pl-c">#</span> connect to database</span>
<span class="pl-k">def</span> <span class="pl-en">connect_db</span>():
    <span class="pl-s"><span class="pl-pds">"""</span>Connects to the database.<span class="pl-pds">"""</span></span>
    rv <span class="pl-k">=</span> sqlite3.connect(app.config[<span class="pl-s"><span class="pl-pds">'</span>DATABASE<span class="pl-pds">'</span></span>])
    rv.row_factory <span class="pl-k">=</span> sqlite3.Row
    <span class="pl-k">return</span> rv


<span class="pl-c"><span class="pl-c">#</span> create the database</span>
<span class="pl-k">def</span> <span class="pl-en">init_db</span>():
    <span class="pl-k">with</span> app.app_context():
        db <span class="pl-k">=</span> get_db()
        <span class="pl-k">with</span> app.open_resource(<span class="pl-s"><span class="pl-pds">'</span>schema.sql<span class="pl-pds">'</span></span>, <span class="pl-v">mode</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>r<span class="pl-pds">'</span></span>) <span class="pl-k">as</span> f:
            db.cursor().executescript(f.read())
        db.commit()


<span class="pl-c"><span class="pl-c">#</span> open database connection</span>
<span class="pl-k">def</span> <span class="pl-en">get_db</span>():
    <span class="pl-k">if</span> <span class="pl-k">not</span> <span class="pl-c1">hasattr</span>(g, <span class="pl-s"><span class="pl-pds">'</span>sqlite_db<span class="pl-pds">'</span></span>):
        g.sqlite_db <span class="pl-k">=</span> connect_db()
    <span class="pl-k">return</span> g.sqlite_db


<span class="pl-c"><span class="pl-c">#</span> close database connection</span>
<span class="pl-en">@app.teardown_appcontext</span>
<span class="pl-k">def</span> <span class="pl-en">close_db</span>(<span class="pl-smi">error</span>):
    <span class="pl-k">if</span> <span class="pl-c1">hasattr</span>(g, <span class="pl-s"><span class="pl-pds">'</span>sqlite_db<span class="pl-pds">'</span></span>):
        g.sqlite_db.close()</pre></div>
<p>And add the <code>init_db()</code> function at the bottom of <code>app.py</code> to make sure we start the server each time with a fresh database:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">if</span> <span class="pl-c1">__name__</span> <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>__main__<span class="pl-pds">'</span></span>:
    init_db()
    app.run()</pre></div>
<p>Now it is possible to create a database by starting up a Python shell and importing and calling the <code>init_db()</code> function:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">&gt;&gt;</span><span class="pl-k">&gt;</span> <span class="pl-k">from</span> app <span class="pl-k">import</span> init_db
<span class="pl-k">&gt;&gt;</span><span class="pl-k">&gt;</span> init_db()</pre></div>
<p>Close the shell, then run the test again. Does it pass? Now we know that the database has been created.</p>
</li>
</ol>
<h2><a id="user-content-templates-and-views" class="anchor" aria-hidden="true" href="#templates-and-views"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Templates and Views</h2>
<p>Next, we need to set up the templates and the associated views, which define the routes. Think about this from a user standpoint:</p>
<ol>
<li>Users should be able to log in and out.</li>
<li>Once logged in, users should be able to post.</li>
<li>Finally, users should be able to view the posts.</li>
</ol>
<p>Write some tests for this first.</p>
<h3><a id="user-content-tests" class="anchor" aria-hidden="true" href="#tests"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Tests</h3>
<p>Take a look at the final code. I added docstrings for explanation.</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">import</span> unittest
<span class="pl-k">import</span> os
<span class="pl-k">import</span> tempfile

<span class="pl-k">import</span> app


<span class="pl-k">class</span> <span class="pl-en">BasicTestCase</span>(<span class="pl-e">unittest</span>.<span class="pl-e">TestCase</span>):

    <span class="pl-k">def</span> <span class="pl-en">test_index</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Initial test: Ensure flask was set up correctly.<span class="pl-pds">"""</span></span>
        tester <span class="pl-k">=</span> app.app.test_client(<span class="pl-c1">self</span>)
        response <span class="pl-k">=</span> tester.get(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>, <span class="pl-v">content_type</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>html/text<span class="pl-pds">'</span></span>)
        <span class="pl-c1">self</span>.assertEqual(response.status_code, <span class="pl-c1">200</span>)

    <span class="pl-k">def</span> <span class="pl-en">test_database</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Initial test: Ensure that the database exists.<span class="pl-pds">"""</span></span>
        tester <span class="pl-k">=</span> os.path.exists(<span class="pl-s"><span class="pl-pds">"</span>flaskr.db<span class="pl-pds">"</span></span>)
        <span class="pl-c1">self</span>.assertEqual(tester, <span class="pl-c1">True</span>)


<span class="pl-k">class</span> <span class="pl-en">FlaskrTestCase</span>(<span class="pl-e">unittest</span>.<span class="pl-e">TestCase</span>):

    <span class="pl-k">def</span> <span class="pl-en">setUp</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Set up a blank temp database before each test.<span class="pl-pds">"""</span></span>
        <span class="pl-c1">self</span>.db_fd, app.app.config[<span class="pl-s"><span class="pl-pds">'</span>DATABASE<span class="pl-pds">'</span></span>] <span class="pl-k">=</span> tempfile.mkstemp()
        app.app.config[<span class="pl-s"><span class="pl-pds">'</span>TESTING<span class="pl-pds">'</span></span>] <span class="pl-k">=</span> <span class="pl-c1">True</span>
        <span class="pl-c1">self</span>.app <span class="pl-k">=</span> app.app.test_client()
        app.init_db()

    <span class="pl-k">def</span> <span class="pl-en">tearDown</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Destroy blank temp database after each test.<span class="pl-pds">"""</span></span>
        os.close(<span class="pl-c1">self</span>.db_fd)
        os.unlink(app.app.config[<span class="pl-s"><span class="pl-pds">'</span>DATABASE<span class="pl-pds">'</span></span>])

    <span class="pl-k">def</span> <span class="pl-en">login</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>, <span class="pl-smi">username</span>, <span class="pl-smi">password</span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Login helper function.<span class="pl-pds">"""</span></span>
        <span class="pl-k">return</span> <span class="pl-c1">self</span>.app.post(<span class="pl-s"><span class="pl-pds">'</span>/login<span class="pl-pds">'</span></span>, <span class="pl-v">data</span><span class="pl-k">=</span><span class="pl-c1">dict</span>(
            <span class="pl-v">username</span><span class="pl-k">=</span>username,
            <span class="pl-v">password</span><span class="pl-k">=</span>password
        ), <span class="pl-v">follow_redirects</span><span class="pl-k">=</span><span class="pl-c1">True</span>)

    <span class="pl-k">def</span> <span class="pl-en">logout</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Logout helper function.<span class="pl-pds">"""</span></span>
        <span class="pl-k">return</span> <span class="pl-c1">self</span>.app.get(<span class="pl-s"><span class="pl-pds">'</span>/logout<span class="pl-pds">'</span></span>, <span class="pl-v">follow_redirects</span><span class="pl-k">=</span><span class="pl-c1">True</span>)

    <span class="pl-c"><span class="pl-c">#</span> assert functions</span>

    <span class="pl-k">def</span> <span class="pl-en">test_empty_db</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Ensure database is blank.<span class="pl-pds">"""</span></span>
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.app.get(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>)
        <span class="pl-k">assert</span> <span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>No entries here so far<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data

    <span class="pl-k">def</span> <span class="pl-en">test_login_logout</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Test login and logout using helper functions.<span class="pl-pds">"""</span></span>
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.login(
            app.app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>],
            app.app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>]
        )
        <span class="pl-k">assert</span> <span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>You were logged in<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.logout()
        <span class="pl-k">assert</span> <span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>You were logged out<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.login(
            app.app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>] <span class="pl-k">+</span> <span class="pl-s"><span class="pl-pds">'</span>x<span class="pl-pds">'</span></span>,
            app.app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>]
        )
        <span class="pl-k">assert</span> <span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>Invalid username<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.login(
            app.app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>],
            app.app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>] <span class="pl-k">+</span> <span class="pl-s"><span class="pl-pds">'</span>x<span class="pl-pds">'</span></span>
        )
        <span class="pl-k">assert</span> <span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>Invalid password<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data

    <span class="pl-k">def</span> <span class="pl-en">test_messages</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Ensure that a user can post messages.<span class="pl-pds">"""</span></span>
        <span class="pl-c1">self</span>.login(
            app.app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>],
            app.app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>]
        )
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.app.post(<span class="pl-s"><span class="pl-pds">'</span>/add<span class="pl-pds">'</span></span>, <span class="pl-v">data</span><span class="pl-k">=</span><span class="pl-c1">dict</span>(
            <span class="pl-v">title</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>&lt;Hello&gt;<span class="pl-pds">'</span></span>,
            <span class="pl-v">text</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>&lt;strong&gt;HTML&lt;/strong&gt; allowed here<span class="pl-pds">'</span></span>
        ), <span class="pl-v">follow_redirects</span><span class="pl-k">=</span><span class="pl-c1">True</span>)
        <span class="pl-k">assert</span> <span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>No entries here so far<span class="pl-pds">'</span></span> <span class="pl-k">not</span> <span class="pl-k">in</span> rv.data
        <span class="pl-k">assert</span> <span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>&amp;lt;Hello&amp;gt;<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data
        <span class="pl-k">assert</span> <span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>&lt;strong&gt;HTML&lt;/strong&gt; allowed here<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data


<span class="pl-k">if</span> <span class="pl-c1">__name__</span> <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>__main__<span class="pl-pds">'</span></span>:
    unittest.main()</pre></div>
<p>If you run the tests now:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ python app-test.py</pre></div>
<p>All will fail except for <code>test_database()</code>:</p>
<div class="highlight highlight-source-shell"><pre>.FFFF
======================================================================
FAIL: test_index (__main__.BasicTestCase)
Initial test: Ensure flask was <span class="pl-c1">set</span> up correctly.
----------------------------------------------------------------------
Traceback (most recent call last):
  File <span class="pl-s"><span class="pl-pds">"</span>app-test.py<span class="pl-pds">"</span></span>, line 14, <span class="pl-k">in</span> test_index
    self.assertEqual(response.status_code, 200)
AssertionError: 404 <span class="pl-k">!</span>= 200

======================================================================
FAIL: test_empty_db (__main__.FlaskrTestCase)
Ensure database is blank.
----------------------------------------------------------------------
Traceback (most recent call last):
  File <span class="pl-s"><span class="pl-pds">"</span>app-test.py<span class="pl-pds">"</span></span>, line 52, <span class="pl-k">in</span> test_empty_db
    assert b<span class="pl-s"><span class="pl-pds">'</span>No entries here so far<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data
AssertionError

======================================================================
FAIL: test_login_logout (__main__.FlaskrTestCase)
Test login and <span class="pl-c1">logout</span> using helper functions.
----------------------------------------------------------------------
Traceback (most recent call last):
  File <span class="pl-s"><span class="pl-pds">"</span>app-test.py<span class="pl-pds">"</span></span>, line 60, <span class="pl-k">in</span> test_login_logout
    assert b<span class="pl-s"><span class="pl-pds">'</span>You were logged in<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data
AssertionError

======================================================================
FAIL: test_messages (__main__.FlaskrTestCase)
Ensure that a user can post messages.
----------------------------------------------------------------------
Traceback (most recent call last):
  File <span class="pl-s"><span class="pl-pds">"</span>app-test.py<span class="pl-pds">"</span></span>, line 85, <span class="pl-k">in</span> test_messages
    assert b<span class="pl-s"><span class="pl-pds">'</span>&amp;lt;Hello&amp;gt;<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data
AssertionError

----------------------------------------------------------------------
Ran 5 tests <span class="pl-k">in</span> 0.020s

FAILED (failures=4)</pre></div>
<p>Let's get these all green, one at a time...</p>
<h3><a id="user-content-show-entries" class="anchor" aria-hidden="true" href="#show-entries"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Show Entries</h3>
<ol>
<li>
<p>First, add a view for displaying the entries to <em>app.py</em>:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>)
<span class="pl-k">def</span> <span class="pl-en">show_entries</span>():
    <span class="pl-s"><span class="pl-pds">"""</span>Searches the database for entries, then displays them.<span class="pl-pds">"""</span></span>
    db <span class="pl-k">=</span> get_db()
    cur <span class="pl-k">=</span> db.execute(<span class="pl-s"><span class="pl-pds">'</span>select * from entries order by id desc<span class="pl-pds">'</span></span>)
    entries <span class="pl-k">=</span> cur.fetchall()
    <span class="pl-k">return</span> render_template(<span class="pl-s"><span class="pl-pds">'</span>index.html<span class="pl-pds">'</span></span>, <span class="pl-v">entries</span><span class="pl-k">=</span>entries)</pre></div>
</li>
<li>
<p>Then add the <em>index.html</em> template to the "templates" folder:</p>
<div class="highlight highlight-text-html-basic"><pre>&lt;!DOCTYPE html&gt;
&lt;<span class="pl-ent">html</span>&gt;
&lt;<span class="pl-ent">head</span>&gt;
  &lt;<span class="pl-ent">title</span>&gt;Flaskr&lt;/<span class="pl-ent">title</span>&gt;
  &lt;<span class="pl-ent">link</span> <span class="pl-e">rel</span>=<span class="pl-s"><span class="pl-pds">"</span>stylesheet<span class="pl-pds">"</span></span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/css<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('static', filename='style.css') }}<span class="pl-pds">"</span></span>&gt;
&lt;/<span class="pl-ent">head</span>&gt;
&lt;<span class="pl-ent">body</span>&gt;

  &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>page<span class="pl-pds">"</span></span>&gt;

    &lt;<span class="pl-ent">h1</span>&gt;Flaskr-TDD&lt;/<span class="pl-ent">h1</span>&gt;
    &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>metanav<span class="pl-pds">"</span></span>&gt;
      {% if not session.logged_in %}
        &lt;<span class="pl-ent">a</span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('login') }}<span class="pl-pds">"</span></span>&gt;log in&lt;/<span class="pl-ent">a</span>&gt;
      {% else %}
        &lt;<span class="pl-ent">a</span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('logout') }}<span class="pl-pds">"</span></span>&gt;log out&lt;/<span class="pl-ent">a</span>&gt;
      {% endif %}
    &lt;/<span class="pl-ent">div</span>&gt;
    {% for message in get_flashed_messages() %}
      &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>flash<span class="pl-pds">"</span></span>&gt;{{ message }}&lt;/<span class="pl-ent">div</span>&gt;
    {% endfor %}
    {% block body %}{% endblock %}

    {% if session.logged_in %}
      &lt;<span class="pl-ent">form</span> <span class="pl-e">action</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('add_entry') }}<span class="pl-pds">"</span></span> <span class="pl-e">method</span>=<span class="pl-s"><span class="pl-pds">"</span>post<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>add-entry<span class="pl-pds">"</span></span>&gt;
        &lt;<span class="pl-ent">dl</span>&gt;
          &lt;<span class="pl-ent">dt</span>&gt;Title:&lt;/<span class="pl-ent">dt</span>&gt;
          &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text<span class="pl-pds">"</span></span> <span class="pl-e">size</span>=<span class="pl-s"><span class="pl-pds">"</span>30<span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>title<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
          &lt;<span class="pl-ent">dt</span>&gt;Text:&lt;/<span class="pl-ent">dt</span>&gt;
          &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">textarea</span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>text<span class="pl-pds">"</span></span> <span class="pl-e">rows</span>=<span class="pl-s"><span class="pl-pds">"</span>5<span class="pl-pds">"</span></span> <span class="pl-e">cols</span>=<span class="pl-s"><span class="pl-pds">"</span>40<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">textarea</span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
          &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>submit<span class="pl-pds">"</span></span> <span class="pl-e">value</span>=<span class="pl-s"><span class="pl-pds">"</span>Share<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
        &lt;/<span class="pl-ent">dl</span>&gt;
      &lt;/<span class="pl-ent">form</span>&gt;
    {% endif %}
    &lt;<span class="pl-ent">ul</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>entries<span class="pl-pds">"</span></span>&gt;
      {% for entry in entries %}
        &lt;<span class="pl-ent">li</span>&gt;&lt;<span class="pl-ent">h2</span>&gt;{{ entry.title }}&lt;/<span class="pl-ent">h2</span>&gt;{{ entry.text|safe }}&lt;/<span class="pl-ent">li</span>&gt;
      {% else %}
        &lt;<span class="pl-ent">li</span>&gt;&lt;<span class="pl-ent">em</span>&gt;No entries yet. Add some!&lt;/<span class="pl-ent">em</span>&gt;&lt;/<span class="pl-ent">li</span>&gt;
      {% endfor %}
    &lt;/<span class="pl-ent">ul</span>&gt;

  &lt;/<span class="pl-ent">div</span>&gt;

&lt;/<span class="pl-ent">body</span>&gt;
&lt;/<span class="pl-ent">html</span>&gt;</pre></div>
</li>
<li>
<p>Run the tests now. You should see:</p>
<div class="highlight highlight-source-shell"><pre>Ran 5 tests <span class="pl-k">in</span> 0.048s

FAILED (failures=2, errors=2)</pre></div>
</li>
</ol>
<h3><a id="user-content-user-login-and-logout" class="anchor" aria-hidden="true" href="#user-login-and-logout"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>User Login and Logout</h3>
<ol>
<li>
<p>Update <em>app.py</em>:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/login<span class="pl-pds">'</span></span>, <span class="pl-v">methods</span><span class="pl-k">=</span>[<span class="pl-s"><span class="pl-pds">'</span>GET<span class="pl-pds">'</span></span>, <span class="pl-s"><span class="pl-pds">'</span>POST<span class="pl-pds">'</span></span>])
<span class="pl-k">def</span> <span class="pl-en">login</span>():
    <span class="pl-s"><span class="pl-pds">"""</span>User login/authentication/session management.<span class="pl-pds">"""</span></span>
    error <span class="pl-k">=</span> <span class="pl-c1">None</span>
    <span class="pl-k">if</span> request.method <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>POST<span class="pl-pds">'</span></span>:
        <span class="pl-k">if</span> request.form[<span class="pl-s"><span class="pl-pds">'</span>username<span class="pl-pds">'</span></span>] <span class="pl-k">!=</span> app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>]:
            error <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>Invalid username<span class="pl-pds">'</span></span>
        <span class="pl-k">elif</span> request.form[<span class="pl-s"><span class="pl-pds">'</span>password<span class="pl-pds">'</span></span>] <span class="pl-k">!=</span> app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>]:
            error <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>Invalid password<span class="pl-pds">'</span></span>
        <span class="pl-k">else</span>:
            session[<span class="pl-s"><span class="pl-pds">'</span>logged_in<span class="pl-pds">'</span></span>] <span class="pl-k">=</span> <span class="pl-c1">True</span>
            flash(<span class="pl-s"><span class="pl-pds">'</span>You were logged in<span class="pl-pds">'</span></span>)
            <span class="pl-k">return</span> redirect(url_for(<span class="pl-s"><span class="pl-pds">'</span>index<span class="pl-pds">'</span></span>))
    <span class="pl-k">return</span> render_template(<span class="pl-s"><span class="pl-pds">'</span>login.html<span class="pl-pds">'</span></span>, <span class="pl-v">error</span><span class="pl-k">=</span>error)


<span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/logout<span class="pl-pds">'</span></span>)
<span class="pl-k">def</span> <span class="pl-en">logout</span>():
    <span class="pl-s"><span class="pl-pds">"""</span>User logout/authentication/session management.<span class="pl-pds">"""</span></span>
    session.pop(<span class="pl-s"><span class="pl-pds">'</span>logged_in<span class="pl-pds">'</span></span>, <span class="pl-c1">None</span>)
    flash(<span class="pl-s"><span class="pl-pds">'</span>You were logged out<span class="pl-pds">'</span></span>)
    <span class="pl-k">return</span> redirect(url_for(<span class="pl-s"><span class="pl-pds">'</span>index<span class="pl-pds">'</span></span>))</pre></div>
<p>In the above <code>login()</code> function, the decorator indicates that the route can accept either a GET or POST request. Put simply, a request is initiated by the end user when they access the <code>/login</code> url. The difference between these requests is simple - GET is used for accessing a webpage, while POST is used when information is sent to the server. Thus, when a user accesses the <code>/login</code> url, they are using a GET request, but when they attempt to log in, a POST request is used.</p>
</li>
<li>
<p>Add the template - <em>login.html</em>:</p>
<div class="highlight highlight-text-html-basic"><pre>&lt;!DOCTYPE html&gt;
&lt;<span class="pl-ent">html</span>&gt;
&lt;<span class="pl-ent">head</span>&gt;
  &lt;<span class="pl-ent">title</span>&gt;Flaskr-TDD | Login&lt;/<span class="pl-ent">title</span>&gt;
  &lt;<span class="pl-ent">link</span> <span class="pl-e">rel</span>=<span class="pl-s"><span class="pl-pds">"</span>stylesheet<span class="pl-pds">"</span></span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/css<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('static', filename='style.css') }}<span class="pl-pds">"</span></span>&gt;
&lt;/<span class="pl-ent">head</span>&gt;
&lt;<span class="pl-ent">body</span>&gt;

  &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>page<span class="pl-pds">"</span></span>&gt;

    &lt;<span class="pl-ent">h1</span>&gt;Flaskr&lt;/<span class="pl-ent">h1</span>&gt;
    &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>metanav<span class="pl-pds">"</span></span>&gt;
      {% if not session.logged_in %}
        &lt;<span class="pl-ent">a</span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('login') }}<span class="pl-pds">"</span></span>&gt;log in&lt;/<span class="pl-ent">a</span>&gt;
      {% else %}
        &lt;<span class="pl-ent">a</span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('logout') }}<span class="pl-pds">"</span></span>&gt;log out&lt;/<span class="pl-ent">a</span>&gt;
      {% endif %}
    &lt;/<span class="pl-ent">div</span>&gt;
    {% for message in get_flashed_messages() %}
      &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>flash<span class="pl-pds">"</span></span>&gt;{{ message }}&lt;/<span class="pl-ent">div</span>&gt;
    {% endfor %}
    {% block body %}{% endblock %}

    &lt;<span class="pl-ent">h2</span>&gt;Login&lt;/<span class="pl-ent">h2</span>&gt;
    {% if error %}
      &lt;<span class="pl-ent">p</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>error<span class="pl-pds">"</span></span>&gt;&lt;<span class="pl-ent">strong</span>&gt;Error:&lt;/<span class="pl-ent">strong</span>&gt; {{ error }}&lt;/<span class="pl-ent">p</span>&gt;
    {% endif %}
    &lt;<span class="pl-ent">form</span> <span class="pl-e">action</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('login') }}<span class="pl-pds">"</span></span> <span class="pl-e">method</span>=<span class="pl-s"><span class="pl-pds">"</span>post<span class="pl-pds">"</span></span>&gt;
      &lt;<span class="pl-ent">dl</span>&gt;
        &lt;<span class="pl-ent">dt</span>&gt;Username:&lt;/<span class="pl-ent">dt</span>&gt;
        &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text<span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>username<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
        &lt;<span class="pl-ent">dt</span>&gt;Password:&lt;/<span class="pl-ent">dt</span>&gt;
        &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>password<span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>password<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
        &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>submit<span class="pl-pds">"</span></span> <span class="pl-e">value</span>=<span class="pl-s"><span class="pl-pds">"</span>Login<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
      &lt;/<span class="pl-ent">dl</span>&gt;
    &lt;/<span class="pl-ent">form</span>&gt;

  &lt;/<span class="pl-ent">div</span>&gt;

&lt;/<span class="pl-ent">body</span>&gt;
&lt;/<span class="pl-ent">html</span>&gt;</pre></div>
</li>
<li>
<p>Run the tests again.</p>
<p>You should still see some errors! Look at one of the errors - <code>werkzeug.routing.BuildError: Could not build url for endpoint 'index'. Did you mean 'login' instead?</code></p>
<p>Essentially, we are trying to redirect to the <code>index()</code> function, which does not exist. Rename the <code>show_entries()</code> function to <code>index()</code> within <em>app.py</em> then re-test:</p>
<div class="highlight highlight-source-shell"><pre>Ran 5 tests <span class="pl-k">in</span> 0.048s

FAILED (failures=1, errors=2)</pre></div>
</li>
<li>
<p>Next, add in a view for adding entries:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/add<span class="pl-pds">'</span></span>, <span class="pl-v">methods</span><span class="pl-k">=</span>[<span class="pl-s"><span class="pl-pds">'</span>POST<span class="pl-pds">'</span></span>])
<span class="pl-k">def</span> <span class="pl-en">add_entry</span>():
    <span class="pl-s"><span class="pl-pds">"""</span>Add new post to database.<span class="pl-pds">"""</span></span>
    <span class="pl-k">if</span> <span class="pl-k">not</span> session.get(<span class="pl-s"><span class="pl-pds">'</span>logged_in<span class="pl-pds">'</span></span>):
        abort(<span class="pl-c1">401</span>)
    db <span class="pl-k">=</span> get_db()
    db.execute(
        <span class="pl-s"><span class="pl-pds">'</span>insert into entries (title, text) values (?, ?)<span class="pl-pds">'</span></span>,
        [request.form[<span class="pl-s"><span class="pl-pds">'</span>title<span class="pl-pds">'</span></span>], request.form[<span class="pl-s"><span class="pl-pds">'</span>text<span class="pl-pds">'</span></span>]]
    )
    db.commit()
    flash(<span class="pl-s"><span class="pl-pds">'</span>New entry was successfully posted<span class="pl-pds">'</span></span>)
    <span class="pl-k">return</span> redirect(url_for(<span class="pl-s"><span class="pl-pds">'</span>index<span class="pl-pds">'</span></span>))</pre></div>
</li>
<li>
<p>Retest.</p>
<p>Now you should see:</p>
<div class="highlight highlight-source-shell"><pre>..F..
======================================================================
FAIL: test_empty_db (__main__.FlaskrTestCase)
Ensure database is blank.
----------------------------------------------------------------------
Traceback (most recent call last):
  File <span class="pl-s"><span class="pl-pds">"</span>app-test.py<span class="pl-pds">"</span></span>, line 52, <span class="pl-k">in</span> test_empty_db
    assert b<span class="pl-s"><span class="pl-pds">'</span>No entries here so far<span class="pl-pds">'</span></span> <span class="pl-k">in</span> rv.data
AssertionError

----------------------------------------------------------------------
Ran 5 tests <span class="pl-k">in</span> 0.054s

FAILED (failures=1)</pre></div>
<p>This error is asserting that when the route <code>/</code> is hit, the message "No entries here so far" is returned. Check the <em>index.html</em> template. The message actually reads "No entries yet. Add some!". So update the test and then retest:</p>
<div class="highlight highlight-source-shell"><pre>Ran 5 tests <span class="pl-k">in</span> 0.055s

OK</pre></div>
<p>Perfect.</p>
</li>
</ol>
<h2><a id="user-content-add-some-color" class="anchor" aria-hidden="true" href="#add-some-color"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Add Some Color</h2>
<p>Save the following styles to a new file called <em>style.css</em> in the "static" folder:</p>
<div class="highlight highlight-source-css"><pre><span class="pl-ent">body</span> {
  <span class="pl-c1"><span class="pl-c1">font-family</span></span>: <span class="pl-c1">sans-serif</span>;
  <span class="pl-c1"><span class="pl-c1">background</span></span>: <span class="pl-c1">#eee</span>;
}

<span class="pl-ent">a</span>, <span class="pl-ent">h1</span>, <span class="pl-ent">h2</span> {
  <span class="pl-c1"><span class="pl-c1">color</span></span>: <span class="pl-c1">#377BA8</span>;
}

<span class="pl-ent">h1</span>, <span class="pl-ent">h2</span> {
  <span class="pl-c1"><span class="pl-c1">font-family</span></span>: <span class="pl-s"><span class="pl-pds">'</span>Georgia<span class="pl-pds">'</span></span>, <span class="pl-c1">serif</span>;
  <span class="pl-c1"><span class="pl-c1">margin</span></span>: <span class="pl-c1">0</span>;
}

<span class="pl-ent">h1</span> {
  <span class="pl-c1"><span class="pl-c1">border-bottom</span></span>: <span class="pl-c1">2<span class="pl-k">px</span></span> <span class="pl-c1">solid</span> <span class="pl-c1">#eee</span>;
}

<span class="pl-ent">h2</span> {
  <span class="pl-c1"><span class="pl-c1">font-size</span></span>: <span class="pl-c1">1.2<span class="pl-k">em</span></span>;
}

<span class="pl-e">.page</span> {
  <span class="pl-c1"><span class="pl-c1">margin</span></span>: <span class="pl-c1">2<span class="pl-k">em</span></span> <span class="pl-c1">auto</span>;
  <span class="pl-c1"><span class="pl-c1">width</span></span>: <span class="pl-c1">35<span class="pl-k">em</span></span>;
  <span class="pl-c1"><span class="pl-c1">border</span></span>: <span class="pl-c1">5<span class="pl-k">px</span></span> <span class="pl-c1">solid</span> <span class="pl-c1">#ccc</span>;
  <span class="pl-c1"><span class="pl-c1">padding</span></span>: <span class="pl-c1">0.8<span class="pl-k">em</span></span>;
  <span class="pl-c1"><span class="pl-c1">background</span></span>: <span class="pl-c1">white</span>;
}

<span class="pl-e">.entries</span> {
  <span class="pl-c1"><span class="pl-c1">list-style</span></span>: <span class="pl-c1">none</span>;
  <span class="pl-c1"><span class="pl-c1">margin</span></span>: <span class="pl-c1">0</span>;
  <span class="pl-c1"><span class="pl-c1">padding</span></span>: <span class="pl-c1">0</span>;
}

<span class="pl-e">.entries</span> <span class="pl-ent">li</span> {
  <span class="pl-c1"><span class="pl-c1">margin</span></span>: <span class="pl-c1">0.8<span class="pl-k">em</span></span> <span class="pl-c1">1.2<span class="pl-k">em</span></span>;
}

<span class="pl-e">.entries</span> <span class="pl-ent">li</span> <span class="pl-ent">h2</span> {
  <span class="pl-c1"><span class="pl-c1">margin-left</span></span>: <span class="pl-c1">-1<span class="pl-k">em</span></span>;
}

<span class="pl-e">.add-entry</span> {
  <span class="pl-c1"><span class="pl-c1">font-size</span></span>: <span class="pl-c1">0.9<span class="pl-k">em</span></span>;
  <span class="pl-c1"><span class="pl-c1">border-bottom</span></span>: <span class="pl-c1">1<span class="pl-k">px</span></span> <span class="pl-c1">solid</span> <span class="pl-c1">#ccc</span>;
}

<span class="pl-e">.add-entry</span> <span class="pl-ent">dl</span> {
  <span class="pl-c1"><span class="pl-c1">font-weight</span></span>: <span class="pl-c1">bold</span>;
}

<span class="pl-e">.metanav</span> {
  <span class="pl-c1"><span class="pl-c1">text-align</span></span>: <span class="pl-c1">right</span>;
  <span class="pl-c1"><span class="pl-c1">font-size</span></span>: <span class="pl-c1">0.8<span class="pl-k">em</span></span>;
  <span class="pl-c1"><span class="pl-c1">padding</span></span>: <span class="pl-c1">0.3<span class="pl-k">em</span></span>;
  <span class="pl-c1"><span class="pl-c1">margin-bottom</span></span>: <span class="pl-c1">1<span class="pl-k">em</span></span>;
  <span class="pl-c1"><span class="pl-c1">background</span></span>: <span class="pl-c1">#fafafa</span>;
}

<span class="pl-e">.flash</span> {
  <span class="pl-c1"><span class="pl-c1">background</span></span>: <span class="pl-c1">#CEE5F5</span>;
  <span class="pl-c1"><span class="pl-c1">padding</span></span>: <span class="pl-c1">0.5<span class="pl-k">em</span></span>;
  <span class="pl-c1"><span class="pl-c1">border</span></span>: <span class="pl-c1">1<span class="pl-k">px</span></span> <span class="pl-c1">solid</span> <span class="pl-c1">#AACBE2</span>;
}

<span class="pl-e">.error</span> {
  <span class="pl-c1"><span class="pl-c1">background</span></span>: <span class="pl-c1">#F0D6D6</span>;
  <span class="pl-c1"><span class="pl-c1">padding</span></span>: <span class="pl-c1">0.5<span class="pl-k">em</span></span>;
}</pre></div>
<h2><a id="user-content-test" class="anchor" aria-hidden="true" href="#test"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Test</h2>
<p>Run you app, log in (username/password = "admin"), add a post, log out.</p>
<h2><a id="user-content-javascript" class="anchor" aria-hidden="true" href="#javascript"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>JavaScript</h2>
<p>Now let's add some JavaScript to make the site slightly more interactive.</p>
<ol>
<li>
<p>Open <em>index.html</em> and update the first <code>&lt;li</code>&gt; like so:</p>
<div class="highlight highlight-text-html-basic"><pre>&lt;<span class="pl-ent">li</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>entry<span class="pl-pds">"</span></span>&gt;
  &lt;<span class="pl-ent">h2</span> <span class="pl-e">id</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ entry.id }}<span class="pl-pds">"</span></span>&gt;{{ entry.title }}&lt;/<span class="pl-ent">h2</span>&gt;
  {{ entry.text|safe }}
&lt;/<span class="pl-ent">li</span>&gt;</pre></div>
<p>Now we can use jQuery to target each <code>&lt;li</code>&gt;. First, we need to add the following scripts to the document just before the closing body tag:</p>
<div class="highlight highlight-text-html-basic"><pre>&lt;<span class="pl-ent">script</span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>//code.jquery.com/jquery-2.2.4.min.js<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;
&lt;<span class="pl-ent">script</span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>//stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;
&lt;<span class="pl-ent">script</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/javascript<span class="pl-pds">"</span></span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>{{url_for('static', filename='main.js') }}<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;</pre></div>
</li>
<li>
<p>Create a <em>main.js</em> file in your "static" directory and add the following code:</p>
<div class="highlight highlight-source-js"><pre><span class="pl-en">$</span>(<span class="pl-k">function</span>() {
  <span class="pl-en">console</span>.<span class="pl-c1">log</span>(<span class="pl-s"><span class="pl-pds">'</span>ready!<span class="pl-pds">'</span></span>); <span class="pl-c"><span class="pl-c">//</span> sanity check</span>
});

<span class="pl-en">$</span>(<span class="pl-s"><span class="pl-pds">'</span>.entry<span class="pl-pds">'</span></span>).<span class="pl-en">on</span>(<span class="pl-s"><span class="pl-pds">'</span>click<span class="pl-pds">'</span></span>, <span class="pl-k">function</span>() {
  <span class="pl-k">var</span> entry <span class="pl-k">=</span> <span class="pl-c1">this</span>;
  <span class="pl-k">var</span> post_id <span class="pl-k">=</span> <span class="pl-en">$</span>(<span class="pl-c1">this</span>).<span class="pl-c1">find</span>(<span class="pl-s"><span class="pl-pds">'</span>h2<span class="pl-pds">'</span></span>).<span class="pl-en">attr</span>(<span class="pl-s"><span class="pl-pds">'</span>id<span class="pl-pds">'</span></span>);
  <span class="pl-smi">$</span>.<span class="pl-en">ajax</span>({
    type<span class="pl-k">:</span><span class="pl-s"><span class="pl-pds">'</span>GET<span class="pl-pds">'</span></span>,
    url<span class="pl-k">:</span> <span class="pl-s"><span class="pl-pds">'</span>/delete<span class="pl-pds">'</span></span> <span class="pl-k">+</span> <span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span> <span class="pl-k">+</span> post_id,
    context<span class="pl-k">:</span> entry,
    <span class="pl-en">success</span><span class="pl-k">:</span><span class="pl-k">function</span>(<span class="pl-smi">result</span>) {
      <span class="pl-k">if</span>(<span class="pl-smi">result</span>.<span class="pl-c1">status</span> <span class="pl-k">===</span> <span class="pl-c1">1</span>) {
        <span class="pl-en">$</span>(<span class="pl-c1">this</span>).<span class="pl-c1">remove</span>();
        <span class="pl-en">console</span>.<span class="pl-c1">log</span>(result);
      }
    }
  });
});</pre></div>
</li>
<li>
<p>Add a new function in <em>app.py</em> to remove the post from the database:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/delete/&lt;post_id&gt;<span class="pl-pds">'</span></span>, <span class="pl-v">methods</span><span class="pl-k">=</span>[<span class="pl-s"><span class="pl-pds">'</span>GET<span class="pl-pds">'</span></span>])
<span class="pl-k">def</span> <span class="pl-en">delete_entry</span>(<span class="pl-smi">post_id</span>):
    <span class="pl-s"><span class="pl-pds">'''</span>Delete post from database<span class="pl-pds">'''</span></span>
    result <span class="pl-k">=</span> {<span class="pl-s"><span class="pl-pds">'</span>status<span class="pl-pds">'</span></span>: <span class="pl-c1">0</span>, <span class="pl-s"><span class="pl-pds">'</span>message<span class="pl-pds">'</span></span>: <span class="pl-s"><span class="pl-pds">'</span>Error<span class="pl-pds">'</span></span>}
    <span class="pl-k">try</span>:
        db <span class="pl-k">=</span> get_db()
        db.execute(<span class="pl-s"><span class="pl-pds">'</span>delete from entries where id=<span class="pl-pds">'</span></span> <span class="pl-k">+</span> post_id)
        db.commit()
        result <span class="pl-k">=</span> {<span class="pl-s"><span class="pl-pds">'</span>status<span class="pl-pds">'</span></span>: <span class="pl-c1">1</span>, <span class="pl-s"><span class="pl-pds">'</span>message<span class="pl-pds">'</span></span>: <span class="pl-s"><span class="pl-pds">"</span>Post Deleted<span class="pl-pds">"</span></span>}
    <span class="pl-k">except</span> <span class="pl-c1">Exception</span> <span class="pl-k">as</span> e:
        result <span class="pl-k">=</span> {<span class="pl-s"><span class="pl-pds">'</span>status<span class="pl-pds">'</span></span>: <span class="pl-c1">0</span>, <span class="pl-s"><span class="pl-pds">'</span>message<span class="pl-pds">'</span></span>: <span class="pl-c1">repr</span>(e)}

    <span class="pl-k">return</span> jsonify(result)</pre></div>
</li>
<li>
<p>Finally, add a new test:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">def</span> <span class="pl-en">test_delete_message</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
    <span class="pl-s"><span class="pl-pds">"""</span>Ensure the messages are being deleted.<span class="pl-pds">"""</span></span>
    rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.app.get(<span class="pl-s"><span class="pl-pds">'</span>/delete/1<span class="pl-pds">'</span></span>)
    data <span class="pl-k">=</span> json.loads((rv.data).decode(<span class="pl-s"><span class="pl-pds">'</span>utf-8<span class="pl-pds">'</span></span>))
    <span class="pl-c1">self</span>.assertEqual(data[<span class="pl-s"><span class="pl-pds">'</span>status<span class="pl-pds">'</span></span>], <span class="pl-c1">1</span>)</pre></div>
<p>Make sure to add the following import as well - <code>import json</code>.</p>
<p>Manually test this out by running the server and adding two new entries. Click on one of them. It should be removed from the DOM as well as the database. Double check this.</p>
<p>Then run your automated test suite. It should pass:</p>
<div class="highlight highlight-source-shell"><pre>......
----------------------------------------------------------------------
Ran 6 tests <span class="pl-k">in</span> 0.062s

OK</pre></div>
</li>
</ol>
<h2><a id="user-content-deployment" class="anchor" aria-hidden="true" href="#deployment"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Deployment</h2>
<p>With the app in a working state, let's shift gears and deploy the app to <a href="https://www.heroku.com" rel="nofollow">Heroku</a>.</p>
<ol>
<li>
<p>To do this, first sign up and then install the <a href="https://devcenter.heroku.com/articles/heroku-cli" rel="nofollow">Heroku CLI</a>.</p>
</li>
<li>
<p>Next, install a production-grade web server called <a href="http://gunicorn.org/" rel="nofollow">gunicorn</a>:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ pip install gunicorn==19.9.0</pre></div>
</li>
<li>
<p>Create a <a href="https://devcenter.heroku.com/articles/procfile" rel="nofollow">Procfile</a> in the project root:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ touch Procfile</pre></div>
<p>And add the following code:</p>
<div class="highlight highlight-source-shell"><pre>web: gunicorn app:app</pre></div>
</li>
<li>
<p>Create a <em>requirements.txt</em> file to specify the external dependencies that need to be installed for the app to work:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ pip freeze <span class="pl-k">&gt;</span> requirements.txt</pre></div>
</li>
<li>
<p>Create a <em>.gitignore</em> file in the project root:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ touch .gitignore</pre></div>
<p>And include the following files and folders (so they are not included in version control):</p>
<div class="highlight highlight-source-shell"><pre>env
<span class="pl-k">*</span>.pyc
<span class="pl-k">*</span>.DS_Store
__pycache__</pre></div>
</li>
<li>
<p>To specify the correct Python runtime, add a new file to the project root called <em>runtime.txt</em>:</p>
<pre><code>python-3.7.0
</code></pre>
</li>
<li>
<p>Add a local Git repo:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ git init
(env)$ git add -A
(env)$ git commit -m <span class="pl-s"><span class="pl-pds">"</span>initial<span class="pl-pds">"</span></span></pre></div>
</li>
<li>
<p>Deploy to Heroku:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ heroku create
(env)$ git push heroku master</pre></div>
</li>
</ol>
<h2><a id="user-content-test-again" class="anchor" aria-hidden="true" href="#test-again"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Test (again!)</h2>
<p>Let's test this in the cloud. Run <code>heroku open</code> to open the app in the browser.</p>
<h2><a id="user-content-bootstrap" class="anchor" aria-hidden="true" href="#bootstrap"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Bootstrap</h2>
<p>Let's update the styles with <a href="http://getbootstrap.com/" rel="nofollow">Bootstrap 4</a>.</p>
<ol>
<li>
<p>First, remove the <em>style.css</em> stylesheet from both <em>index.html</em> and <em>login.html</em>. Then add this stylesheet to both files:</p>
<div class="highlight highlight-text-html-basic"><pre>&lt;<span class="pl-ent">link</span> <span class="pl-e">rel</span>=<span class="pl-s"><span class="pl-pds">"</span>stylesheet<span class="pl-pds">"</span></span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/css<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>//stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css<span class="pl-pds">"</span></span>&gt;</pre></div>
<p>Now we have full access to all of the Bootstrap helper classes.</p>
</li>
<li>
<p>Replace the code in <em>login.html</em> with:</p>
<div class="highlight highlight-text-html-basic"><pre>&lt;!DOCTYPE html&gt;
&lt;<span class="pl-ent">html</span>&gt;
&lt;<span class="pl-ent">head</span>&gt;
  &lt;<span class="pl-ent">title</span>&gt;Flaskr-TDD | Login&lt;/<span class="pl-ent">title</span>&gt;
  &lt;<span class="pl-ent">link</span> <span class="pl-e">rel</span>=<span class="pl-s"><span class="pl-pds">"</span>stylesheet<span class="pl-pds">"</span></span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/css<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>//stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css<span class="pl-pds">"</span></span>&gt;
&lt;/<span class="pl-ent">head</span>&gt;
&lt;<span class="pl-ent">body</span>&gt;

  &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>container<span class="pl-pds">"</span></span>&gt;

    &lt;<span class="pl-ent">h1</span>&gt;Flaskr&lt;/<span class="pl-ent">h1</span>&gt;

    &lt;<span class="pl-ent">br</span>&gt;&lt;<span class="pl-ent">br</span>&gt;

    {% for message in get_flashed_messages() %}
      &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>flash alert alert-success col-sm-4<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>success<span class="pl-pds">"</span></span>&gt;{{ message }}&lt;/<span class="pl-ent">div</span>&gt;
    {% endfor %}

    &lt;<span class="pl-ent">h3</span>&gt;Login&lt;/<span class="pl-ent">h3</span>&gt;

    {% if error %}&lt;<span class="pl-ent">p</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>alert alert-danger col-sm-4<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>danger<span class="pl-pds">"</span></span>&gt;&lt;<span class="pl-ent">strong</span>&gt;Error:&lt;/<span class="pl-ent">strong</span>&gt; {{ error }}{% endif %}&lt;/<span class="pl-ent">p</span>&gt;
    &lt;<span class="pl-ent">form</span> <span class="pl-e">action</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('login') }}<span class="pl-pds">"</span></span> <span class="pl-e">method</span>=<span class="pl-s"><span class="pl-pds">"</span>post<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>form-group<span class="pl-pds">"</span></span>&gt;
      &lt;<span class="pl-ent">dl</span>&gt;
        &lt;<span class="pl-ent">dt</span>&gt;Username:&lt;/<span class="pl-ent">dt</span>&gt;
        &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text<span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>username<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>form-control col-sm-4<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
        &lt;<span class="pl-ent">dt</span>&gt;Password:&lt;/<span class="pl-ent">dt</span>&gt;
        &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>password<span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>password<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>form-control col-sm-4<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
        &lt;<span class="pl-ent">br</span>&gt;&lt;<span class="pl-ent">br</span>&gt;
        &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>submit<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>btn btn-primary<span class="pl-pds">"</span></span> <span class="pl-e">value</span>=<span class="pl-s"><span class="pl-pds">"</span>Login<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
        &lt;<span class="pl-ent">span</span>&gt;Use "admin" for username and password&lt;/<span class="pl-ent">span</span>&gt;
      &lt;/<span class="pl-ent">dl</span>&gt;
    &lt;/<span class="pl-ent">form</span>&gt;

  &lt;/<span class="pl-ent">div</span>&gt;

  &lt;<span class="pl-ent">script</span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>//code.jquery.com/jquery-2.2.4.min.js<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;
  &lt;<span class="pl-ent">script</span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>//stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;
  &lt;<span class="pl-ent">script</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/javascript<span class="pl-pds">"</span></span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>{{url_for('static', filename='main.js') }}<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;

&lt;/<span class="pl-ent">body</span>&gt;
&lt;/<span class="pl-ent">html</span>&gt;</pre></div>
</li>
<li>
<p>And replace the code in <em>index.html</em> with:</p>
<div class="highlight highlight-text-html-basic"><pre>&lt;!DOCTYPE html&gt;
&lt;<span class="pl-ent">html</span>&gt;
&lt;<span class="pl-ent">head</span>&gt;
  &lt;<span class="pl-ent">title</span>&gt;Flaskr&lt;/<span class="pl-ent">title</span>&gt;
  &lt;<span class="pl-ent">link</span> <span class="pl-e">rel</span>=<span class="pl-s"><span class="pl-pds">"</span>stylesheet<span class="pl-pds">"</span></span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/css<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>//stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css<span class="pl-pds">"</span></span>&gt;
&lt;/<span class="pl-ent">head</span>&gt;
&lt;<span class="pl-ent">body</span>&gt;

  &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>container<span class="pl-pds">"</span></span>&gt;

    &lt;<span class="pl-ent">h1</span>&gt;Flaskr-TDD&lt;/<span class="pl-ent">h1</span>&gt;

    {% if not session.logged_in %}
      &lt;<span class="pl-ent">a</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>btn btn-success<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>button<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('login') }}<span class="pl-pds">"</span></span>&gt;log in&lt;/<span class="pl-ent">a</span>&gt;
    {% else %}
      &lt;<span class="pl-ent">a</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>btn btn-warning<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>button<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('logout') }}<span class="pl-pds">"</span></span>&gt;log out&lt;/<span class="pl-ent">a</span>&gt;
    {% endif %}

    &lt;<span class="pl-ent">br</span>&gt;&lt;<span class="pl-ent">br</span>&gt;

    {% for message in get_flashed_messages() %}
      &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>flash alert alert-success col-sm-4<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>success<span class="pl-pds">"</span></span>&gt;{{ message }}&lt;/<span class="pl-ent">div</span>&gt;
    {% endfor %}

    {% if session.logged_in %}
      &lt;<span class="pl-ent">form</span> <span class="pl-e">action</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('add_entry') }}<span class="pl-pds">"</span></span> <span class="pl-e">method</span>=<span class="pl-s"><span class="pl-pds">"</span>post<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>add-entry form-group<span class="pl-pds">"</span></span>&gt;
        &lt;<span class="pl-ent">dl</span>&gt;
          &lt;<span class="pl-ent">dt</span>&gt;Title:&lt;/<span class="pl-ent">dt</span>&gt;
          &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text<span class="pl-pds">"</span></span> <span class="pl-e">size</span>=<span class="pl-s"><span class="pl-pds">"</span>30<span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>title<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>form-control col-sm-4<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
          &lt;<span class="pl-ent">dt</span>&gt;Text:&lt;/<span class="pl-ent">dt</span>&gt;
          &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">textarea</span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>text<span class="pl-pds">"</span></span> <span class="pl-e">rows</span>=<span class="pl-s"><span class="pl-pds">"</span>5<span class="pl-pds">"</span></span> <span class="pl-e">cols</span>=<span class="pl-s"><span class="pl-pds">"</span>40<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>form-control col-sm-4<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">textarea</span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
          &lt;<span class="pl-ent">br</span>&gt;&lt;<span class="pl-ent">br</span>&gt;
          &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>submit<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>btn btn-primary<span class="pl-pds">"</span></span> <span class="pl-e">value</span>=<span class="pl-s"><span class="pl-pds">"</span>Share<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">dd</span>&gt;
        &lt;/<span class="pl-ent">dl</span>&gt;
      &lt;/<span class="pl-ent">form</span>&gt;
    {% endif %}

    &lt;<span class="pl-ent">br</span>&gt;

    &lt;<span class="pl-ent">ul</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>entries<span class="pl-pds">"</span></span>&gt;
      {% for entry in entries %}
        &lt;<span class="pl-ent">li</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>entry<span class="pl-pds">"</span></span>&gt;&lt;<span class="pl-ent">h2</span> <span class="pl-e">id</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ entry.id }}<span class="pl-pds">"</span></span>&gt;{{ entry.title }}&lt;/<span class="pl-ent">h2</span>&gt;{{ entry.text|safe }}&lt;/<span class="pl-ent">li</span>&gt;
      {% else %}
        &lt;<span class="pl-ent">li</span>&gt;&lt;<span class="pl-ent">em</span>&gt;No entries yet. Add some!&lt;/<span class="pl-ent">em</span>&gt;&lt;/<span class="pl-ent">li</span>&gt;
      {% endfor %}
    &lt;/<span class="pl-ent">ul</span>&gt;

  &lt;/<span class="pl-ent">div</span>&gt;

  &lt;<span class="pl-ent">script</span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>//code.jquery.com/jquery-2.2.4.min.js<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;
  &lt;<span class="pl-ent">script</span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>//stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;
  &lt;<span class="pl-ent">script</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/javascript<span class="pl-pds">"</span></span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>{{url_for('static', filename='main.js') }}<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;

&lt;/<span class="pl-ent">body</span>&gt;
&lt;/<span class="pl-ent">html</span>&gt;</pre></div>
</li>
<li>
<p>Run the app locally:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ python app.py</pre></div>
<p>Check out your changes in the browser!</p>
</li>
</ol>
<h2><a id="user-content-sqlalchemy" class="anchor" aria-hidden="true" href="#sqlalchemy"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>SQLAlchemy</h2>
<p>Let's upgrade to <a href="http://pythonhosted.org/Flask-SQLAlchemy/" rel="nofollow">Flask-SQLAlchemy</a>, in order to better manage our database.</p>
<h3><a id="user-content-setup-sqlalchemy" class="anchor" aria-hidden="true" href="#setup-sqlalchemy"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Setup SQLAlchemy</h3>
<ol>
<li>
<p>Start by installing Flask-SQLAlchemy:</p>
<div class="highlight highlight-source-shell"><pre>$ pip install Flask-SQLAlchemy==2.3.2</pre></div>
</li>
<li>
<p>Create a <em>create_db.py</em> file, then add the following code:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-c"><span class="pl-c">#</span> create_db.py</span>


<span class="pl-k">from</span> app <span class="pl-k">import</span> db
<span class="pl-k">from</span> models <span class="pl-k">import</span> Flaskr


<span class="pl-c"><span class="pl-c">#</span> create the database and the db table</span>
db.create_all()

<span class="pl-c"><span class="pl-c">#</span> commit the changes</span>
db.session.commit()</pre></div>
<p>This file will be used to create our new database. Go ahead and delete the old <em>.db</em> (<em>flaskr.db</em>) along with the <em>schema.sql</em> file.</p>
</li>
<li>
<p>Next add a <em>models.py</em> file, which will be used to generate the new schema:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">from</span> app <span class="pl-k">import</span> db


<span class="pl-k">class</span> <span class="pl-en">Flaskr</span>(<span class="pl-e">db</span>.<span class="pl-e">Model</span>):

    __tablename__ <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">"</span>flaskr<span class="pl-pds">"</span></span>

    post_id <span class="pl-k">=</span> db.Column(db.Integer, <span class="pl-v">primary_key</span><span class="pl-k">=</span><span class="pl-c1">True</span>)
    title <span class="pl-k">=</span> db.Column(db.String, <span class="pl-v">nullable</span><span class="pl-k">=</span><span class="pl-c1">False</span>)
    text <span class="pl-k">=</span> db.Column(db.String, <span class="pl-v">nullable</span><span class="pl-k">=</span><span class="pl-c1">False</span>)

    <span class="pl-k">def</span> <span class="pl-c1">__init__</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>, <span class="pl-smi">title</span>, <span class="pl-smi">text</span>):
        <span class="pl-c1">self</span>.title <span class="pl-k">=</span> title
        <span class="pl-c1">self</span>.text <span class="pl-k">=</span> text

    <span class="pl-k">def</span> <span class="pl-c1">__repr__</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-k">return</span> <span class="pl-s"><span class="pl-pds">'</span>&lt;title <span class="pl-c1">{}</span>&gt;<span class="pl-pds">'</span></span>.format(<span class="pl-c1">self</span>.body)</pre></div>
</li>
</ol>
<h3><a id="user-content-update-apppy" class="anchor" aria-hidden="true" href="#update-apppy"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Update <em>app.py</em></h3>
<div class="highlight highlight-source-python"><pre><span class="pl-c"><span class="pl-c">#</span> imports</span>
<span class="pl-k">import</span> os

<span class="pl-k">from</span> flask <span class="pl-k">import</span> Flask, request, session, g, redirect, url_for, \
     abort, render_template, flash, jsonify
<span class="pl-k">from</span> flask_sqlalchemy <span class="pl-k">import</span> SQLAlchemy


<span class="pl-c"><span class="pl-c">#</span> get the folder where this file runs</span>
basedir <span class="pl-k">=</span> os.path.abspath(os.path.dirname(<span class="pl-c1">__file__</span>))

<span class="pl-c"><span class="pl-c">#</span> configuration</span>
<span class="pl-c1">DATABASE</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>flaskr.db<span class="pl-pds">'</span></span>
<span class="pl-c1">DEBUG</span> <span class="pl-k">=</span> <span class="pl-c1">True</span>
<span class="pl-c1">SECRET_KEY</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>my_precious<span class="pl-pds">'</span></span>
<span class="pl-c1">USERNAME</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>admin<span class="pl-pds">'</span></span>
<span class="pl-c1">PASSWORD</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>admin<span class="pl-pds">'</span></span>

<span class="pl-c"><span class="pl-c">#</span> define the full path for the database</span>
<span class="pl-c1">DATABASE_PATH</span> <span class="pl-k">=</span> os.path.join(basedir, <span class="pl-c1">DATABASE</span>)

<span class="pl-c"><span class="pl-c">#</span> database config</span>
<span class="pl-c1">SQLALCHEMY_DATABASE_URI</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>sqlite:///<span class="pl-pds">'</span></span> <span class="pl-k">+</span> <span class="pl-c1">DATABASE_PATH</span>
<span class="pl-c1">SQLALCHEMY_TRACK_MODIFICATIONS</span> <span class="pl-k">=</span> <span class="pl-c1">False</span>

<span class="pl-c"><span class="pl-c">#</span> create app</span>
app <span class="pl-k">=</span> Flask(<span class="pl-c1">__name__</span>)
app.config.from_object(<span class="pl-c1">__name__</span>)
db <span class="pl-k">=</span> SQLAlchemy(app)

<span class="pl-k">import</span> models


<span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>)
<span class="pl-k">def</span> <span class="pl-en">index</span>():
    <span class="pl-s"><span class="pl-pds">"""</span>Searches the database for entries, then displays them.<span class="pl-pds">"""</span></span>
    entries <span class="pl-k">=</span> db.session.query(models.Flaskr)
    <span class="pl-k">return</span> render_template(<span class="pl-s"><span class="pl-pds">'</span>index.html<span class="pl-pds">'</span></span>, <span class="pl-v">entries</span><span class="pl-k">=</span>entries)


<span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/add<span class="pl-pds">'</span></span>, <span class="pl-v">methods</span><span class="pl-k">=</span>[<span class="pl-s"><span class="pl-pds">'</span>POST<span class="pl-pds">'</span></span>])
<span class="pl-k">def</span> <span class="pl-en">add_entry</span>():
    <span class="pl-s"><span class="pl-pds">"""</span>Adds new post to the database.<span class="pl-pds">"""</span></span>
    <span class="pl-k">if</span> <span class="pl-k">not</span> session.get(<span class="pl-s"><span class="pl-pds">'</span>logged_in<span class="pl-pds">'</span></span>):
        abort(<span class="pl-c1">401</span>)
    new_entry <span class="pl-k">=</span> models.Flaskr(request.form[<span class="pl-s"><span class="pl-pds">'</span>title<span class="pl-pds">'</span></span>], request.form[<span class="pl-s"><span class="pl-pds">'</span>text<span class="pl-pds">'</span></span>])
    db.session.add(new_entry)
    db.session.commit()
    flash(<span class="pl-s"><span class="pl-pds">'</span>New entry was successfully posted<span class="pl-pds">'</span></span>)
    <span class="pl-k">return</span> redirect(url_for(<span class="pl-s"><span class="pl-pds">'</span>index<span class="pl-pds">'</span></span>))


<span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/login<span class="pl-pds">'</span></span>, <span class="pl-v">methods</span><span class="pl-k">=</span>[<span class="pl-s"><span class="pl-pds">'</span>GET<span class="pl-pds">'</span></span>, <span class="pl-s"><span class="pl-pds">'</span>POST<span class="pl-pds">'</span></span>])
<span class="pl-k">def</span> <span class="pl-en">login</span>():
    <span class="pl-s"><span class="pl-pds">"""</span>User login/authentication/session management.<span class="pl-pds">"""</span></span>
    error <span class="pl-k">=</span> <span class="pl-c1">None</span>
    <span class="pl-k">if</span> request.method <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>POST<span class="pl-pds">'</span></span>:
        <span class="pl-k">if</span> request.form[<span class="pl-s"><span class="pl-pds">'</span>username<span class="pl-pds">'</span></span>] <span class="pl-k">!=</span> app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>]:
            error <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>Invalid username<span class="pl-pds">'</span></span>
        <span class="pl-k">elif</span> request.form[<span class="pl-s"><span class="pl-pds">'</span>password<span class="pl-pds">'</span></span>] <span class="pl-k">!=</span> app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>]:
            error <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>Invalid password<span class="pl-pds">'</span></span>
        <span class="pl-k">else</span>:
            session[<span class="pl-s"><span class="pl-pds">'</span>logged_in<span class="pl-pds">'</span></span>] <span class="pl-k">=</span> <span class="pl-c1">True</span>
            flash(<span class="pl-s"><span class="pl-pds">'</span>You were logged in<span class="pl-pds">'</span></span>)
            <span class="pl-k">return</span> redirect(url_for(<span class="pl-s"><span class="pl-pds">'</span>index<span class="pl-pds">'</span></span>))
    <span class="pl-k">return</span> render_template(<span class="pl-s"><span class="pl-pds">'</span>login.html<span class="pl-pds">'</span></span>, <span class="pl-v">error</span><span class="pl-k">=</span>error)


<span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/logout<span class="pl-pds">'</span></span>)
<span class="pl-k">def</span> <span class="pl-en">logout</span>():
    <span class="pl-s"><span class="pl-pds">"""</span>User logout/authentication/session management.<span class="pl-pds">"""</span></span>
    session.pop(<span class="pl-s"><span class="pl-pds">'</span>logged_in<span class="pl-pds">'</span></span>, <span class="pl-c1">None</span>)
    flash(<span class="pl-s"><span class="pl-pds">'</span>You were logged out<span class="pl-pds">'</span></span>)
    <span class="pl-k">return</span> redirect(url_for(<span class="pl-s"><span class="pl-pds">'</span>index<span class="pl-pds">'</span></span>))


<span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/delete/&lt;int:post_id&gt;<span class="pl-pds">'</span></span>, <span class="pl-v">methods</span><span class="pl-k">=</span>[<span class="pl-s"><span class="pl-pds">'</span>GET<span class="pl-pds">'</span></span>])
<span class="pl-k">def</span> <span class="pl-en">delete_entry</span>(<span class="pl-smi">post_id</span>):
    <span class="pl-s"><span class="pl-pds">"""</span>Deletes post from database.<span class="pl-pds">"""</span></span>
    result <span class="pl-k">=</span> {<span class="pl-s"><span class="pl-pds">'</span>status<span class="pl-pds">'</span></span>: <span class="pl-c1">0</span>, <span class="pl-s"><span class="pl-pds">'</span>message<span class="pl-pds">'</span></span>: <span class="pl-s"><span class="pl-pds">'</span>Error<span class="pl-pds">'</span></span>}
    <span class="pl-k">try</span>:
        new_id <span class="pl-k">=</span> post_id
        db.session.query(models.Flaskr).filter_by(<span class="pl-v">post_id</span><span class="pl-k">=</span>new_id).delete()
        db.session.commit()
        result <span class="pl-k">=</span> {<span class="pl-s"><span class="pl-pds">'</span>status<span class="pl-pds">'</span></span>: <span class="pl-c1">1</span>, <span class="pl-s"><span class="pl-pds">'</span>message<span class="pl-pds">'</span></span>: <span class="pl-s"><span class="pl-pds">"</span>Post Deleted<span class="pl-pds">"</span></span>}
        flash(<span class="pl-s"><span class="pl-pds">'</span>The entry was deleted.<span class="pl-pds">'</span></span>)
    <span class="pl-k">except</span> <span class="pl-c1">Exception</span> <span class="pl-k">as</span> e:
        result <span class="pl-k">=</span> {<span class="pl-s"><span class="pl-pds">'</span>status<span class="pl-pds">'</span></span>: <span class="pl-c1">0</span>, <span class="pl-s"><span class="pl-pds">'</span>message<span class="pl-pds">'</span></span>: <span class="pl-c1">repr</span>(e)}
    <span class="pl-k">return</span> jsonify(result)


<span class="pl-k">if</span> <span class="pl-c1">__name__</span> <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>__main__<span class="pl-pds">'</span></span>:
    app.run()</pre></div>
<p>Notice the changes in the config at the top, as well the means in which we're now accessing and manipulating the database in each view function - via SQLAlchemy instead of vanilla SQL.</p>
<h3><a id="user-content-create-the-db" class="anchor" aria-hidden="true" href="#create-the-db"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Create the DB</h3>
<p>Run the following command to create the initial database:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ python create_db.py</pre></div>
<h3><a id="user-content-update-indexhtml" class="anchor" aria-hidden="true" href="#update-indexhtml"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Update <em>index.html</em></h3>
<p>Update this line:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">&lt;</span>li <span class="pl-k">class</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>entry<span class="pl-pds">"</span></span><span class="pl-k">&gt;&lt;</span>h2 <span class="pl-c1">id</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span><span class="pl-c1">{{</span> entry.post_id <span class="pl-c1">}}</span><span class="pl-pds">"</span></span><span class="pl-k">&gt;</span>{{ entry.title }}<span class="pl-k">&lt;</span><span class="pl-k">/</span>h2<span class="pl-k">&gt;</span>{{ entry.text<span class="pl-k">|</span>safe }}<span class="pl-k">&lt;</span><span class="pl-k">/</span>li<span class="pl-k">&gt;</span></pre></div>
<p>Pay attention to the <code>post_id</code>. Check the database to ensure that there is a matching field.</p>
<h3><a id="user-content-tests-1" class="anchor" aria-hidden="true" href="#tests-1"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Tests</h3>
<p>Finally, update the tests:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">import</span> unittest
<span class="pl-k">import</span> os
<span class="pl-k">import</span> json

<span class="pl-k">from</span> app <span class="pl-k">import</span> app, db

<span class="pl-c1">TEST_DB</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>test.db<span class="pl-pds">'</span></span>


<span class="pl-k">class</span> <span class="pl-en">BasicTestCase</span>(<span class="pl-e">unittest</span>.<span class="pl-e">TestCase</span>):

    <span class="pl-k">def</span> <span class="pl-en">test_index</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>initial test. ensure flask was set up correctly<span class="pl-pds">"""</span></span>
        tester <span class="pl-k">=</span> app.test_client(<span class="pl-c1">self</span>)
        response <span class="pl-k">=</span> tester.get(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>, <span class="pl-v">content_type</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>html/text<span class="pl-pds">'</span></span>)
        <span class="pl-c1">self</span>.assertEqual(response.status_code, <span class="pl-c1">200</span>)

    <span class="pl-k">def</span> <span class="pl-en">test_database</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>initial test. ensure that the database exists<span class="pl-pds">"""</span></span>
        tester <span class="pl-k">=</span> os.path.exists(<span class="pl-s"><span class="pl-pds">"</span>flaskr.db<span class="pl-pds">"</span></span>)
        <span class="pl-c1">self</span>.assertTrue(tester)


<span class="pl-k">class</span> <span class="pl-en">FlaskrTestCase</span>(<span class="pl-e">unittest</span>.<span class="pl-e">TestCase</span>):

    <span class="pl-k">def</span> <span class="pl-en">setUp</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Set up a blank temp database before each test<span class="pl-pds">"""</span></span>
        basedir <span class="pl-k">=</span> os.path.abspath(os.path.dirname(<span class="pl-c1">__file__</span>))
        app.config[<span class="pl-s"><span class="pl-pds">'</span>TESTING<span class="pl-pds">'</span></span>] <span class="pl-k">=</span> <span class="pl-c1">True</span>
        app.config[<span class="pl-s"><span class="pl-pds">'</span>SQLALCHEMY_DATABASE_URI<span class="pl-pds">'</span></span>] <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>sqlite:///<span class="pl-pds">'</span></span> <span class="pl-k">+</span> \
            os.path.join(basedir, <span class="pl-c1">TEST_DB</span>)
        <span class="pl-c1">self</span>.app <span class="pl-k">=</span> app.test_client()
        db.create_all()

    <span class="pl-k">def</span> <span class="pl-en">tearDown</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Destroy blank temp database after each test<span class="pl-pds">"""</span></span>
        db.drop_all()

    <span class="pl-k">def</span> <span class="pl-en">login</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>, <span class="pl-smi">username</span>, <span class="pl-smi">password</span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Login helper function<span class="pl-pds">"""</span></span>
        <span class="pl-k">return</span> <span class="pl-c1">self</span>.app.post(<span class="pl-s"><span class="pl-pds">'</span>/login<span class="pl-pds">'</span></span>, <span class="pl-v">data</span><span class="pl-k">=</span><span class="pl-c1">dict</span>(
            <span class="pl-v">username</span><span class="pl-k">=</span>username,
            <span class="pl-v">password</span><span class="pl-k">=</span>password
        ), <span class="pl-v">follow_redirects</span><span class="pl-k">=</span><span class="pl-c1">True</span>)

    <span class="pl-k">def</span> <span class="pl-en">logout</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Logout helper function<span class="pl-pds">"""</span></span>
        <span class="pl-k">return</span> <span class="pl-c1">self</span>.app.get(<span class="pl-s"><span class="pl-pds">'</span>/logout<span class="pl-pds">'</span></span>, <span class="pl-v">follow_redirects</span><span class="pl-k">=</span><span class="pl-c1">True</span>)

    <span class="pl-c"><span class="pl-c">#</span> assert functions</span>

    <span class="pl-k">def</span> <span class="pl-en">test_empty_db</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Ensure database is blank<span class="pl-pds">"""</span></span>
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.app.get(<span class="pl-s"><span class="pl-pds">'</span>/<span class="pl-pds">'</span></span>)
        <span class="pl-c1">self</span>.assertIn(<span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>No entries yet. Add some!<span class="pl-pds">'</span></span>, rv.data)

    <span class="pl-k">def</span> <span class="pl-en">test_login_logout</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Test login and logout using helper functions<span class="pl-pds">"""</span></span>
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.login(app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>], app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>])
        <span class="pl-c1">self</span>.assertIn(<span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>You were logged in<span class="pl-pds">'</span></span>, rv.data)
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.logout()
        <span class="pl-c1">self</span>.assertIn(<span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>You were logged out<span class="pl-pds">'</span></span>, rv.data)
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.login(app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>] <span class="pl-k">+</span> <span class="pl-s"><span class="pl-pds">'</span>x<span class="pl-pds">'</span></span>, app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>])
        <span class="pl-c1">self</span>.assertIn(<span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>Invalid username<span class="pl-pds">'</span></span>, rv.data)
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.login(app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>], app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>] <span class="pl-k">+</span> <span class="pl-s"><span class="pl-pds">'</span>x<span class="pl-pds">'</span></span>)
        <span class="pl-c1">self</span>.assertIn(<span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>Invalid password<span class="pl-pds">'</span></span>, rv.data)

    <span class="pl-k">def</span> <span class="pl-en">test_messages</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Ensure that user can post messages<span class="pl-pds">"""</span></span>
        <span class="pl-c1">self</span>.login(app.config[<span class="pl-s"><span class="pl-pds">'</span>USERNAME<span class="pl-pds">'</span></span>], app.config[<span class="pl-s"><span class="pl-pds">'</span>PASSWORD<span class="pl-pds">'</span></span>])
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.app.post(<span class="pl-s"><span class="pl-pds">'</span>/add<span class="pl-pds">'</span></span>, <span class="pl-v">data</span><span class="pl-k">=</span><span class="pl-c1">dict</span>(
            <span class="pl-v">title</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>&lt;Hello&gt;<span class="pl-pds">'</span></span>,
            <span class="pl-v">text</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">'</span>&lt;strong&gt;HTML&lt;/strong&gt; allowed here<span class="pl-pds">'</span></span>
        ), <span class="pl-v">follow_redirects</span><span class="pl-k">=</span><span class="pl-c1">True</span>)
        <span class="pl-c1">self</span>.assertNotIn(<span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>No entries here so far<span class="pl-pds">'</span></span>, rv.data)
        <span class="pl-c1">self</span>.assertIn(<span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>&amp;lt;Hello&amp;gt;<span class="pl-pds">'</span></span>, rv.data)
        <span class="pl-c1">self</span>.assertIn(<span class="pl-s"><span class="pl-k">b</span><span class="pl-pds">'</span>&lt;strong&gt;HTML&lt;/strong&gt; allowed here<span class="pl-pds">'</span></span>, rv.data)

    <span class="pl-k">def</span> <span class="pl-en">test_delete_message</span>(<span class="pl-smi"><span class="pl-smi">self</span></span>):
        <span class="pl-s"><span class="pl-pds">"""</span>Ensure the messages are being deleted<span class="pl-pds">"""</span></span>
        rv <span class="pl-k">=</span> <span class="pl-c1">self</span>.app.get(<span class="pl-s"><span class="pl-pds">'</span>/delete/1<span class="pl-pds">'</span></span>)
        data <span class="pl-k">=</span> json.loads(rv.data)
        <span class="pl-c1">self</span>.assertEqual(data[<span class="pl-s"><span class="pl-pds">'</span>status<span class="pl-pds">'</span></span>], <span class="pl-c1">1</span>)


<span class="pl-k">if</span> <span class="pl-c1">__name__</span> <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">'</span>__main__<span class="pl-pds">'</span></span>:
    unittest.main()</pre></div>
<p>We've mostly just updated the <code>setUp()</code> and <code>tearDown()</code> methods.</p>
<p>Run the tests, and then manually test it by running the server and logging in and out, adding new entries, and deleting old entries.</p>
<p>If all is well, update your requirements (<code>pip  freeze &gt; requirements.txt</code>) commit your code, then PUSH the new version to Heroku!</p>
<h2><a id="user-content-search-page" class="anchor" aria-hidden="true" href="#search-page"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Search Page</h2>
<p>Let's add a search page to our blog. It will be a nice feature that will come in handy after we have a number of blog posts.</p>
<h3><a id="user-content-update-apppy-1" class="anchor" aria-hidden="true" href="#update-apppy-1"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Update <em>app.py</em></h3>
<div class="highlight highlight-source-python"><pre><span class="pl-en">@app.route</span>(<span class="pl-s"><span class="pl-pds">'</span>/search/<span class="pl-pds">'</span></span>, <span class="pl-v">methods</span><span class="pl-k">=</span>[<span class="pl-s"><span class="pl-pds">'</span>GET<span class="pl-pds">'</span></span>])
<span class="pl-k">def</span> <span class="pl-en">search</span>():
    query <span class="pl-k">=</span> request.args.get(<span class="pl-s"><span class="pl-pds">"</span>query<span class="pl-pds">"</span></span>)
    entries <span class="pl-k">=</span> db.session.query(models.Flaskr)
    <span class="pl-k">if</span> query:
        <span class="pl-k">return</span> render_template(<span class="pl-s"><span class="pl-pds">'</span>search.html<span class="pl-pds">'</span></span>, <span class="pl-v">entries</span><span class="pl-k">=</span>entries, <span class="pl-v">query</span><span class="pl-k">=</span>query)
    <span class="pl-k">return</span> render_template(<span class="pl-s"><span class="pl-pds">'</span>search.html<span class="pl-pds">'</span></span>)</pre></div>
<blockquote>
<p><strong>NOTE</strong>: Be sure to write a test for this on your own!</p>
</blockquote>
<h3><a id="user-content-add-searchhtml" class="anchor" aria-hidden="true" href="#add-searchhtml"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Add <em>search.html</em></h3>
<p>In the "templates" folder create a new file called <em>search.html</em>:</p>
<div class="highlight highlight-source-shell"><pre>(env)$ touch search.html</pre></div>
<p>Now add the following code to <em>search.html</em>:</p>
<div class="highlight highlight-text-html-basic"><pre>&lt;!DOCTYPE html&gt;
&lt;<span class="pl-ent">html</span>&gt;
&lt;<span class="pl-ent">head</span>&gt;
  &lt;<span class="pl-ent">title</span>&gt;Flaskr&lt;/<span class="pl-ent">title</span>&gt;
  &lt;<span class="pl-ent">link</span> <span class="pl-e">rel</span>=<span class="pl-s"><span class="pl-pds">"</span>stylesheet<span class="pl-pds">"</span></span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/css<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>//stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css<span class="pl-pds">"</span></span>&gt;
&lt;/<span class="pl-ent">head</span>&gt;
&lt;<span class="pl-ent">body</span>&gt;

  &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>container<span class="pl-pds">"</span></span>&gt;

    &lt;<span class="pl-ent">h1</span>&gt;Flaskr-TDD&lt;/<span class="pl-ent">h1</span>&gt;

    &lt;<span class="pl-ent">a</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>btn btn-primary<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>button<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('index') }}<span class="pl-pds">"</span></span>&gt; Home &lt;/<span class="pl-ent">a</span>&gt;

    {% if not session.logged_in %}
      &lt;<span class="pl-ent">a</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>btn btn-success<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>button<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('login') }}<span class="pl-pds">"</span></span>&gt;log in&lt;/<span class="pl-ent">a</span>&gt;
    {% else %}
      &lt;<span class="pl-ent">a</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>btn btn-warning<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>button<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('logout') }}<span class="pl-pds">"</span></span>&gt;log out&lt;/<span class="pl-ent">a</span>&gt;
    {% endif %}

    &lt;<span class="pl-ent">br</span>&gt;&lt;<span class="pl-ent">br</span>&gt;

    {% for message in get_flashed_messages() %}
      &lt;<span class="pl-ent">div</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>flash alert alert-success col-sm-4<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>success<span class="pl-pds">"</span></span>&gt;{{ message }}&lt;/<span class="pl-ent">div</span>&gt;
    {% endfor %}

    &lt;<span class="pl-ent">form</span> <span class="pl-e">action</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('search') }}<span class="pl-pds">"</span></span> <span class="pl-e">method</span>=<span class="pl-s"><span class="pl-pds">"</span>get<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>from-group<span class="pl-pds">"</span></span>&gt;
      &lt;<span class="pl-ent">dl</span>&gt;
        &lt;<span class="pl-ent">dt</span>&gt;Search:&lt;/<span class="pl-ent">dt</span>&gt;
        &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text<span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>query<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>form-control col-sm-4<span class="pl-pds">"</span></span> &gt;&lt;/<span class="pl-ent">dd</span>&gt;
        &lt;<span class="pl-ent">br</span>&gt;
        &lt;<span class="pl-ent">dd</span>&gt;&lt;<span class="pl-ent">input</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>submit<span class="pl-pds">"</span></span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>btn btn-info<span class="pl-pds">"</span></span> <span class="pl-e">value</span>=<span class="pl-s"><span class="pl-pds">"</span>Search<span class="pl-pds">"</span></span> &gt;&lt;/<span class="pl-ent">dd</span>&gt;
      &lt;/<span class="pl-ent">dl</span>&gt;
    &lt;/<span class="pl-ent">form</span>&gt;

    &lt;<span class="pl-ent">ul</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>entries<span class="pl-pds">"</span></span>&gt;
      {% for entry in entries %}
        {% if query.lower() in entry.title.lower() or query.lower() in entry.text.lower() %}
        &lt;<span class="pl-ent">li</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>entry<span class="pl-pds">"</span></span>&gt;&lt;<span class="pl-ent">h2</span> <span class="pl-e">id</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ entry.post_id }}<span class="pl-pds">"</span></span>&gt;{{ entry.title }}&lt;/<span class="pl-ent">h2</span>&gt;{{ entry.text|safe }}&lt;/<span class="pl-ent">li</span>&gt;
        {% endif %}
      {% endfor %}
    &lt;/<span class="pl-ent">ul</span>&gt;


  &lt;/<span class="pl-ent">div</span>&gt;

  &lt;<span class="pl-ent">script</span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>//code.jquery.com/jquery-2.2.4.min.js<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;
  &lt;<span class="pl-ent">script</span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>//stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;
  &lt;<span class="pl-ent">script</span> <span class="pl-e">type</span>=<span class="pl-s"><span class="pl-pds">"</span>text/javascript<span class="pl-pds">"</span></span> <span class="pl-e">src</span>=<span class="pl-s"><span class="pl-pds">"</span>{{url_for('static', filename='main.js') }}<span class="pl-pds">"</span></span>&gt;&lt;/<span class="pl-ent">script</span>&gt;

&lt;/<span class="pl-ent">body</span>&gt;
&lt;/<span class="pl-ent">html</span>&gt;</pre></div>
<h3><a id="user-content-update-indexhtml-1" class="anchor" aria-hidden="true" href="#update-indexhtml-1"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Update <em>index.html</em></h3>
<p>Add a search button for better navigation just below <code>&lt;h1&gt;Flaskr-TDD&lt;/h1&gt;</code>:</p>
<div class="highlight highlight-text-html-basic"><pre>&lt;<span class="pl-ent">a</span> <span class="pl-e">class</span>=<span class="pl-s"><span class="pl-pds">"</span>btn btn-info<span class="pl-pds">"</span></span> <span class="pl-e">role</span>=<span class="pl-s"><span class="pl-pds">"</span>button<span class="pl-pds">"</span></span> <span class="pl-e">href</span>=<span class="pl-s"><span class="pl-pds">"</span>{{ url_for('search') }}<span class="pl-pds">"</span></span>&gt;Search&lt;/<span class="pl-ent">a</span>&gt;</pre></div>
<p>Test it out locally. If all is well, commit your code and update the version on Heroku.</p>
<h2><a id="user-content-conclusion" class="anchor" aria-hidden="true" href="#conclusion"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Conclusion</h2>