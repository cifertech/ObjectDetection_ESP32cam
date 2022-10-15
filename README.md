<div align="center">

  <img src="https://user-images.githubusercontent.com/62047147/195847997-97553030-3b79-4643-9f2c-1f04bba6b989.png" alt="logo" width="100" height="auto" />
  <h1>ObjectDetection ESP32cam</h1>
  
  <p>
    Using ESP32-CAM for the Object Detection system
  </p>
  
  
<!-- Badges -->

<p>
<a href="https://github.com/cifertech/ObjectDetection_ESP32cam" title="Go to GitHub repo"><img src="https://img.shields.io/static/v1?label=cifertech&message=ObjectDetection_ESP32cam&color=white&logo=github" alt="cifertech - ObjectDetection_ESP32cam"></a>
<a href="https://github.com/cifertech/ObjectDetection_ESP32cam"><img src="https://img.shields.io/github/stars/cifertech/ObjectDetection_ESP32cam?style=social" alt="stars - ObjectDetection_ESP32cam"></a>
<a href="https://github.com/cifertech/ObjectDetection_ESP32cam"><img src="https://img.shields.io/github/forks/cifertech/ObjectDetection_ESP32cam?style=social" alt="forks - ObjectDetection_ESP32cam"></a>
   
<h4>
    <a href="https://twitter.com/cifertech1">TWITTER</a>
  <span> · </span>
    <a href="https://www.instagram.com/cifertech/">INSTAGRAM</a>
  <span> · </span>
    <a href="https://www.youtube.com/c/cifertech">YOUTUBE</a>
  <span> · </span>
    <a href="https://cifertech.net/">WEBSITE</a>
  </h4>
</div>

<br />

<!-- Table of Contents -->
# :notebook_with_decorative_cover: Table of Contents

