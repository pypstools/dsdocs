Reference
=========


Generate a dictionary to help buses
-----------------------------------

.. code:: python

    buses = app.GetCalcRelevantObjects("*.ElmTerm")
    name2bus = {}
    for bus in buses:
        name2bus.update({bus.cDisplayName:bus})  


Generate a dictionary to help with synchronous machines
-------------------------------------------------------

.. code:: python

    syms = app.GetCalcRelevantObjects("*.ElmSym")
    name2sym = {}
    for sym in syms:
        name2sym.update({sym.cDisplayName:sym})



Generate a dictionary to help with DSL models
---------------------------------------------

.. code:: python

    dsls = app.GetCalcRelevantObjects("*.ElmDsl")
    name2dsl = {}
    for dsl in dsls:
        parent = dsl.GetParent()
        parent_name = parent.cDisplayName            # parent name i.e. plant name
        dsl_name = dsl.cDisplayName
        if not parent_name in name2dsl:
            name2dsl.update({parent_name:{}})
        name2dsl[parent_name].update({dsl_name:dsl})



Change synchronous generator AVR voltage reference
--------------------------------------------------

.. code:: python

    evt_gen = events_folder.CreateObject('EvtParam', 'GEN event');  # generator parameter event is created here
    evt_gen[0].p_target = name2dsl['plant_name']['SEXS']   #  replace 'plant_name' with the name of the plant, 
                                                           #  and 'SEXS' with the name of the AVR type
    evt_gen[0].variable = 'usetp'  # variable to modify
    evt_gen[0].value = str(1.00)   # new voltage reference value
    evt_gen[0].time = 1.0          # time for the event
    evt_gen[0].Execute()           
                            

Define a trip event for synchronous machine
-------------------------------------------

.. code:: python

    evt1 = events_folder.CreateObject('EvtSwitch', 'G1 trip event');  # generator switch event is created here
    evt1[0].p_target = name2sym['G1']   # event target is the second synchronous generator replace 
                                        # 'G1' with the particular name
    evt1[0].time = 1.0  # the event will be executed at t=1s
    


Define a short circuit
----------------------

.. code:: python

    evt1 = events_folder.CreateObject('EvtShc', 'short circuit');  # generator switch event is created here
    evt1[0].p_target =  name2bus['name_of_bus']   # event target is bus 'name_of_bus', change with the particular bus
    evt1[0].time = 1.0 # short circuit starts at t=1 s
    evt1[0].i_shc = 0
    evt1[0].Execute()


