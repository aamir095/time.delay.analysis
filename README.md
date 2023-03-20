

# Delay Measurement in the Network of Different Interconnected Devices

Abstract— A testbed was built that included devices such as a  Programmable Logic Controller (PLC), two Raspberry PI, camera, conveyor belt, robotic arm, and high-performance laptop for edge computing. Then, like industry standards, an  assembly line system was developed. The object in the conveyor  belt is first detected by the Raspberry Pi with a camera. Object  detection was done by the Yolo algorithm. The object detected  signals were sent to the PLC, which controls the conveyor belt  and sends commands to the robotic arm via Raspberry Pi.  Based on the detected object type, the robotic arm can perform  a specific task. The network switch connects all these devices,  and the communication protocol is TCP/IP. Signals passed  through various nodes on their way from object detection to  robotic arm motion. When signals are computed and  transmitted from one device to another, computational and  transfer delays are introduced. As a result, the system  experiences lag after commands are send. Because there are  multiple devices in our testbed, the delay will be greater. The  delay in the various nodes were thoroughly investigated and  measured. The solutions proposed for the delay have led to  substantial improvements. When compared to object detection  using a Raspberry Pi, edge computing reduces object detection  time by around 7 times. The combination of edge computing  with a high-performance hardware device result in a significant  decrease of total network delay. 

Keywords— Programmable Logic Controller (PLC), Robotic  Arm, Network Delay, Raspberry Pi (PI), Python,  Ladder Programming, Cyber-Physical System  (CPS), YOLO, Edge Computing

## Introduction

According to Lee and Seshia, the term Cyber-Physical System was coined by Helen Gill around 2006, at the National Science Foundation (NSF) in the U.S., to refer to the integration of computation with physical processes [1]. Often, it also has a communicating or networked aspect. The interconnected system of different physical and computational components that provides the foundation of engineering application, and base of emerging smart services refer to Cyber-Physical Systems. Cyber-Physical System (CPS) innovation and application are emerging in real life word such as Embedded Systems, Control Theory, and Mechatronics. Reactive computation, concurrency, feedback control of the physical world, real-time computation, and safety-critical applications are some of the features of CPS. Cleaning robots, smart heating, medical instruments, industrial systems, electric bikes, and smart grids are just a few examples of real world CPS applications. The use of CPS in health-care applications such as image-guided surgery, robotic surgery, and brain-machine interfaces is growing. In this era of Industry 4.0, cps develops smart industry capabilities and aids in the development of the industrial internet of things. CPS uses modern control systems, a variety of hardware and software, and the Internet of Things (IoT) to create new ways of producing, creating value, and optimizing in real-time. The CPS can be connected to the internet, allowing for cloud computing, smart monitoring, and data management. CPS is critical to the fourth industrial revolution's organization and improvement. The Industrial control system (ICS) contains a wide range of CPS applications. Industrial processes are automated using a variety of devices, systems, controls, and networks. The most common ICS are Supervisory Control and Data Acquisition (SCADA) systems and Distributed Control Systems (DCS). Within the same geographic region, a distributed control system (DCS) is utilized to control production systems. It typically consists of a computer that communicates with control devices located throughout the plant or process, such as machine or process controllers and PLCs, through a bus or directly, and displays the data gathered. For command and monitoring, DCS systems typically include a hierarchy of controllers located throughout a plant and connected by a communications network, whereas SCADAs have centralized control. DCS components are frequently seen in SCADA systems. DCS control system are organized into five subsystems: process interface, process control, process operations, applications engines, and communication subsystems. The DCS is more process oriented whereas SCADS is data oriented [2]. The simple architecture of the DCS is expressed in figure 1. The DCS system consists of multiple controlling elements and reliability of the system increases.


