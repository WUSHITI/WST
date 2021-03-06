.. _rc-pwm-doc:

================================================================================
RC PWM input
================================================================================

.. contents::
   :depth: 1
   :local:
   
You can control the ODrive directly from a hobby RC receiver.

Any of the numerical parameters that are writable from the ODrive Tool can be hooked up to a PWM input. 
The :ref:`Pinout <pinout-chart>` tells you which pins are PWM input capable. As an example, we'll configure GPIO4 to control the angle of axis 0. 
We want the axis to move within a range of -2 to 2 turns.

#. Make sure you're able control the axis 0 angle by writing to :code:`odrv0.axis0.controller.input_pos`. 
   If you need help with this follow the :ref:`getting started guide <getting-started>`.
#. If you want to control your ODrive with the PWM input without using anything else to activate the ODrive, you can configure the ODrive such that axis 0 automatically goes operational at startup. 
   See :ref:`here <commands-startup-procedure>` for more information.
#. In ODrive Tool, configure the PWM input mapping

    .. code::  iPython
            
        odrv0.config.gpio4_mode = GPIO_MODE_PWM
        odrv0.config.gpio4_pwm_mapping.min = -2
        odrv0.config.gpio4_pwm_mapping.max = 2
        odrv0.config.gpio4_pwm_mapping.endpoint = odrv0.axis0.controller._input_pos_property

   .. note:: 
       
       you can disable the input by setting :code:`odrv0.config.gpio4_pwm_mapping.endpoint = None`

#. Save the configuration and reboot

    .. code:: iPython
            
        odrv0.save_configuration()
        odrv0.reboot()

#. With the ODrive powered off, connect the RC receiver ground to the ODrive's GND and one of the RC receiver signals to GPIO4. 
   You may try to power the receiver from the ODrive's 5V supply if it doesn't draw too much power. Power up the the RC transmitter. 
   You should now be able to control axis 0 from one of the RC sticks.

Be sure to setup the Failsafe feature on your RC Receiver so that if connection is lost between the remote and the receiver, the receiver outputs 0 for the velocity setpoint of both axes (or whatever is safest for your configuration). 
Also note that if the receiver turns off (loss of power, etc) or if the signal from the receiver to the ODrive is lost (wire comes unplugged, etc), the ODrive will continue the last commanded velocity setpoint. 
There is currently no timeout function in the ODrive for PWM inputs.