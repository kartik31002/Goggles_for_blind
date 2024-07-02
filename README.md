# CS 321 : GOGGLES FOR BLIND

```
Functional facial recognition with navigation assist through obstacle detection
```
![](/images/5.png)

## INTRODUCTION

#### PROJECT OVERVIEW

The purpose of this project is to develop a pair of smart goggles designed to
assist individuals who are visually impaired. These goggles incorporate two key
features: obstacle detection through an ultrasonic sensor and facial recognition
using an ESP32 Cam. By combining these functionalities, the smart goggles aim to
enhance the daily lives of blind individuals, enabling them to navigate their
surroundings more safely and engage in social interactions more effectively.


#### OBJECTIVES AND GOALS

The primary objective of this project is to develop a robust and reliable
system that assists individuals with visual impairments in navigating their
environment. The specific goals include:
```
● Creating a hardware setup that seamlessly integrates the ultrasonic sensor
and ESP32 Cam into a pair of goggles.
● Implementing obstacle detection functionality to provide real-time
feedback about nearby obstacles.
● Incorporating facial recognition capabilities to enable the recognition of
familiar faces.
```
#### SIGNIFICANCE AND IMPACT

Visual impairment poses significant challenges to individuals in their daily
lives, limiting their independence and access to information. The potential impact
of this project is substantial, as it has the potential to enhance the quality of life
for visually impaired individuals, promoting greater mobility, social inclusion,
and overall well-being.


## METHODOLOGY

#### HARDWARE COMPONENTS

The hardware components used in this project include:
```
● NodeMCU: This microcontroller board serves as the central control unit for
the smart goggles, interfacing with the various sensors and actuators.
● Ultrasonic Sensor: An ultrasonic sensor is employed to detect obstacles in
the surroundings by emitting sound waves and measuring the time it takes
for them to bounce back.
● ESP32 Cam: The ESP32 Cam module is utilized for capturing images and
performing facial recognition tasks.
● Buzzer: A buzzer is connected to the NodeMCU to provide audio feedback
when an obstacle is detected.
```
#### SOFTWARE TOOLS AND LANGUAGES

The following software tools and programming languages are used in this project:
```
● Arduino IDE: It is used for programming and configuring the ESP32 CAM
microcontroller
● Thonny IDE: It is used for programming and configuring the NodeMCU
microcontroller board in micropython.
● Micropython: A less rigorous version of python which works well when
executing codes and multiple files in a microcontroller like NodeMCU.
● Python: Python programming language is used for image processing, facial
recognition algorithms, and data analysis.
● MQTT: A data transfer protocol used for transfer of data between our
server and the microcontrollers.
```

## HARDWARE SETUP

#### COMPONENT CONNECTION

Wehavetwosectionsofhardwareconnection.

We have two sections of hardware connection .
1. **Obstacle detection** - For Obstacle detection, Ultrasonic sensor and
NodeMCU are used . The Ultrasonic sensor and buzzer are connected with
the NodeMCU and the NodeMCU is connected to a power supply (Here we
used a power bank).

![](/images/4.png)

2. **Face Detection** - For tasks such as uploading sketches, configuring settings,
and monitoring the output of the ESP32 Cam on a computer, an FTDI
adapter is required to be connected with the ESP32 Cam. The connections
are made as follows:

![](/images/3.png)

#### GOGGLE DESIGN

The smart goggles should be designed to accommodate the hardware components
securely and comfortably. More considerations are to be given to ergonomics,
weight distribution, and user comfort in the future. Mounting options for the
ultrasonic sensor and ESP32 Cam are to be incorporated into the design, ensuring
proper alignment and field of view.

![](/images/2.png)

## OBSTACLE DETECTION

#### NodeMCU and Ultrasonic Sensor Interaction

Importing hcsr04 micropython library . This is a micropython driver for the well-known
ultrasonic sensor HC-SR04 .
Hardware Connections:
1. Connect the VCC pin of the ultrasonic sensor to the 5V pin on the NodeMCU.
2. Connect the GND pin of the ultrasonic sensor to the GND pin on the NodeMCU.
3. Connect the Trig pin of the ultrasonic sensor to any available digital pin on the
NodeMCU (e.g., D1).
4. Connect the Echo pin of the ultrasonic sensor to any available digital pin on the
NodeMCU (e.g., D2)
5. Connect the buzzer to the output pin of the NodeMCU .

