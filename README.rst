Synthea Installer via docker-compose
+++++++++++++++++++++++++++++++++++++

Usage Instructions
-------------------

Install Docker
###############

Following the instructions here_, install Docker for the proper host system.

Start docker-compose
####################

The command to execute the docker-compose process is simple.  It will build or pull
the proper images and start the containers after the process completes.

Within this directory, execute :

.. parsed-literal::

  C:\Users\joe.snyder\work\OSEHRA\synthea_compose> **docker-compose up --build**
  Building synservice
  Step 1/9 : FROM robcaruso/synthea:1
   ---> e764ee42c084
  Step 2/9 : ARG env
   ---> Using cache
   ---> 58304a8e751d
  Step 3/9 : RUN apt-get install -y gradle
   ---> Using cache
   ---> 4943f72c29ba
  Step 4/9 : ENV profile=${env}
   ---> Using cache
   ---> ac650a5c1cee
  Step 5/9 : WORKDIR /service
   ---> Using cache
   ---> 0a8c0bfc9884
  Step 6/9 : COPY . /service
   ---> a33d5759b3cb
   <...>
   
The screen will scroll as the containers prepare themselves for execution.
The final message before the containers are ready is: 

.. parsed-literal::
  synservice_1  | 2019-06-12 15:43:33.762  INFO 107 --- [  restartedMain] c.d.d.d.DhpSyntheaServiceApplication     : Started DhpSyntheaServiceApplication in 10.58 seconds (JVM running for 11.231

View in browser
###############

Once the scrolling has stopped, visit http://localhost:8081 to view the 
"DHP Manager" interface and begin to generate patients.

Shutting down
#############

To stop the process, simply hit CTL+C and the containers will attempt to quit
gracefully.  An additional CTL+C will force the containers down.  

We recommend that ``docker-compose rm`` is run to remove the containers to
prevent a known issues with the Synthetic Visualization Docker setup.  See
below for further explanation.

Known Issues
------------

Due to how the HTTP server is started in the Synthea Visualization container,
the container will not start correctly after being stopped.  This, in turn, 
causes the DHP Manager container to not be brought up properly.  

.. _here: https://docs.docker.com/install/