![DCS controlling different parts](https://user-images.githubusercontent.com/48818645/209208406-0855b5b3-500b-43e5-9cc6-b713788c0030.PNG)

Figure 1: DCS controlling multiple devices with PLC 


In the industry, the use of robots and robotic arms is increasing. There is automation everywhere. A similar assembly line is created in this experiment using a conveyor belt and a robotic arm. “Machine vision systems (MVS) are based on capturing an image (typically via camera), extracting a series of data from that image, analyzing it, and once evaluated, deciding accordingly [3]. MVP is critical in the development of automated systems. According to a report published by the German industry association VDMA, the machine vision industry grew by 2.6 billion euros in 2017, with the automotive industry accounting for a large portion of that growth [4]. The production industry is focused on robots and machine vision for, which is critical to the growth of Industry V4.0. In real-time machine operation, even a millisecond delay is significant. The object detection algorithm used by the MVS has been optimized to speed up the process. To reduce computational time and memory, a variety of new machine learning and neural network techniques have been optimized for image and video analyses. Deep neural networks have improved their performance to process visual data over time [5]. They have been established as a key player in computer vision applications. Object vision is used in a variety of fields, including autonomous driving, surveying, and health care. Various algorithms were developed, including Fast R-CNN, Faster R-CNN, Single Shot Detector (SSD), Spatial Pyramid Pooling (SPP-net), and YOLO (You Only Look Once) [6]. The state of the art in object detection is YOLO. Yolo is used in this study to detect objects on the conveyor belt. The rapid development 3D object detection and next-generation sensor will help to grow the automation industry. Various components were connected to form a network in this experiment. With the help of the Yolo algorithm, the Pi camera is used to capture video of the conveyor system and detect object. The object detection signal is then sent to a PLC, which controls the conveyor belt as well as a second Pi with a robotic arm. The Raspberry Pi collects data from the PLC and uses it to control the robotic arm. The Raspberry Pi and robotic arm are both programmed in Python. Ladder programming is used to program the PLC. The experiment set up is explained with the help of figure 4. Signals passed through various nodes on their way from object detection to robotic arm motion. When signals are computed and transmitted from one device to another, computational and transfer delays are introduced. As a result, the system experienced some lag after commands were sent. Because there are multiple devices in our testbed, the delay will be greater. The delay in the various nodes were thoroughly investigated and measured.

## Proposed System
In this section, the brief explanation regarding the hardware, software, and challenges are discussed. In this testbed, the PLC serves as the central system. It acts as a communication bridge between two Raspberry PI’s and controls the conveyor belt. A 24V dc motor on the conveyor belt is directly connected to the PLC. The PLC memory is updated with the object class when the object is detected in a moving conveyor belt. The conveyor belt is directly connected to the PLC's output pins and is controlled using ladder logic. The conveyor belt gets stopped when an object is detected within the camera frame. The PI controlling the robotic arm receives the object's information from the PLC memory and instructs the robotic arm to drag the object away from the conveyor belt.

### A. Hardware

One PI controls the camera, while the other controls the  robotic arm. The PLC functions as a central station, receiving  signals from one PI and sending them to another. Let's go over  each component in the testbed and their functions. Only the  physical operation is defined in this section and software  which operates hardware will be discussed later. The functions  of each device in the testbed are listed in table. 

![device with the functions](https://user-images.githubusercontent.com/48818645/209210582-75fce2db-d1ab-494f-915b-598f12b31868.PNG)

The conveyor belt was improved, and a 24v 5RPM motor was installed. The motor can be controlled directly by the PLC. The testbed's sequential operation is described below.
1. The PI1 camera continuously monitors the object on the conveyor belt as it moves. The PLC is used to control the conveyor belt. The PI camera will be at some height above the conveyor belt. 
2. If the camera detects our target objects, PI1 sends commands to the PLC. 
3. The PLC receives information about the object's class and location. The PLC sends the same data to the PI2. The PLC then instructs the conveyor belt to come to a halt. 
4. The PLC sends signals to the PI2 with robotic arm about the object's class and location. The arm's servo motors are then given commands to perform a specific task for that object. 
5. This process continues. 

Signals travel from various nodes and devices to detect objects and move robotic arms. The signals are computed at each device, which increases the signal delay. We're looking for a delay in signal transmission between nodes in this experiment.


### B. Software

Hardware cannot run standalone. The backbone that drives  the hardware is software. Python is used to program the  Raspberry Pi and robotic arm. Ladder programming is used in  PLCs. The figure 3 explains about the modules and software  used. 

![software n it's function](https://user-images.githubusercontent.com/48818645/209211856-2d45f788-1427-448f-9ac3-000c074f36af.PNG)

The video is captured by the camera, which is then processed by the Yolov5 algorithm (via PI1) to detect the object. The Raspberry Pi's object detection time is quite long. Using the snap7 python library, the object's class is written into the PLC's data base memory. After reading the PLC's memory about the object's location and class, PI2 controls the robotic arm.

![block diagram](https://user-images.githubusercontent.com/48818645/209212790-97291674-bcb6-4d50-87ca-2785cdc6506f.PNG)


## Experimental Setup and Results

### A. Experiment Setup
The list of hardware implemented in this experiment is described in detail in section 3. The hardware configuration and specifics are detailed in this section. The testbed consists of devices such as a Programmable Logic Controller (PLC), two Raspberry PI, camera, conveyor belt, robotic arm, and high-performance laptop for edge computing. The camera records video of the conveyor belt to detect objects. The Raspberry Pi is used in the first scenario to analyze video and detect objects on the conveyor belt. In the second scenario, video is sent via TCP to another high-performance device for processing and object detection. Both devices' processing times were monitored and studied. Following the discovery of an object of interest on the conveyor belt. The Raspberry Pi informs the PLC that an object has been detected. The PLC then stops the conveyor belt from moving and the second PI controlling the robotic arm reads the data from the PLC. The PI directs the robotic arm to do the desired task. The figure 5 illustrates all the hardware used in the experiment.


![hardware testbed](https://user-images.githubusercontent.com/48818645/209214015-2d465331-da3f-4627-963d-839179c83ca4.PNG)

Let's go through each component of hardware with their roles. 
1. Raspberry Pi with camera (@ 30 fps) for video recording and object detection on conveyor belt. Write into the PLC memory address about the type of object detected. 
2. High performance laptop for edge computing. It uses the transmission control protocol technique to receive video from the PI. The video is processed using the YOLOv5 algorithm, and the object detection information is sent to PI. The time it takes to detect an item is considerably decreased when using edge computing. 
3. Programmable Logic Controller (PLC) serves as the central controller, controlling the conveyor belt and storing the information about the detected object in its memory. Both Raspberry PI are networked to the PLC. 
4. The conveyor belt is directly controlled by the PLC. The PLC's 24 V output pin is connected to the motor in the belt. The belt is controlled by a 24V and 5 RPM motor. 
5. The PI that reads the object detection information from the PLC memory address and controls the robotic arm. 
6. The PLC, two Raspberry PIs’, and the edge server are all connected through the network switch. TCP is used as a communication module and all devices are connected through an ethernet cable.

### B. Experiment Results
The testbed has several devices and nodes. There is a delay when one device sends signals or data to another. When signals are computed and transmitted from one device to another, computational and transfer delays are introduced. As a result, the system experienced some lag after commands are send. Because there are multiple devices in our testbed, the delay will be greater. The delay in the various nodes were thoroughly investigated and measured. The table 3 illustrates all the delay introduced in the network. The Raspberry Pi's camera records videos to detect objects on the conveyor belt. The Yolov5 algorithm is used to process and identify objects in the video. The Raspberry Pi processes and identifies an object in 3-4 seconds [table 3], which is a long time for detection. The second column in table 3 represents the Raspberry Pi's objection detection time. Because the PI has limited performance power, processing video takes more time. The average time for object detection was reduced significantly to 0.485 seconds when edge computing was used for video processing and object detection. The average object detection time for the edge computing platform is shown in the third column of table 3. For edge computing, a high-performance laptop was used. Following the object detection, the Raspberry Pi sends the data to the PLC. The object detection class is stored in the PLC's memory address. The total time to write 4 bytes of information into the PLC memory from the Raspberry Pi is shown in the third column of the above table. Writing four bytes of data into the PLC memory takes an average of 0.128 seconds [table 3] for raspberry pi. An ethernet switch connects the raspberry pi and the plc. The other PI connected to the PLC which controls the robotic arm reads the PLC memory. Reading 4 bytes of data from the PLC memory takes an average of 0.063 seconds. The average time required by the Pi to read bytes information from the PLC is shown in the fifth column of the above table. When the Raspberry Pi sends a command to the robotic arm to perform a specific action, the final delay occurs. The robotic arm has four adeept AD002 servos. Sending commands from the PI takes about 0.056 seconds [table 3] to move the robotic arm. 

Using PI for object detection, the total delay from object detection to robotic arm movement = 3.409 seconds. 
Using edge computing for object detection, the total delay from object detection to robotic arm movement = 0.734 seconds.


![delay_meaurement_results](https://user-images.githubusercontent.com/48818645/209215721-9dcc0ca4-73d7-4cae-bb02-fadc92bbcb38.PNG)


## Conclusion and Future Direction

A network of interconnected cyber-physical systems was constructed. Computation and transfer delays are introduced when signals are computed and transmitted from one device to another. As a result, the system lags after commands have been sent. The delay depends on the number of devices in the network. When using PI to process object detection, a significant delay was introduced. The delay in object detection in the network was eliminated with the introduction of edge computing. When compared to object detection using a Raspberry Pi, edge computing reduces object detection time by around 7 times. The combination of edge computing with a high-performance hardware device result in a significant decrease of total network delay. The delay in writing and reading from the PLC memory was thoroughly investigated and analyzed. A holistic method for determining the delay of an interconnected network of devices and solutions for the delay was proposed.

#### This work was supported by National Science Foundation(NSF) Awards No. 1646458 and No. 2146968.





