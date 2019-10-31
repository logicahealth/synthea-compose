Synthea Installer via docker-compose
+++++++++++++++++++++++++++++++++++++

Usage Instructions
-------------------

Install Docker
###############

Following the instructions here_, install Docker for the proper host system.

When the system has been installed, we recommend that the Docker process be
given at least 3GB of dedicated memory.

Start docker-compose
####################

The command to execute the docker-compose process is simple.  It will build or pull
the proper images and start the containers after the process completes.

Within this directory, execute :

.. parsed-literal::

  C:\\Users\\joe.snyder\\work\\OSEHRA\\synthea_compose> **docker-compose up --build**
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
  
Note that it may take up to 10 - 15 minutes to complete the installation.

Accessing the containers
#########################

DHP Synthea Manager
$$$$$$$$$$$$$$$$$$$

Once the scrolling has stopped, visit http://localhost:8081 to view the 
"DHP Manager" interface and begin to generate patients.  The user interface
provides the ability to set the parameters for patient records generation,
filter the generated patient list by diagnosis, visualize the individual patient
history, and send the patient record to VistA.

Please note that when sending a patient record to VistA, i.e., clicking on the
"SEND TO VISTA" link, it may take up to 10 seconds for the import process to
complete.  When the import process completes, the VistA EHR will return the
patient medical number as indicated in the VISTA ICN field.

OSEHRA Synthea Visualization
$$$$$$$$$$$$$$$$$$$$$$$$$$$$

The Synthea visualization created by OSEHRA can be reached by visiting
http://localhost:8082/synthea/synthea_upload.php

The default address for a FHIR server to query is set to the one set up by this
Docker-Compose script.  More information on that can be found below.

Accessing VistA
$$$$$$$$$$$$$$$

The container with VistA running can be accessed by a couple of different
methods.

Ports
  * 9430: Opened for connections from CPRS
    * Download of CPRS and other GUIS found here: Installer_
  * 2222: Opened for SSH connections to the container
    * information on connecting found here: `SSH Connections`_
  * 8001: Used for VistALink

FHIR on VistA
$$$$$$$$$$$$$

The Synthea system now contains both halves of the `FHIR on VistA`_
system.  The VistA installation has the KIDS build, while the ``fhir``
service contains the Hapi FHIR implementation.  To access the API,
visit ``http://localhost:8080/api/metadata``.

Further exercising of the API can be accessed after a patient is imported into
VistA.  Using the ``VistA ICN`` value, information on the patient can be reported
by accessing:

``http://localhost:8080/api/Patient/<icn>``

Shutting down
#############

To stop the process, simply hit CTL+C and the containers will attempt to quit
gracefully.  An additional CTL+C will force the containers down.  

To remove the containers, execute the following command: ``docker-compose down``

.. _here: https://docs.docker.com/install/
.. _`SSH Connections`: https://github.com/OSEHRA/docker-vista#roll-and-scroll-access-for-non-cach%C3%A9-installs
.. _Installer: https://code.osehra.org/files/clients/OSEHRA_VistA/Installer_For_All_Clients/OSEHRA_VISTA_GUI_Demo.msi
.. _`FHIR on VistA`: https://github.com/OSEHRA/FHIR-on-VistA