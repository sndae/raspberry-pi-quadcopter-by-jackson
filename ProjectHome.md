the raspberry pi seemed like such a cool little hacking broad when it first lunched and i was able to get my hands on one recently and i am now trying to pretty much create my own auto pilot if you will i making the program from scratch and learning on the way i hope you guys like this project and it inspires you guys to maybe follow im my tracks and try something like this out


any questions email me at iamtictac456@gmail.com i will be glad to help out :)





---


**#Update 1**_(2/26/2014)_

So i when out and go some parts witch im going to be using for testing and playing around with i got all my parts from https://core-electronics.com.au/store/ if said otherwise

MPU-6050 Module 3 Axis Gyroscope + Acce​​lerometer
018-MPU-6050
$7.44

Ordered: 1
$7.44

9g micro servo (1.6kg)
SER0006
$4.23

Ordered: 4
$16.91

Transparent Solderless Breadboard - 300 Tie Points (ZYJ-60)
019-ZYJ-60
$3.44

Ordered: 1
$3.44

Solderless Breadboard Jumper Cable Wires (75 Pieces)
019-ZY-800
$3.49

Ordered: 1
$3.49

Solderless Breadboard Jumper Cable Wires (Female-Female 75 Pieces)
019-ZY-811
$3.47

Ordered: 1
$3.47


and if you use the promo code FACEBOOK you get some money off witch is allays nice.
im not to sure if this auto pilot is going to be better to do in a quad or a plane...... im not too sure yet email me guys if u want to tell me something or just ask a question email is :iamtictac456@gamil.com


Just some more links that i have been using as help :
http://blog.bitify.co.uk/2013/11/3d-opengl-visualisation-of-data-from.html
http://diydrones.com/forum/topics/hexa-yaws-ccw-when-raising-throttle
http://blog.bitify.co.uk/2013/11/reading-data-from-mpu-6050-on-raspberry.html


**How to read data from MPU-6050**
> first run this command
```
sudo apt-get install python-smbus 
```

then make a new python code with this
```

#!/usr/bin/python

import smbus
import math

# Power management registers
power_mgmt_1 = 0x6b
power_mgmt_2 = 0x6c

def read_byte(adr):
    return bus.read_byte_data(address, adr)

def read_word(adr):
    high = bus.read_byte_data(address, adr)
    low = bus.read_byte_data(address, adr+1)
    val = (high << 8) + low
    return val

def read_word_2c(adr):
    val = read_word(adr)
    if (val >= 0x8000):
        return -((65535 - val) + 1)
    else:
        return val

def dist(a,b):
    return math.sqrt((a*a)+(b*b))

def get_y_rotation(x,y,z):
    radians = math.atan2(x, dist(y,z))
    return -math.degrees(radians)

def get_x_rotation(x,y,z):
    radians = math.atan2(y, dist(x,z))
    return math.degrees(radians)

bus = smbus.SMBus(0) # or bus = smbus.SMBus(1) for Revision 2 boards
address = 0x68       # This is the address value read via the i2cdetect command

# Now wake the 6050 up as it starts in sleep mode
bus.write_byte_data(address, power_mgmt_1, 0)

print "gyro data"
print "---------"

gyro_xout = read_word_2c(0x43)
gyro_yout = read_word_2c(0x45)
gyro_zout = read_word_2c(0x47)

print "gyro_xout: ", gyro_xout, " scaled: ", (gyro_xout / 131)
print "gyro_yout: ", gyro_yout, " scaled: ", (gyro_yout / 131)
print "gyro_zout: ", gyro_zout, " scaled: ", (gyro_zout / 131)

print
print "accelerometer data"
print "------------------"

accel_xout = read_word_2c(0x3b)
accel_yout = read_word_2c(0x3d)
accel_zout = read_word_2c(0x3f)

accel_xout_scaled = accel_xout / 16384.0
accel_yout_scaled = accel_yout / 16384.0
accel_zout_scaled = accel_zout / 16384.0

print "accel_xout: ", accel_xout, " scaled: ", accel_xout_scaled
print "accel_yout: ", accel_yout, " scaled: ", accel_yout_scaled
print "accel_zout: ", accel_zout, " scaled: ", accel_zout_scaled

print "x rotation: " , get_x_rotation(accel_xout_scaled, accel_yout_scaled, accel_zout_scaled)
print "y rotation: " , get_y_rotation(accel_xout_scaled, accel_yout_scaled, accel_zout_scaled)
```

Then just save that as gyrotest.py and then run this command
```
sudo python gyrotest.py
```
and your done

---
