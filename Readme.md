# Calibration and noise estimation of ICM 42605 IMU

All readings are taken at a sample frequency of 50Hz, and the data was collected for 30 mins. <br>

App Used to collect data - Sensor Logger (developed by Kelvin Tsz Hei Choi) <br>

This app records the data sensed by the accelerometer and gyroscope sensors in built in the phone and outputs the data in a CSV or JSON file. 

## Sensor Details

Accelerometer sensor: icm4x6xx Accelerometer Non-wakeup (vendor – TDK-Invensense) <br>
Gyroscope sensor: icm4x6xx Gyroscope Non-wakeup (vendor – TDK-Invensense) <br>

Both the accelerometer and gyroscope sensors are inbuilt within a single IMU chip. 

## Datasheet

Sensor ID – ICM 42605/ The datasheet for the sensor is present in the repository. Please refer to it for the accelerometer and gyroscope specifications.

## Experimental Setup

The android phone was placed on a flat wooden table on top of a dry cloth. The inclination of the phone with respect to the table was ensured to be zero. The phone was left undisturbed until a specified time (30 mins) to record the sensor data. 

## Calibration

All data were collected at room temperature of 24 degree Celcius (75F). The phone was left undisturbed for the whole time while recording the sensor data without any influence from external sources. The sensors (accelerometer and gyroscope) were made to run while the phone was at rest. The sensors were made to collect data for three to four 15-minute period before proceeding to the collection of actual experimental data. This was done for the sensor to read values closer to 9.81 when measuring the acceleration due to gravity in the Z direction. The phone was positioned such that the X and Y axis lie along the plane of the table and the Z axis is in the vertical direction. 

## Assumptions

*	No electromagnetic interference 
*	The data was recorded at room temperatures 
*	No humidity in the air
*	No disturbances due to nearby vibrations
*	Sensor is sensitive enough to reflect even the slightest changes 
*	Recorded data lies within the frequency range of the sensors

## Sensor Equations

#### Accelerometer

`a<sub>t</sub> = a<sub>l</sub> + a<sub>g</sub> + noise` <br>

a<sub>l</sub> - Linear acceleration (Zero) <br>
a<sub>g</sub> - Acceleration due to graviy (Only in z-dirn) <br>

*	Assume ax and ay are 0 m/s<sup>2</sup> because the phone is at rest without any motion.<br> 
  No linear acceleration. <br>
  Only a<sub>z</sub> must read 9.81 m/s<sup>2<s/up> due acceleration due to gravity.
*	Subtracting 9.81m/s<sup>2</sup> from the obtained a<sub>z</sub> values would give the noise in the z dirn. 

#### Gyroscope

`_w<sup>'</sup>_ = _w_ + bias + _n_` <br>

_w<sup>'</sup>_ - actual value <br>
_w_ = true value <br>
_n_ - noise <br>

*	True value must be 0 since the phone (gyroscope sensor) does not undergo any change in orientation.
*	The gyroscope data obtained from experiment must be equal to the sum of bias and noise.
*	Assuming bias to be the mean of the recorded data (as per lecture slides) and subtracting this mean from all readings would fetch us the noise (as per lecture slides).
*	Taking RMS of the noise would give us the magnitude of average noise in each direction.
*	Bias is assumed to be constant and doesn’t alter with changes in the environment.

#### Units

Accelerometer – All readings are in m/s<sup>2</sup> <br>
Gyroscope – All readings are in rad/s

## Calculation Procedure

#### Accelerometer:

1.	Subtracted 9.81m/s<sup>2</sup> from the readings in the z direction and the found the corresponding noise.
2.	All readings in x and y direction directly gives us the noise values.
3.	Using least squares method <br>
                   `y = x + _v_ `, <br> where y is the measured value, x is the true value and _v_ is the error.
4.	x is 9.81 m/s<sup>2</sup> while calculating noise in the z direction and 0 while calculating in the x and y directions. 
5.	Found the RMS (or normal average) of the noise values.
6.	Performed unit conversion and proceeded with the comparison of the values.

#### Gyroscope:

1.	Found the mean of the x, y and z readings individually – BIAS
2.	Calculated the RMS (or normal average) bias values and moved on to the unit conversion for further comparison.
3.	Subtracted the bias value from each of the readings in the corresponding direction and got the noise values using least squares method.
4.	Found the RMS of the noise values in each direction and did the unit conversion.

## Why the theoretical and experimental bias and noise values different?

The frequency (100Hz) used by the manufacturer during the testing was different than the one I used (50Hz). The time period of testing employed by the manufacturer to arrive at the values might be different than the ones employed for this experiment. 

The values obtained from experiment and the data sheet vary by a certain extent due to a lot of external uncontrollable variables. Environmental parameters such as temperature, humidity, electromagnetic interferences cannot be hindered or maintained constant. Improper calibration also led to errored data. In addition, phone sensors are cheap and cannot be expected to provide accurate and precise readings, thus going with a better sensor would fetch us more reliable values. Presence of innate instrumental errors in the phone sensor. Possibility of drift present in the sensor, which could deviate the succeeding readings. Bias could have varied as the experiment was performed as the temperature could not be maintained a constant. 

## What could have been done better?

- Instead of going for Gaussian based approach, a probabilistic (Bayesian) approach could have been a bit more reliable. 
- Bias could have been found more accurately and precisely using Allan variance method.
- Could have used either a piezoresistive or piezoelectric accelerometer instead of a MEM based sensor.
- Could have used a band pass filter/ Kalman filter to remove the noise component. (For BIAS calculation)
- Could have recorded the data for a longer period.
- Could have created a more ideal testing condition. 

###### A readme.docx file is attached to the repository which contains information in detail.