- [About the Project](#star2-about-the-project)
  * [Pictures](#camera-Pictures)
  * [Features](#dart-features)
- [Getting Started](#toolbox-getting-started)
  * [Schematic](#electric_plug-Schematic)
  * [Installation](#gear-installation)
- [Usage](#eyes-usage)
- [Contributing](#wave-contributing)
- [License](#warning-license)
- [Contact](#handshake-contact)

  

<!-- About the Project -->
## :star2: About the Project
In the this tutorials of the ESP32-CAM series, we saw that using the original code, we will be able to process face image from face recognition to face separation, but in cases where we need to recognize different objects, different models must be introduced to our code. To be able to identify the objects we want with self-learning. But no matter how high the processing power of ESP chips, we can not leave all this complex processing to this small chip, so we will use Tensorflow.JS to combine it with the video sent from ESP32-CAM. Note that in this tutorial, Tensorflow.JS runs in the computer browser and therefore the machine learning model runs inside your browser.

<!-- Pictures -->
### :camera: Pictures

<div align="center"> 
  <img src="https://user-images.githubusercontent.com/62047147/195979364-d6898632-643b-46e4-81a0-fb5312c57eea.jpg" width="800" height="auto" />
</div>

<!-- Features -->
### :dart: Features

- Selection of the desired objects in the web server
- Real-time object detection
- No need to run the second code on the computer

<!-- Getting Started -->
## 	:toolbox: Getting Started

In this project, by streaming images using the ESP32-CAM board and receiving and displaying them in the browser, we will also use Tensorflow.JS to process images using the default models applied.

- ESP32-CAM
- USB-To-TTL

<!-- Schematic -->
### :electric_plug: Schematic

To program the ESP-CAM board, we need Arduino-IDE software, and of course, download the relevant board in the software environment, as well as install the required libraries. For information on these cases, you can refer to this tutorial. Make the connections according to the schematic below according to the USB TO TTL used. Note that when programming code, ie after compiling the code, the two pins GPIO 0 and GND are connected to each other, and after successfully compiling the code, you must disconnect this connection to run the project.

Make the connections according to the table and schematic below.

| ESP32CAM | USB-To-TTL |
| ----   | -----|
| U0T | RXD |
| U0R | TXD |
| 5v  | VCC |
| GND | GND |


* Complete Schematic

<img src="https://user-images.githubusercontent.com/62047147/195979839-2305fe9a-5749-47cb-b2f5-0511903d32f0.jpg" alt="screenshot" width="800" height="auto" />


<!-- Installation -->
### :gear: Installation

In this part of the action code, we will introduce Tensorflow.js to analyze the received images in the browser. In the next line, we will also import COCO-SSD models along with Tensorflow.js.
```c++
<script src="https:\/\/ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js"></script>
<script src="https:\/\/cdn.jsdelivr.net/npm/@tensorflow/tfjs@1.3.1/dist/tf.min.js"> </script>
<script src="https:\/\/cdn.jsdelivr.net/npm/@tensorflow-models/coco-ssd@2.1.0"> </script>
```

In the next section, we will load the introduced models so that they can be recognized as a result of processing the received image for our machine.
```c++
    function ObjectDetect() {
      result.innerHTML = "Please wait for loading model.";
      cocoSsd.load().then(cocoSsd_Model => {
        Model = cocoSsd_Model;
        result.innerHTML = "";
        getStill.style.display = "block";
      }); 
    }
```

After our machine detects an object in the image, it should now be visible to the user in the form that squares around the detected image are usually used in image processing, the following code is for this.
```c++
 if (Predictions.length>0) {
          result.innerHTML = "";
          for (var i=0;i<Predictions.length;i++) {
            const x = Predictions[i].bbox[0];
            const y = Predictions[i].bbox[1];
            const width = Predictions[i].bbox[2];
            const height = Predictions[i].bbox[3];
            context.lineWidth = Math.round(s/200);
            context.strokeStyle = "#00FFFF";
            context.beginPath();
            context.rect(x, y, width, height);
            context.stroke(); 
            context.lineWidth = "2";
            context.fillStyle = "red";
            context.font = Math.round(s/30) + "px Arial";
            context.fillText(Predictions[i].class, x, y);
```
   
   
   
<!-- Usage -->
## :eyes: Usage

After uploading the desired code, I reset the board in the serial ip of the web server will be displayed for us, such as 192.168.1.103, which we will search for in the browser to enter the web server page, we have to wait for a while until Load the related models, then click on the StartDetect icon to display the images and recognize the defined items. Also, if there is a problem with the image processing or other parts, click on the Restart option.

<img src="https://user-images.githubusercontent.com/62047147/195980021-7637c625-116f-4123-a6f4-a36742b5495f.jpg" alt="screenshot" width="500" height="auto" />

- In the web server, it is possible to count it by specifying the desired item. By selecting the desired object, the number of detected items will be displayed in front of it.
<img src="https://user-images.githubusercontent.com/62047147/195980136-173d2ef5-bbee-4776-ba79-f67e6ba8162b.jpg" alt="screenshot" width="500" height="auto" />

- In this section you adjust the output video settings. For example, it is possible to change the resolution or mirror the image in this section.
<img src="https://user-images.githubusercontent.com/62047147/195980222-08ea28db-fbdb-479a-93b1-f4d9a2adb499.jpg" alt="screenshot" width="500" height="auto" />

- In this section, the contrast, quality and brightness of the received image can be adjusted.
<img src="https://user-images.githubusercontent.com/62047147/195980239-200f2833-4b5a-438b-a50b-9c5628131109.jpg" alt="screenshot" width="500" height="auto" />

- If the object is detected, its name will be displayed in this section, and also the percentage of probability that our machine analysis is correct, as well as the coordinates and number of detected objects can be displayed in this section.
<img src="https://user-images.githubusercontent.com/62047147/195980252-9bca839f-fe8d-447a-8891-be696c15b2f8.jpg" alt="screenshot" width="500" height="auto" />

- Finally, we have the received image, which will inform the user if he detects an object with a square and also enters its name.
<img src="https://user-images.githubusercontent.com/62047147/195980264-6a17171a-5a0d-4cfc-adfe-287f465374e3.jpg" alt="screenshot" width="500" height="auto" />



<!-- Contributing -->
## :wave: Contributing

<a href="https://github.com/cifertech/ObjectDetection_ESP32cam/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=cifertech/ObjectDetection_ESP32cam" />
</a>


<!-- License -->
## :warning: License

Distributed under the MIT License. See LICENSE.txt for more information.


<!-- Contact -->
## :handshake: Contact

CiferTech - [@twitter](https://twitter.com/cifertech1) - CiferTech@gmali.com

Project Link: [https://github.com/cifertech/ObjectDetection_ESP32cam](https://github.com/cifertech/ObjectDetection_ESP32cam)