#### Obstacle detection criterion

When the ultrasonic sensor will detect the distance of the obstacle and if its less then 70
cm , the buzzer will start buzzing .

```
if distance >  0 and distance < 70:
```
#### Buzzer activation

buzzer = Pin(14, Pin.OUT) - buzzer initialization at output pin 14
buzzer.value(1) - activation of buzzer when conditions get satisfied


HCSR04micropythondriver-
[Github Link](https://github.com/rsc1975/micropython-hcsr04/blob/master/hcsr04.py)

## Face recognition

![](/images/1.png)

#### ESP 32 CAM FEATURES

```
● ESP32-CAM is a WIFI+ bluetooth dual-mode development board that uses PCB
on-board antennas and cores based on ESP32 chips. It can work independently
as a minimum system.
● ESP integrates WiFi, traditional bluetooth and BLE Beacon, with 2
high-performance 32-bit LX6 CPUs, 7-stage pipeline architecture, main
frequency adjustment range 80MHz to 240MHz, on-chip sensor, Hall sensor,
temperature sensor, etc.
● Fully compliant with WiFi 802.11b/g/n/e/i and bluetooth 4.2 standards, it can be
used as a master mode to build an independent network controller, or as a slave
to other host MCUs to add networking capabilities to existing devices.
● ESP32-CAM can be widely used in various IoT applications. It is suitable for
home smart devices, industrial wireless control, wireless monitoring, QR wireless
identification, wireless positioning system signals and other IoT applications. It is
an ideal solution for IoT applications.
```
#### CAPTURING AND PROCESSING FACIAL IMAGES

1. Hardware Setup
● Connect the ESP32-CAM board and camera module .Make sure the
ESP32-CAM board is powered appropriately and connected to a stable
network.
2. Software Setup
● Set up the Arduino IDE with ESP32 support and run a web server based
camera streaming module. This creates a webserver on a local ip and
sends the camera’s output onto the web server. The ESP also publishes a
message to an MQTT topic on which it sends the ip of the web server.
3. Face recognition setup
● Set up a server and install the python face recognition library. With the
help of the paho-mqtt client we will access the ip of the web server
created by the ESP and get the image captured by the ESP.
4. Capture Some Known Face
● Capture and save images of known faces using the camera module
connected to the ESP32-CAM.
● Preprocess and encode the unknown face image using the face
recognition library.

#### FACE RECOGNITION AND AUDIO FEEDBACK

1. MQTT Communication
● Set up an MQTT broker or use an existing broker for communication.
● Configure the ESP32-CAM to connect to the MQTT broker using the
appropriate MQTT library (e.g., PubSubClient).
● On the face recognition server, subscribe to the specific topic on the
MQTT broker to receive the IP address .
● In the MQTT message callback, retrieve the url from the received message
payload. As soon as the IP is set , the camera module will start taking
data.
● Compare the unknown face encoding with the known face encodings
saved earlier to determine if there is a match .
2. Audio Feedback
● Import ‘playsound’ library for audio feedback of the recognized face .
● Pre save all the necessary mp3 files of the names of known persons.
● As soon as a known face gets detected , we’ll trigger those pre saved files.


#### CONSIDERATIONS AND CHALLENGES

When implementing face recognition using ESP32-CAM and MQTT , there are several
consideration and challenges to keep in mind :
1. Face Recognition Accuracy: Face recognition accuracy can vary depending on
the chosen library and the quality of the captured images. Factors such as
lighting conditions, angles, and variations in facial appearances can affect the
accuracy of the recognition algorithm. It's important to test and fine-tune the
system to achieve satisfactory results.
2. Network Connectivity : The ESP32-CAM needs a stable network connection for
MQTT communication.
3. Power Consumption: The ESP32-CAM can consume significant power, especially
when the camera module is actively capturing images .


## Integration, Testing and Evaluation

#### COMBINED FUNCTIONALITY

The ESP32 and the NodeMCU both are mounted on the frame of the goggles, connected to
the NodeMCU are the ultrasonic sensor and a buzzer. When both the micro-controllers
are powered, our smart goggles are ready to use. The buzzer buzzes as soon as the
reading from the sensor is less than 70 cm or when there is an obstacle at less than 70 cm
from the person wearing the goggles. Once the ESP 32 starts, it creates a web server and
sends the ip to the face recognition server. On the face recognition server, it starts taking
photos from the server at very high speed and whenever it detects a face of a known
person, the pre-recorded sound is played.


#### Testing Process

1. Testing the obstacle detection function
The obstacle detection functionality was tested with many different combinations,
with different number of sensors, different position for the sensor and different
threshold distance for the buzzer to get activated and a simple design of a single
sensor and a threshold distance of 70 cm was the most functional.
2. Testing the facial recognition function
Face recognition was tested on the basis of how fast it can get the image from the
web server and how much time it takes to recognize a person from that picture.
First ip was coded manually in the code, then mqtt server was utilized and
pre-trained model for facial recognition was used, this decreased the time for face
detection many folds.

#### Performance Results

1. Obstacle Detection
The person has to rotate his/her neck to detect obstacles in the respective side. The
ultrasonic sensor takes 10 microseconds to detect the distance of the obstacle and
all in all it takes less than a millisecond for the buzzer to start buzzing when an
obstacle is detected. The ultrasonic sensor has a delay of 1 millisecond between
every measurement.
2. The facial recognition takes 50 milliseconds to recognize a face, where it
downloads the image from the web server, recognizes the face and then plays the
sound of the name of the person. When the sound is played the loop pauses for 1
second (length of the sound played).

#### USER FEEDBACK AND USABILITY

When worn by a blindfolded friend, he was easily able to detect obstacles like walls and
doors from a safe distance. He had to move his neck quite often from left to right in
order to detect obstacles on the side. The facial recognition was on point, with less than
10 percent error chances. The person has to stand in front of the camera for less than a
second and his face will be recognized and the server will play his name sound. Usability
wise the project is good enough for basic obstacle detection but if we want to detect
obstacles on the side we have to look at that side.

#### LIMITATIONS AND IMPROVEMENTS

1. More ultrasonic sensors could be used to effectively map the walkable region of
the visually impaired person and then output him the correct direction.
2. Haptic feedback is more intuitive than a buzzer. Vibration motors can be used and
with more than one sensor and motor we can give different output for different
directions.
3. New face registration is currently unavailable and has to be hardcoded in the
server's code.

## Conclusion

#### PROJECT SUMMARY

The purpose of this project was to develop a pair of smart goggles designed to
assist individuals who are visually impaired. These goggles incorporate two key
features: obstacle detection through an ultrasonic sensor and facial recognition
using an ESP32 Cam. By combining these functionalities, the smart goggles aim to
enhance the daily lives of blind individuals, enabling them to navigate their
surroundings more safely and engage in social interactions more effectively.

#### IMPACT AND FUTURE PROSPECTS

When perfected more in the similar direction, these goggles can efficiently become the
eyes of a visually impaired person. The day to day difficulties of not being able to sense
what is in front of you can be changed by these goggles. Now, they don’t have to depend
on the person standing in front of them to talk to identify themselves but it can be done
automatically.
When combined with more sensors, obstacle detection can be perfected. Obstacles on the
side, with different heights can be detected efficiently. With the added feature of object
recognition, the camera can provide so much more information. We can add an SOS
button which can send the current location and a snapshot of what is happening to the
person to emergency contacts or the police. And many more functionalities can be added
to the goggles.

#### LESSON LEARNT

The lesson we learned while the development of this project was how rewarding the help
of a needy person can be. The idea of being able to become the eye of the unfortunate
gives a rush to the brain and heart and strives us forward to make the project as useful
as possible to the needful. In conclusion, using simple microcontrollers and sensors, we
can make something so simple yet so elegant and can provide benefits to all of society.


