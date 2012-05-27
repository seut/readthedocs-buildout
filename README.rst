==========================
 Buildout for readthedocs
==========================

This buildout will install readthedocs, including memcached and solr.

Install
=======
Boostrap buildout (using distribute):
::
   python bootstrap.py -d

Run buildout, installing all parts.
::
   bin/buildout -vv

Start all daemons (supervisor)
==============================
::
   bin/supervisord



Setup readthedoc
================

Build your database:
::
   bin/manage syncdb

This will prompt you to create a superuser account for Django. Do that. Then:
::
   bin/manage migrate

Go ahead and load in a couple users and a test projects:
::
   bin/manage loaddata test_data
   bin/manage update_repos pip

Setup solr
==========
Generate the schema.xml file:
::
   bin/manage build_solr_schema > parts/solr/solr/conf/schema.xml

Restart solr::
::
   bin/supervisorctl restart solr

Index the data:
::
   bin/manage build_files # creates database objects referencing project files
   bin/manage update_index



Start the webserver
===================

Finally, you’re ready to start the webserver:
::
   bin/manage runserver

Visit http://127.0.0.1:8000/ in your browser to see how it looks; you can use the admin interface via http://127.0.0.1:8000/admin (logging in with the superuser account you just created).