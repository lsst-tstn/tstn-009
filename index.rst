..
  Technote content.

  See https://developer.lsst.io/restructuredtext/style.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-technote-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

.. TODO: Delete the note below before merging new content to the master branch.

.. note::

   **This technote is not yet published.**

   Testing

.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).

Control PC
==========
Cabinets of the coater and the PLC (programmable logic controller) are connected to the Control PC. Along with the washing station, main plc of the coater and main plc of the pressure control of the coater. Lifting station and buggy also has a plc but not connected to the Control PC. The most impoartant ones are the coater and the washing machine. These are the ones the Wicon (Windows Control) communicates with. You can access all of them through the TIA Portal V14, lets open this program to get an overview of the system.

Open the path *D:/PLC/Archive*, this contains all the coating archives.

Open the washing machine project.

When you open the project there will be many folders that appear. However the only useful ones are the "Devices & Network" and the project itself that is named closley after the folder you open.

Open "devices & Networks" Click the small green light that gives you the property of the ethernet. Under "general" you can see the IP of that device.

IP Table D:SKYVA_AdressList.xlsm - In here you can see the IP's for the different projects. We can find all the ip address of all the waching machine by the acronym of WM

If you want to change the IP address you need to do some trickery specific to that part and is recommended we consult with an expert. 

A neat feature on this windows is to "Go Online" on the top of the window. When we do this a windows pops up and we need to select the device that we want to go online.  

Select PN (profinet) through the dropdown windows using the network card connected to the coating machines. You can then do "start search" and it will populate the middle table with devices it found on that netwrork.

We find that device then we can attempt to "Go online". This will tell us the staus of the clients.

When viewing the device we can double click to see how that device is connected to other devices. On the "Device Overview" window we can see a list of input and output devices of that component. We can see an I for input and Q for output. The I address will tells you IP of the internal address. 

What we can do with the input output addresses is expand the "PLC tags" folder in the project folder and view a list of tags that will tell you what that address is for. This tells you what it does for the information that is passed on that address. This can be useful when running diagnostics. You can right click this value and select "show crossreferences" to give you more information about that input. You can then trace to see where it is used. 

Networks
========
There are two network cards connected. One to the lsst network the other is connected to the coating machines.

Whenever asked by Siemanns which card to use you will always use the connection to the coating machines.


Programming in SCL
==================
You use functions and data. Using Python as a reference, functions are similiar to methods and data is similiar to objects without methods. This data is also what can be used as an interface of information from Wicon to PLC. 

To compile the program go to Main OB1, this is the main project. All input starts at the OB1. 

If you double click the OB1 you can view all the functions that cascade from it. The interface ends where it passes information from the ethernet.

In the datablock we sometimes store some information from the PLC so that Wicon can consume it. 

Offsets are the offsets of the data location within the block. This is important to know because any consumers of the data need to know the offsets. Generally you use scripts to do this cleanly.

You can create your own data types and use ones alrewady included. If you want to see the members of a data type you can rightand "go to definition"

WICON
=====
There are two parts. The first is the visualization. The second is the software PLCc. The software PLC is in a small tringular blue within square box at the bottom right corner of the winfow. This window is not too interesting. Just has some cycle times and proccesses.

Everything that is related to Wicon is inside the *d:\wicon32* directory then inside we can go to *run\coated*. Here we see a control part and a visulization part. Inside the control folder we find all related configuration files.

An item of interest are the configuration scripts (cs). Here we will find 5 configuration scripts to various parts of the controls.

We start with the S7 link. In this file the interface to the plc's are configurated. Edit this file with notepad++. In this file we can see 5 plc's. The Dummy is not of interest for us. In these files you can see an IP address to connect to the PLC and reads in the data blocks.

All values of the datablock are here with the offset mentioned in Programming in SCL. There are scripts that generate these data definitions with thier addresses and offsets.

There is a configuration file for the control of WICON. Every value you see in the visualisation you can find in the control.cs. You can search for the number value on the visualization tool and search for it in the .cs file. 

System Tray
===========
You can find other PLC monitors. you can view these. When you double click on these you can see all the data that they are monitoring. 

Errors & Traceback
==================
First way to do this is in the error window. The error history windows will show various information about the error. 

The second way is with the many small papers looking icon on the top left. It shows the last 100 lines of messages. These can show you information about the system. 

The third way is to go to *d:/wicon32/wun_coater/visualization/LogFiles*. Here we see 4 types of logfiles

First is the output. These are the messages that are written in the many small paper looking icon. There are up to 10 of these files. Number 10 is the oldest, 1 is the newest. 

Second is states. These are everything that is operating, it shows every state change. Here you can find mouse actions and anything the user is doing with the mouse. At the moment not used too much. 

Third is communication, not too much of interest because everything written here is for communcations between the visualization part and the controller part. 

Fourth is technology log. To automate the coding inside the visualization there are some scripts running in the background and every line which is executed from these scripts is recorded into the technology log. 

Within the postgresSQL database we can querry any value change. To do this you can use ProcessDBquerry. 

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
