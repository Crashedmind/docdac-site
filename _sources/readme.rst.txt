****************************************************************************
Documentation of Code - Documentation as Code
****************************************************************************

Goals
========

#. Documentation-Of-Code: documentation that helps the user to do what they need i.e. explanation of the APIs supplemented with examples, howtos with associated diagrams.
#. Documentation-As-Code: documentation that is current i.e. all documentation and diagram source maintained in GIT/Bitbucket with the source code. In addition the toolset is maintained as code. 

Sub-goals
-----------
#. Bring documentation closer to the code so that it is in sync with the code:

    #. Code is developed consistent with the documentation
    #. Documentation diagrams and description is current and correct

#. Remove the initial friction to create diagrams - by auto-generating them
#. Diagram source text files are version-controlled in GIT in relevant source code repo
#. Consistent diagrams - that can be easily interlinked

    #. Use Themed Templates
    #. Use a common icon-set
    #. Hyper-Linkable diagrams

#. Different types of documentation integrated i.e. make it easy to combine documentation into one repo - exportable to different views.

Use-Case
-----------
Given a code base consisting of:

#. Code written in C, C++
#. Diagrams in Plantuml, Gliffy, various image formats (png, jpeg,...)
#. Documentation written in a Wiki or Markup language or Office document 

We want to 

#. To create documentation for users of the software - developers, and end-users.
#. For the documention to be pretty and modern looking
#. To support documentation extraction from C, C++ code and also standalone descriptive text files that include PlantUML diagrams.


What's Out There
================


Github
------
Github supports several markup languages for documentation; the most popular ones are Markdown (*.md) and reStructuredText (*.rst).

.. csv-table:: Markup supported by GitHub
   :header: "Markup", "Associated with / Origins", "Example Projects Using"
   :widths: 15, 15, 30
       
    "Markdown",	"Web content in general",	"Most Github projects. Static Site Generators with content in GitHub e.g. Gatsby"
    "`RST reStructured Text <http://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`_", "Python",	"Most Python projects e.g. `Scrapy <https://docs.scrapy.org/en/master/index.html>`_. The `Linux kernel <https://www.kernel.org/doc/html/v4.10/index.html>`_ as described `here <https://www.kernel.org/doc/html/v4.14/doc-guide/sphinx.html>`_"
    "POD Plain Old Documentation",	"Perl",	"Perl, OpenSSL"
    "ASCIIDoc",	"General documentation", ""
    "Doxygen",	"C, C++.",	"Most C, C++ projects"
    "Rdoc",	"Ruby", ""	


Bitbucket
---------
`Bitbucket <https://confluence.atlassian.com/bitbucket/wikis-221449748.html>`_ supports Markdown, reStructured Text, Textile and Creole.

However a plugin may be required e.g. `ReStructuredText Viewer for Bitbucket plugin <https://marketplace.atlassian.com/apps/1217687/restructuredtext-viewer-for-bitbucket?hosting=server&tab=overview>`_ to render an rst file when viewed in Bitbucket.


Confluence
-----------
Today we use Confluence which makes it easy for everyone to add content or provide comments on it. However, the content becomes stale and is disconnected from the code and software teams.

Many community projects use Confluence for user documentation e.g. Apache https://cwiki.apache.org/confluence/display/HTTPD/ which is a wiki containing user-contributed recipes, tips, and tricks for the Apache HTTP Server.

.. important:: 

   #) We work with many teams that don't have Confluence access (VFC, PCI and EMV labs, other subsiduaries/contractors). The information about the s/w should travel with the s/w.


Why Sphinx and reStructuredText
===============================

See these 2 good articles that answer this question:

#. https://varnish-cache.org/docs/trunk/phk/sphinx.html:
#. https://devblogs.microsoft.com/cppblog/clear-functional-c-documentation-with-sphinx-breathe-doxygen-cmake/

.. note ::
    The `Linux kernel <https://www.kernel.org/doc/html/v4.10/index.html>`_ uses Sphinx and ReStructuredText as described `here <https://www.kernel.org/doc/html/v4.14/doc-guide/sphinx.html>`_.
    

