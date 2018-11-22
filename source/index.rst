.. dsdocs documentation master file, created by
   sphinx-quickstart on Thu Nov 22 11:22:36 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to dsdocs's documentation!
==================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:


DigSILENT Initialization
------------------------

.. code:: python
   

    import sys
    sys.path.append(r"C:\PF15_1_7\python")
    import powerfactory

    # start PowerFactory  in engine  mode
    app = powerfactory.GetApplication('user_name','password') # change 'user_name' by your user name and 'password' by your password

    # activate project
    project = app.ActivateProject("name_of_project")  # change by the name of your project
    prj = app.GetActiveProject()    # active project instance


Name application methods
------------------------

.. code:: python

    ldf = app.GetFromStudyCase("ComLdf")    # Load flow
    ini = app.GetFromStudyCase('ComInc')    # Dynamic initialization
    sim = app.GetFromStudyCase('ComSim')    # Transient simulations



Name elements
-------------

.. code:: python

    buses = app.GetCalcRelevantObjects("*.ElmTerm")  # Buses
    syms = app.GetCalcRelevantObjects("*.ElmSym")    # Synchronous machines
    loads = app.GetCalcRelevantObjects("*.ElmLod")   # Loads
    dsls = app.GetCalcRelevantObjects("*.ElmDsl")    # DSL models



Chanel definition for buses
---------------------------

.. code:: python

    for bus in buses:
        elmres.AddVars(bus,'m:u','m:phiu','m:fehz')  # creating channels for:
                                                     # voltage ('m:u'),  angle ('m:phi') and frequency ('m:fehz') 



Power flow
----------

.. code:: python
   
    ldf.Execute()  # executes power flow




Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
