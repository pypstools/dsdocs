Generator trip simulation
=========================

Before current script the folllowing codes have to be placed:

* :ref:`ds-initialization`
* :ref:`ds-methods`
* :ref:`ds-folders`
* :ref:`ds-elements`


Channel definition synchronous machines
---------------------------------------

.. code:: python

    name2sym = {}
    for sym in syms:
        name2sym.update({sym.cDisplayName:sym})
        elmres.AddVars(sym, 
                        's:ve',    # p.u  Excitation Voltage
                        's:pt',    # p.u.   IN    Turbine Power
                        's:ut',    # p.u.   OUT   Terminal Voltage
                        's:ie',    # p.u.   OUT   Excitation Current
                        's:xspeed',# p.u.   OUT   Speed
                        's:xme',   # p.u.   OUT   Electrical Torque
                        's:xmt',   # p.u.   OUT   Mechanical Torque
                        's:cur1',  # p.u.   OUT   Positive-Sequence Current, Magnitude
                        's:P1',    # MW     OUT   Positive-Sequence, Active Power
                        's:Q1',    # Mvar   OUT   Positive-Sequence, Reactive Power
                        'c:firel', # deg    Rotor angle with reference to reference machine angle 
                        )

                        

Define trip event for 'G1' synchronous machine
----------------------------------------------

.. code:: python

    evt1 = events_folder.CreateObject('EvtSwitch', 'G1 trip event');  # generator switch event is created here
    evt1[0].p_target = name2sym['G1']   # event target is the second synchronous generator replace 'G1' with the particular name
    evt1[0].time = 1.0  # the event will be executed at t=1s
    

Power flow and run simulation
-----------------------------

.. code:: python
   
    ldf.Execute()  # executes power flow
    ini.Execute()  # executes initialization
    sim.Execute()  # executes simulation
    
    
Export results to file
----------------------

.. code:: python
   
    comres = app.GetFromStudyCase('ComRes'); 
    comres.iopt_csel = 0
    comres.iopt_tsel = 0
    comres.iopt_locn = 2
    comres.ciopt_head = 1
    comres.pResult=elmres
    comres.f_name = r'C:\results.txt'  # change 'C:\results.txt' with your path
    comres.iopt_exp=4
    comres.Execute()


    
Clear events an reset calculations 
----------------------------------

.. code:: python
      
    events_folder.Delete()
    app.ResetCalculation()

    
