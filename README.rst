BicycleDAQ
==========

This a simple graphical user interface used to collect data on the `Davis
Instrumented Bicycle`_. The software connects to both a National Instruments
USB-6218 and a VectorNav VN-100 development board through USB connections in a
laptop PC. It makes use of the Matlab Data Aquistion Toolbox, NIDAQmx driver,
and the Matlab Serial interface. It is intended to run on a netbook computer,
in particular the ASUS Eee PC 1001P-MU17-WT.

.. _Davis Instrumented Bicycle: http://moorepants.github.io/dissertation/davisbicycle.html

Dependencies
============

These dependencies are not necessarily that strict but were the only ones the
software was tested with.

Hardware
--------

- VectorNav VN-100 http://www.vectornav.com
- NI USB-6218

Software
--------

- Windows XP Pro
- Matlab (7.8.0 R2009a) + Data Aquisition Toolbox
- VectorNav Matlab Library http://www.vectornav.com/downloads

Usage
=====

The GUI can be run by simply starting the Matlab interpreter and calling the
main script::

   >> BicycleDAQ.m

Various meta data and recording settings for the trial can be specified in the
various input cells. To initialize the system and connect to the two hardware
devices, press the "Connect" button. Once the system is connected, the "Tare"
button will tare the VN-100. When ready to start a trial press the "Record"
button and the system will wait for the handlebar button trigger to be pressed.
Once the trigger is pressed data will be collected for the specified duration
and automatically saved to disk. After the recording, the time series traces
can be inspected in the graph window by selecting the different graph type
buttons. Once recording is completed, the "Disconnect" button will disconnect
from the hardware.

When not recording any previously recorded ``.mat`` file can be loaded for
graphical inspection. The meta data can also be edited and overwritten with the
"Save" button.

Data
====

The main GUI program collects data during the trials and the various scripts in
the ``tools/`` directory collect data for sensor calibration purposes.

Trials
------

After a trial is collected via the GUI interface, the resulting ``.mat`` file
is stored in the `data/` directory with a filename ``XXXXX.mat`` where
``XXXXX`` is a unique 5 digit number for that trial. Each ``.mat`` file
contains several variables described in the following sections.

``InputPairs``, 1 x 1 structure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This structure contains a mapping from the name of the input signal to the pin
number on the NI-USB 6008.

#. ``PushButton``: 0
#. ``SteerPotentiometer``: 1
#. ``HipPotentiometer``: 2
#. ``LeanPotentiometer``: 3
#. ``TwistPotentiometer``: 4
#. ``SteerRateGyro``: 5
#. ``WheelSpeedMotor``: 6
#. ``FrameAccelX``: 7
#. ``FrameAccelY``: 8
#. ``FrameAccelZ``: 9
#. ``SteerTorqueSensor``: 10
#. ``SeatpostBridge1``: 11
#. ``SeatpostBridge2``: 12
#. ``SeatpostBridge3``: 13
#. ``SeatpostBridge4``: 14
#. ``SeatpostBridge5``: 15
#. ``SeatpostBridge6``: 16
#. ``RightFootBridge1``: 17
#. ``RightFootBridge2``: 18
#. ``LeftFootBridge1``: 19
#. ``LeftFootBridge2``: 20
#. ``PullForceBridge``: 21
#. ``ThreeVolts``: 22
#. ``FiveVolts``: 23
#. ``RollPotentiometer``: 24

NIData, matrix
~~~~~~~~~~~~~~

This matrix contains the data collected from the NI USB-6008 where the columns
correspond to the signals and the rows to the sample at a paritcular time. The
size of the matrix is (par.NINumSamples, length(InputPairs)).

par, structure
~~~~~~~~~~~~~~

``AccelerometerCompensation``, string
    This is the raw string from the VN-100 that gives the programable
    compensation parameters for the accelerometers.
``AccelerometerGain``, string
    This is the raw string from the VN-100 that gives the programable
    gain for the accelerometers.
``ADOT``, integer
    Asynchornous Data Output Type. This tells you what the asynchronous
    output is of the VectorNav. Right now it can either be 14 or 253. 14
    is the filtered data and 253 is the unfiltered. Refer to the VN-100
    documentation.
``Baudrate``, integer
    This is the baud rate at which the VN-100 is connected at.
``Bicycle``, string
    The gives the bicycle name and/or configuration.

    -  Rigid Rider : The instrumented bicycle with the rigid body cast.

``DateTime``, string
    The date and time of data collection. Formatted as 'DD-Month-YYYY
    HH:MM:SS'.
``Duration``, float
    The duration of the run in seconds.
``Environment``, string
    This is the location, building and/or equipment where the data was
    taken. Options include: Pavilion Floor, Laboratory, Hull Treadmill