Overview
================

.. uml:: overview.puml
    :align: center
    :caption: *Overview of the Documentation System* 

.. note ::
    This diagram was created from PlantUML C4.

Tools
-----------

.. csv-table:: Tools Used
   :header: "Tool", "Details"
   :widths: 10, 50
    
    "`Doxygen <http://www.doxygen.nl/>`_ ",	"Doxygen is the de facto standard tool for generating documentation from annotated C++ sources,  but it also supports other popular programming languages. Runs native."
    "`Sphinx <https://build-me-the-docs-please.readthedocs.io/en/latest/Using_Sphinx/OnReStructuredText.html>`_ ",	"Sphinx is a tool that makes it easy to create intelligent and beautiful documentation. Sphinx uses reStructuredText as its markup language, and many of its strengths come from the power and straightforwardness of reStructuredText and its parsing and translating suite, the Docutils. Runs on Python."
    "`Breathe <https://breathe.readthedocs.io/en/latest/>`_ ",	"Breathe provides a bridge between the Sphinx and Doxygen documentation systems. It is an easy way to include Doxygen information in a set of documentation generated by Sphinx. Runs on Python."
    "`Exhale <https://exhale.readthedocs.io/en/latest/>`_ ",	"Automatic C++ library API documentation generator using Doxygen, Sphinx, and Breathe. Exhale revives Doxygen’s class / file hierarchies using reStructuredText for superior markup syntax / websites.  Exhale provides a layer of automation, enabling launching Doxygen and generating the full website all from your conf.py. Runs on Python."
    "`PlantUML <http://plantuml.com>`_ ",	"PlantUML creates UML and other diagrams from a simple text description. Runs on Java."




Setup Toolset
==============

#. Linux host is used.
#. Source code will reside on host - but be shared into the Docker container to process with the tools.
#. Toolset will be installed in Docker container i.e. no tools installed/needed on host (except Docker). 
#. Docker Command will be run as current user. (Docker Daemon always runs as the root user)
#. Docker Container will be run as current user i.e. not root which is the default.
#. Docker Container runs stateless and on demand.





Layout
-----------
The files layout is as per https://github.com/svenevs/exhale:

.. code-block:: 

    MyProject/
    ├── documentation # Doxygen stuff - optional - for using doxygen manually
    │   ├── html/
    │   ├── latex/
    │   └── Doxyfile
    │
    ├── docs/ #Sphinx stuff
    │   ├── api/
    │   │   └── library_root.rst
    │   │
    │   ├── conf.py # Sphinx configuration file
    │   ├── index.rst # Sphinx index file
    │   │
    │   └── doxyoutput/ 
    │       └── xml/ # Doxygen xml output - used as input to Sphinx
    │           └── index.xml
    │
    ├── include/ # source headers
    │   └── h,hpp files
    │
    └── src/ # source C, C++ files
        └── c,cpp files





Pre-Requisites
---------------
Docker is installed.

Make Docker Accessible by Current User
---------------------------------------------

.. note:: 

   #) There's 2 common ways to make Docker accessible by current user - see https://docs.docker.com/install/linux/linux-postinstall/. 

   #)  Setfacl is used as an alternative to give more fine grain control.

Allow user to read/write the Unix socket used by docker.

.. code-block:: bash

    sudo setfacl -m user:`whoami`:rw /var/run/docker.sock


Get Toolset
---------------
.. code-block:: bash

    git clone ssh://git@bitbucket.verifone.com:7999/~chris_m11/docdac.git
    cd ./docdac


Build Toolset from Dockerfile
------------------------------
.. code-block:: bash

    ./docker_build.sh 

This takes several minutes e.g. ~4 minutes in timed example below (most of which is updating the apt repos)

.. code-block:: 
    :linenos: 
    :emphasize-lines: 6


    time ./docker_build.sh 
    ...
    Successfully tagged docdac:latest
    Successfully tagged docdac:ubuntu-1.0

    real    4m12.616s
    user    0m0.350s
    sys     0m0.236s


docker_build.sh source

.. literalinclude:: ../docker_build.sh 
    :linenos: 


