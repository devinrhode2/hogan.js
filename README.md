_The Nom Noms are coming._
------------------------------

Developer Environment Configuration:
    ruby 1.9.2
    rails 3.2.2
    gem


You may need to update rails with: `gem update rails`

**clone** down the github repo

**cd** into the project directory

Install project gems:

    $ bundle install

**Download and run the Postgres installer**
- Official download: http://www.enterprisedb.com/products-services-training/pgdownload
- During installation, use your standard system password. 
- I renamed my install directory to /usr/local/pgsql and the data source to /usr/local/pgsql/data2. These steps probably aren't necessary. Otherwise, stick with all the defaults. At the end there may be something about another install stack thing, skip this.

Now we want to become the postgres super user created from the installer:

    $ sudo su - postgres
    postgres $ psql template1
    template1=# CREATE USER okraapp WITH PASSWORD 'yourStandardPassword';
    template1=# CREATE DATABASE okraapp_development WITH OWNER = postgres;
    template1=# GRANT ALL PRIVILEGES ON DATABASE okraapp_development to okraapp;
    template1=# CREATE DATABASE okraapp_test WITH OWNER = postgres;
    template1=# GRANT ALL PRIVILEGES ON DATABASE okraapp_test to okraapp;
    template1=# \q
    postgres $ exit

**Edit config/database.yml**
You need to open config/database.yml and add in your standard password for the password: field under both development: and test:

Finish off the database stuff:

    $ rake db:migrate

if you see a bunch of output like the following you win!

    ==  AddAttachments: migrating =================================================
    -- add_column(:notifications, :attachment, :string)
       -> 0.0018s
    ==  AddAttachments: migrated (0.0029s) ========================================
    
    ==  CreateMessagesTable: migrating ============================================

And finally run rails server and go to localhost:3000 (or open http://localhost:3000) you should see an app running! Congrats!

Problems with `rake db:migrate`

Error:

    -dlopen(/Users/rangetutoring/.rvm/gems/ruby-1.9.2-p290/gems/pg-0.14.0/lib/pg_ext.bundle, 9): Library not loaded: /usr/lib/libpq.5.dylib
      Referenced from: /Users/rangetutoring/.rvm/gems/ruby-1.9.2-p290/gems/pg-0.14.0/lib/pg_ext.bundle
      Reason: image not found - /Users/rangetutoring/.rvm/gems/ruby-1.9.2-p290/gems/pg-0.14.0/lib/pg_ext.bundle

–You need to install postgres silly, that's step one.

Error:

    fe_sendauth: no password supplied
    
    Tasks: TOP => db:migrate => environment
    (See full trace by running task with --trace)

–You need to open config/database.yml and add in your standard password for the password: value

Error:

    FATAL:  password authentication failed for user "okraapp"
    
    Tasks: TOP => db:migrate => environment
    (See full trace by running task with --trace)

–You probably don't have the database user created. I think.

Error:

    FATAL:  database "okraapp_development" does not exist
    
    Tasks: TOP => db:migrate => environment
    (See full trace by running task with --trace)

–Clearly, you need to create the database. It's probably best to copy/past commands to avoid typos and syntax errors.