``FilterActiveTuningParameters``, string
    This is the raw string from the VN-100 that gives the programable
    active tuning parameters for the Kalman filter.
``FilterTuningParameters``, string
    This is the raw string from the VN-100 that gives the programable
    Kalman filter tuning parameters.
``FirmwareVersion``, string
    This is the raw string from the VN-100 displaying the device's
    firmware version.
``HardSoftIronParameters``, string
    This is the raw string from the VN-100 that gives the programable
    hard/soft iron compensation parameters.
``HardwareRevision``, string
    This is the raw string from the VN-100 displaying the device's
    hardware version.
``Maneuver``, string
    The particular maneuver being performed.

    -  System Test : This is a generic label for data collected during
       various system tests.
    -  Balance : The rider is instructed to simply balance the bicycle
       and keep a relatively straight heading. The rider should look
       into the distance and not focus on any close objects.
    -  Balance With Disturbance : Same as 'Balance' except that a
       lateral force disturbance is applied to the seat of the bicycle.
    -  Tracking Straight Line : The rider is instructed to focus on a
       straight line that is on the ground and attempt to keep the edge
       of the front wheel aligned with the line. The line of site from
       the rider's eyes to the the line on the ground should be tangent
       to the front of the front wheel.
    -  Tracking Straight Line With Disturbance : Same as "Tracking
       Straight Line" except that a lateral perturbation force is
       applied to the seat of the bicycle.
    -  Lane Change : The rider is instructed to perform a lane change
       trying to keep the bicycle on a line on the ground. For the
       Pavillion Floor, the line is taped on the ground and the rider is
       instructed to do whatever feels best to stay on the line. They
       can use full preview looking ahead, focus on the front wheel and
       line, or a combination of both.
    -  Steer Dynamics Test : These are for the experiments setup to
       determine the friction in the steering column bearings.

``ModelNumber``, string
    This is the raw string from the VN-100 displaying the device's model
    number.
``NISampleRate``, integer
    The sample rate in hertz of the NI USB-6218
``NINumSamples``, integer
    The number of samples taken during the run on the NI USB-6218
``Notes``, string
    Notes about the run.
``ReferenceFrameRotation``, string
    This is the raw string from the VN-100 that gives the programable
    direction cosine matrix.
``Rider``, string
    This gives the first name of the person riding the bicycle or 'None'
    if noone is on the bicycle while the data was taken.
``RunID``, integer
    The unique five digit number for the run.
``SerialNumber``, string
    This is the raw string from the VN-100 displaying the device's
    serial number.
``Speed``, float
    The desired speed of the bicycle. This is slighty redundant, the
    rear wheel speed motor voltage should be used for the actual speed.
``VNavComPort``, string
    The Windows communications port that the VN-100 is connected to. The
    is typically COM3 but could be others.
``VNavSampleRate``, integer
    The sample rate in hertz of the NI USB-6218
``VNavNumSamples``, integer
    The number of samples taken for the run on the VN-100
``Wait``, float
    This is the time in seconds that the software waits for the rider to
    press the collect data trigger. If the rider doesn't push the button
    before this time, the software crashes.

VNavCols, cell array
~~~~~~~~~~~~~~~~~~~~

This cell array contains the ordered names of the data signals collected
from the VN-100. These depend on what par.ADOT is set to.

par.ADOT = 253

#. Mag X
#. Mag Y
#. Mag Z
#. Acceleration X
#. Acceleration Y
#. Acceleration Z
#. Angular Rate X
#. Angular Rate Y
#. Angular Rate Z
#. Temperature

par.ADOT = 14

#. Angular Rotation Z
#. Angular Rotation Y
#. Angular Rotation X
#. Mag X
#. Mag Y
#. Mag Z
#. Acceleration X
#. Acceleration Y
#. Acceleration Z
#. Angular Rate X
#. Angular Rate Y
#. Angular Rate Z

VNavData, matrix
~~~~~~~~~~~~~~~~

This matrix contains the data collected from the VN-100 where the
columns correspond to the signals and the rows to the sample at a
paritcular time. The size of the matrix is (par.VNavNumSamples,
length(VNavCols)). This data has an NaN value for any corrupt lines from
VNavDataText.

VNavDataText, cell array
~~~~~~~~~~~~~~~~~~~~~~~~

This cell array contains the actual text output from the VN-100 for each
sample. Some lines are corrupted.

