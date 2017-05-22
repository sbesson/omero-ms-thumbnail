OMERO Thumbnail Microservice
============================

OMERO thumbnail microservice server endpoint for OMERO.web.

Requirements
============

* OMERO 5.2.x+
* OMERO.web 5.2.x+
* Redis backed sessions
* Java 8+

Workflow
========

The microservice server endpoint for OMERO.web relies on the following workflow:

1. Setup of OMERO.web to use database or Redis backed sessions

1. Running the microservice endpoint for OMERO.web

1. Redirecting your OMERO.web installation to use the microservice endpoint

Development Installation
========================

1. Clone the repository::

        git clone git@github.com:glencoesoftware/omero-thumbnail.git

1. Run the Gradle build and utilize the artifacts as requried:

        ./gradlew installDist
        cd build/install
        ...


Configuring and Running the Server
==================================

The thumbnail microservice server endpoint piggybacks on the OMERO.web Django
session.  As such it is essential that as a prerequisite to running the
server that your OMERO.web instance be configured to use Redis backed sessions.
Filesystem backed sessions **are not** supported.

1. Start the server::

        ./omero-thumbnail --redis-password redispw --debug

Redirecting OMERO.web to the Server
===================================

What follows are two snippets which can be placed in your nginx configuration
for OMERO.web to redirect searches to the search server endpoint::

    upstream thumbnail-backend {
        server 127.0.0.1:8080 fail_timeout=0 max_fails=0;
    }

    ...

    location /webclient/render_thumbnail/
        proxy_pass http://thumbnail-backend/;
    }

Running Tests
=============

Using Gradle run the unit tests:

    ./gradlew test

Reference
=========