Dockerfile source

.. literalinclude:: ../Dockerfile
    :emphasize-lines: 1, 14-15, 19-21, 23
    :linenos: 





Ubuntu vs Alpine base
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Ubuntu:latest is used as the base (`64MB <https://ubuntu.com/blog/minimal-ubuntu-released>`_).
While Alpine base is smaller (5MB) it is not used (even as a second Dockerfile option) for these reasons:

#. Consistency with Ubuntu development environment
#. Alpine packaging and packages not compatible with Ubuntu (apk vs deb) 
#. `Poor CVE tracking <https://kubedex.com/follow-up-container-scanning-comparison>`_



Sanity Test Toolset
------------------------------
.. code-block:: 

    ./docker_test.sh 


Example Output

.. code-block:: 
    :emphasize-lines: 1-2, 4, 6

    doxygen
    1.8.13

    sphinx-build 2.2.0

    (0.000 - 502 Mo) 495 Mo - PlantUML Version 1.2019.09
    (0.004 - 502 Mo) 495 Mo - GraphicsEnvironment.isHeadless() true
    (0.004 - 502 Mo) 495 Mo - Forcing -Djava.awt.headless=true
    (0.004 - 502 Mo) 495 Mo - java.awt.headless set as true
    (0.004 - 502 Mo) 495 Mo - Forcing resource load on OpenJdk
    (0.125 - 502 Mo) 490 Mo - Found 0 files
    No diagram found

docker_test.sh source

.. literalinclude:: ../docker_test.sh 
    :linenos: 



Transpile DoxyGen Output to Sphinx and add other Documentation
======================================================================




Configure Sphinx conf.py
-----------------------------
.. note ::
    svg format is used for images because it supports embedding hyperlinks.


conf.py source

.. literalinclude:: ./conf.py
    :emphasize-lines: 26, 60, 63-64, 90-96, 99, 102
    :linenos: 


Add Source Code 
-----------------------------

include/hello-world.h source

.. literalinclude:: ../include/hello-world.h
    :linenos: 



Add index.rst
-----------------------------

index.rst source

.. literalinclude:: ./index.rst
    :linenos: 


Run Sphinx
-----------------------------

.. code-block:: 

    ./sphinx_build.sh 



sphinx_build.sh source

.. literalinclude:: ../sphinx_build.sh 
    :linenos: 




Annex: Build Source code Doxygen
==========================================
The toolset above (Exhale) builds Doxygen for us i.e. does all of below automatically. 
This section is shown here if someone wants to build DoxyGen only using toolset.

Get Source Code
-----------------------------

.. code-block:: 

    git clone ssh://git@bitbucket.verifone.com:7999/trin/trinity-vfc-deliverables.git
    cd ./trinity-vfc-deliverables


Create Doxygen Config File
-----------------------------

Run doxygen from container to create config file. Run container exes as me (current user:group) - not root

.. code-block:: 

    mkdir documentation #We do this from host so dir owner:group is host user (not root)
    docker run --user $(id -u):$(id -g) -i  -v $(pwd):/home/documentation/ -t docdac:ubuntu-1.0 doxygen -g 

Files created from container as local host user

.. code-block:: 

    ls -la documentation/
    total 116
    drwxrwxr-x 2 chris_m11 chris_m11   4096 Sep  3 15:02 .
    drwxrwxr-x 8 chris_m11 chris_m11   4096 Sep  3 15:02 ..
    -rw-r--r-- 1 chris_m11 chris_m11 108516 Sep  3 15:02 Doxyfile

Configure Doxyfile. 
Only minimal changes shown for illustration purposes.

.. code-block:: 

    PROJECT_NAME           = "Trinity VFC Deliverables"
    ...
    INPUT = ../include
    

Build Source Code Doxygen Output
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: 

    docker run -u `id -u $USER`:`id -g $USER` -i  -v $(pwd):/home/documentation/ -t docdac:ubuntu-1.0 doxygen
    
    #view pages with browser e.g.
    google-chrome documentation/html/index.html 
    google-chrome documentation/html/structvfc__keystore__key__info__t.html