- ``par``: A structure which contains key value pairs of the primary meta data
  for the trial.

   - ``Rider``: A char specifying the rider of the bicycle during the trial,
     e.g. ``'Elliot'``.
   - ``Speed``: A 1 x 1 double specifying the desired nominal speed during the
     trial, e.g ``7.4000``
   - ``Bicycle``: A char specifying the bicycle configuration used during the
     trial, e.g. ``'Rigid Rider'``.
   - ``Maneuver``: A char specifying the performed maneuver during the trial,
     e.g. ``'Slalom Test'``.
   - ``Notes``: A char with any notes recording by the experimentalist during
     the trial, e.g. ``'cs(23f),wind northwest'``.
   - ``Environment``: A char specifying the environment the trial was performed
     in, e.g. ``'Horse Treadmill'``.
   - ``RunID``: A 1 x 1 double representing the unique integer identification
     number of the trial, e.g. ``759``.
   - ``Duration``: A 1 x 1 double giving the total duration of the trial in
     seconds, e.g. ``20``.
   - ``NISampleRate``: A 1 x 1 double giving the sample rate in Hertz of the NI
     USB-6218 time series data collected during the trial, e.g. ``200``.
   - ``VNavSampleRate``: A 1 x 1 double giving the sample rate in Hertz of the
     VN-100 time series data collected during the trial, e.g. ``200``.
   - ``VNavComPort``: A char specifying the Windows com port the VN-100 was
     connected to during the trial, e.g. ``'COM3'``.
   - ``Wait``: A 1 x 1 double specifying the value of the NI USB-6218 trigger
     wait time in seconds, e.g. ``600``.
   - ``BaudRate``: 921600
   - VNavNumSamples: 4000
   - NINumSamples: 4000
   - ADOT: 253
   - ModelNumber: '$VNRRG,01,VN-100_v4*47'
   - HardwareRevision: '$VNRRG,02,4*69'
   - SerialNumber: '$VNRRG,03,066F00383733335843124251*2E'
   - FirmwareVersion: '$VNRRG,04,10*5A'
   - ReferenceFrameRotation: '$VNWRG,26,+1.000000E+00,+0.000000E+00,+0.000000E+00,+0.000000E+00,+0.000000E+00,+1.000000E+00,+0.000000E+00,-1.000000E+00,+0.000000E+00*02'
   - FilterTuningParameters: '$VNWRG,22,+1.000000E-08,+1.000000E-08,+1.000000E-08,+1.000000E-08,+1.000000E-01,+1.000000E-01,+1.000000E+01,+1.000000E-05,+1.000000E-05,+1.000000E-05*74'
   - HardSoftIronParameters: '$VNRRG,23,+1.000000E+00,+0.000000E+00,+0.000000E+00,+0.000000E+00,+1.000000E+00,+0.000000E+00,+0.000000E+00,+0.000000E+00,+1.000000E+00,+0.000000E+00,+0.000000E+00,+0.0000...'
   - FilterActiveTuningParameters: '$VNWRG,24,+0.000000E+00,+1.000000E+00,+9.900000E-01,+9.900000E-01*71'
   - AccelerometerCompensation: '$VNRRG,25,+1.000000E+00,+0.000000E+00,+0.000000E+00,+0.000000E+00,+1.000000E+00,+0.000000E+00,+0.000000E+00,+0.000000E+00,+1.000000E+00,+0.000000E+00,+0.000000E+00,+0.0000...'
   - AccelerometerGain: '$VNRRG,28,0*65'
   - DateTime: '21-Feb-2012 11:48:46'

- ``InputPairs``: A structure which contains key value pairs that map chars
  representing column headings for the ``NIData`` matrix to integer values
  which correspond to a column indice in ``NIData``.
- ``NIData``: An N x M matrix of doubles containing the time histories of the
  signals collected by the NI USB-6218 DAQ box. N is the number of samples and
  M is the number of signals.
- ``VNavCols``: A 10 x 1 cell array of chars that represent the column headings
  of the time series in ``VNavData``, in corresponding order to the column
  indices.
- ``VNavData``: An N x 10 matrix of doubles containing the time histories of
  the signals collected by the VN-100. N is the number of samples and the
  VN-110 reports 10 signals. This is a lightly processed version of
  ``VNavDataText``.
- ``VNavDataText``: An N x 1 cell array of chars which contain the RAW ASCII
  strings output by the VN-100 at each of the N samples.

TODO
====

- Add in the LED light
- Display the data realtime
- Scaled data graphs
- If the trigger doesn't get pushed then have it break out of record mode
- Needs a cancel record function/button
- If you press connect, then record, then disconnect and try to close the gui,
  matlab crashes
- When you load a data run, edit it and save, the runlist.txt doesn't get
  updated.
- Do you want to add or append the note after the run popup dialog box
- Make the appendeddata.mat file update based on the contents of runlist.txt,
  that way non used terms get dropped
- Have some kind of check for battery levels.
- Maybe have the notes reset after every run, because many times the notes get
  added and stay on for several runs before the operator notices. The ability
  to easily add notes after the end of the run would be helpful too.
