  <style type="text/css">
    p.p3 { font-family: 12.0px Menlo; color: #c70000}
    p.p4 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Menlo; min-height: 14.0px}
    p.p5 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Menlo; color: #c70000}
    p.p6 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Menlo}
    p.p7 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Helvetica; min-height: 14.0px}
    p.p8 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Helvetica}
    p.p9 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 18.0px; font: 12.0px Menlo; min-height: 14.0px}
    p.p10 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Helvetica; color: #bc2e00}
    p.p11 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Menlo}
    span.s1 {font: 12.0px Menlo}
    span.s2 {font: 12.0px Consolas; color: #c70000}
    span.s3 {font: 12.0px Helvetica; color: #000000}
    span.s4 {color: #000000}
    span.s5 {color: #f90000}
    span.s6 {color: #c70000}
    span.s7 {text-decoration: underline}
    span.s8 {font: 12.0px Menlo; color: #c70000}
    span.s9 {text-decoration: underline ; color: #000000}
    span.s10 {font: 12.0px Menlo; text-decoration: underline}
    span.s11 {font: 12.0px Helvetica}
  </style>
<p class="p2">You should have <span class="s1">ruby 1.9.2, rails 3.2.2, and gem</span> installed and setup.</p>
<p class="p1"><br></p>
<p class="p2">You may need to update rails with: <span class="s2">gem update rails</span></p>
<p class="p1"><br></p>
<p class="p2"><b>clone</b> down the github repo</p>
<p class="p2"><b>cd</b> into the project directory</p>
<p class="p2">to install various gems with the project, run:</p>
<p class="p3"><span class="s3">$ </span>bundle install</p>
<p class="p1"><br></p>
<p class="p2">-Download and run the Postgres installer from the official site at <a href="http://www.enterprisedb.com/products-services-training/pgdownload">http://www.enterprisedb.com/products-services-training/pgdownload</a><span class="Apple-converted-space"> </span></p>
<p class="p2">During installation, use your standard system password.<span class="Apple-converted-space"> </span></p>
<p class="p2">I renamed my install directory to /usr/local/pgsql and the data source to /usr/local/pgsql/data2. These steps probably aren't necessary. Otherwise, stick with all the defaults. At the end there may be something about another install stack thing, skip this.</p>
<p class="p1"><br></p>
<p class="p2">Now we want to become the postgres super user created from the installer:</p>
<p class="p4"><br></p>
<p class="p3"><span class="s4">$ </span>sudo su - postgres</p>
<p class="p3"><span class="s4">postgres $ </span>psql template1</p>
<p class="p5"><span class="s4">template1=# </span>CREATE USER okraapp WITH PASSWORD '<span class="s5"><b><i>yourStandardPassword</i></b></span>';</p>
<p class="p5"><span class="s4">template1=# </span>CREATE DATABASE okraapp_development WITH OWNER = postgres;</p>
<p class="p5"><span class="s4">template1=# </span>GRANT ALL PRIVILEGES ON DATABASE okraapp_development to okraapp;</p>
<p class="p5"><span class="s4">template1=# </span>CREATE DATABASE okraapp_test WITH OWNER = postgres;</p>
<p class="p5"><span class="s4">template1=# </span>GRANT ALL PRIVILEGES ON DATABASE okraapp_test to okraapp;</p>
<p class="p6">template1=# <span class="s6">\q</span></p>
<p class="p6">postgres $ <span class="s6">exit</span></p>
<p class="p7"><br></p>
<p class="p2">Edit config/database.yml</p>
<p class="p2">You need to open config/database.yml and add in your standard password for the <span class="s1">password:</span> field under <span class="s1">development:</span> and <span class="s1">test:</span></p>
<p class="p1"><br></p>
<p class="p5"><span class="s4">$ </span>rake db:migrate</p>
<p class="p7"><br></p>
<p class="p8"><span class="s7">if you see a bunch of output like the following you win!</span></p>
<p class="p7"><br></p>
<p class="p6">==<span class="Apple-converted-space">  </span>AddAttachments: migrating =================================================</p>
<p class="p6">-- add_column(:notifications, :attachment, :string)</p>
<p class="p6"><span class="Apple-converted-space">   </span>-&gt; 0.0018s</p>
<p class="p6">==<span class="Apple-converted-space">  </span>AddAttachments: migrated (0.0029s) ========================================</p>
<p class="p9"><br></p>
<p class="p6">==<span class="Apple-converted-space">  </span>CreateMessagesTable: migrating ============================================</p>
<p class="p7"><br></p>
<p class="p8">And finally run <span class="s1"><b>rails server</b></span> and go to localhost:3000 (or <span class="s8">open http://localhost:3000</span>) you should see an app running! Congrats!</p>
<p class="p7"><br></p>
<p class="p10"><span class="s9">Problems with </span><span class="s10">rake db:migrate</span><span class="s7"><span class="Apple-converted-space"> </span></span></p>
<p class="p1"><br></p>
<p class="p2"><span class="s7">Error:</span></p>
<p class="p11"><span class="s11">-</span>dlopen(/Users/rangetutoring/.rvm/gems/ruby-1.9.2-p290/gems/pg-0.14.0/lib/pg_ext.bundle, 9): Library not loaded: /usr/lib/libpq.5.dylib</p>
<p class="p11"><span class="Apple-converted-space">  </span>Referenced from: /Users/rangetutoring/.rvm/gems/ruby-1.9.2-p290/gems/pg-0.14.0/lib/pg_ext.bundle</p>
<p class="p11"><span class="Apple-converted-space">  </span>Reason: image not found - /Users/rangetutoring/.rvm/gems/ruby-1.9.2-p290/gems/pg-0.14.0/lib/pg_ext.bundle</p>
<p class="p1"><br></p>
<p class="p2">–You need to install postgres silly, that's step one.</p>
<p class="p1"><br></p>
<p class="p2"><span class="s7">Error:</span></p>
<p class="p11">fe_sendauth: no password supplied</p>
<p class="p4"><br></p>
<p class="p11">Tasks: TOP =&gt; db:migrate =&gt; environment</p>
<p class="p11">(See full trace by running task with --trace)</p>
<p class="p1"><br></p>
<p class="p2">–You need to open config/database.yml and add in your standard password for the <span class="s1">password:</span> value</p>
<p class="p1"><br></p>
<p class="p2"><span class="s7">Error:</span></p>
<p class="p11">FATAL:<span class="Apple-converted-space">  </span>password authentication failed for user "okraapp"</p>
<p class="p4"><br></p>
<p class="p11">Tasks: TOP =&gt; db:migrate =&gt; environment</p>
<p class="p11">(See full trace by running task with --trace)</p>
<p class="p1"><br></p>
<p class="p2">–You probably don't have the database user created. I think.</p>
<p class="p1"><br></p>
<p class="p2"><span class="s7">Error:</span></p>
<p class="p11">FATAL:<span class="Apple-converted-space">  </span>database "okraapp_development" does not exist</p>
<p class="p4"><br></p>
<p class="p11">Tasks: TOP =&gt; db:migrate =&gt; environment</p>
<p class="p11">(See full trace by running task with --trace)</p>
<p class="p1"><br></p>
<p class="p2">–Clearly, you need to create the database. It's probably best to copy/past commands to avoid typos and syntax errors.</p>
</body>
</html>
