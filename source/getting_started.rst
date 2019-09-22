.. dsdocs documentation master file, created by
   sphinx-quickstart on Thu Nov 22 11:22:36 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Getting started
===============

.. _ds-initialization:

DigSILENT Initialization
------------------------

Take into account that all scripts depicted hereinafter use
the dynamic compiled module powerfactory.pyd.
Therefore it should be used the python version and dependencies required by powerfactory.pyd.

For more information see PowerFactory user manual.

.. code:: python
   

    import sys
    # Go to the powerfactory.pyd directory to import the powerfactory.pyd module
    path = r"C:\PF15_1_7"               # PowerFactory installation absolute directory
    sys.path.append(path + r"\python")  # Directory of the .dll and .pyd files
    
    import powerfactory

    # start PowerFactory  in engine  mode
    user_name = 'user_name'         # Specify your PowerFactory user name if you have,
                                    # otherwise use user_name = ''
    password = 'password'           # Specify PowerFactory user name password if you have,
                                    # otherwise use pasword = ''
    app = powerfactory.GetApplication(user_name, password)

    # activate project
    project = app.ActivateProject("name_of_project")  # change by the name of your project
    prj = app.GetActiveProject()    # active project instance

.. _ds-methods:

Get application methods
-----------------------

.. code:: python

    ldf = app.GetFromStudyCase("ComLdf")    # Load flow
    ini = app.GetFromStudyCase('ComInc')    # Dynamic initialization
    sim = app.GetFromStudyCase('ComSim')    # Transient simulations
 
 
.. _ds-folders:

Get folders
-----------

.. code:: python

    elmres = app.GetFromStudyCase('Results.ElmRes')  # Results folder ??
    events_folder = app.GetFromStudyCase('IntEvt');  # to get events folder

.. _ds-elements:

Get elements
------------

.. code:: python

    buses = app.GetCalcRelevantObjects("*.ElmTerm")  # Buses
    syms = app.GetCalcRelevantObjects("*.ElmSym")    # Synchronous machines
    loads = app.GetCalcRelevantObjects("*.ElmLod")   # Loads
    dsls = app.GetCalcRelevantObjects("*.ElmDsl")    # DSL models



Channel definition for buses
----------------------------

.. code:: python

    for bus in buses:
        elmres.AddVars(bus,'m:u','m:phiu','m:fehz')  # creating channels for:
                                                     # voltage ('m:u'),  angle ('m:phi') and frequency ('m:fehz') 





Channel definition for loads
----------------------------

.. code:: python

    for load in loads:                        # creating channels for:
        elmres.AddVars(load, 'n:u1:bus1',     # voltage ('m:u')
                              'm:I1:bus1',     # current ('s:P1')
                              'm:Psum:bus1',   # active power ('s:P1')
                              'm:Qsum:bus1')   # reactive power ('s:Q1') 


                              
                              
                              
Power flow
----------

.. code:: python
   
    ldf.Execute()  # executes power flow



Run time simulation
-------------------

.. code:: python

    ini.Execute()  # executes initialization
    sim.Execute()  # executes simulation




Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
