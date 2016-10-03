# How-to-Use-IR-Remotes-with-Arduino
Turn a LED on and off with any TV remote

source: http://www.instructables.com/id/The-Easiest-Way-to-Use-Any-IR-Remote-with-Ardiuno/?ALLSTEPS

Step 1: Ingredients:

Electronics:

Arduino
Any IR remote
IR receiver
Breadboard
Jumper Cables
LED

Step 2: Downloads

Here is the link to all the downloads in this instructable.
(Sorry for some odd reason the link won't work so just copy and paste it in your browser)


http://www.mediafire.com/download/cv0r5191fvtq0y2/IR_I'ble_Package.zip

Step 3: Conflicting RobotIRremote Library

Arduino has been making some changes to the IDE, and now there is a conflicting IR library. I won't be using this library, because I still want to use the IRremote by Ken Sheriff. I've had great success with it in the past, and so have many others. So let's get this file deleted.

Mac: Applications/Arduino (right mouse click -->show package contents) /Contents/Java/libraries/RobotIRremote

Windows: C:\Program Files (x86)\Arduino\libraries\RobotIRremote

Once you have located the folder RobotIRremote, delete it. Restart the Arduino IDE and your RobotIRremote library should be gone.

Step 4: All Those Remotes!

TV Remotes, CD player remotes, heater remotes, DVD player remotes, all those remotes! Many people just have old remotes laying around because they item that the went to broke. I have collected quite a few remotes over the past week. I just asked all my friends if they had any old remotes laying around and sure enough I collected about 7 of them. So finding a remote isn't very hard. I good option if you want a professional looking one is to buy the specialty MP3 player remote. I have one because it came with my Arduino kit.

Here are a few good cheap remotes that you can get.

https://www.sparkfun.com/products/10280
http://www.jameco.com/webapp/wcs/stores/servlet/Product_10001_10001_2152315_-1
And even Dollar General has cheap, universal remotes!!

Step 5: Installing the IR Library

The very first thing that we need to do associating with Arduino is to download the IR library. To make things simpler, I have included a .zip of the IR library. Download it to your computer, unzip it, then place it in your Arduino libraries folder. Don't know where it is?

Open up the Arduino IDE and on the menu select Sketch>IncludeLibrary>Add Library and select the 'IRremote' folder. If you are on a PC you need to delete the mac content within the IRremote folder.

For manual placement go to

Mac: Applications/Arduino (right mouse click -->show package contents) /Contents/Java/libraries/
Windows: C:\Program Files (x86)\Arduino\libraries\

For resources here is the Github
https://github.com/shirriff/Arduino-IRremote

Here is the IR remote folder

http://www.mediafire.com/download/jd5j7911amju36g/IRremote.zip

Step 6: Recognizing IR Signals

You now need to download the IR decoder sketch.
http://www.mediafire.com/view/6qnsndqp9a838xe/Decode_IR.ino


I totally re-edited the github sketch to make it work. I put all the credits in the sketch. The sketch is attached to this step or you can get if from step 2. Upload this sketch to your Arduino. Now hook up the IR sensor.

The IR sensor's pins are attached to Arduino as so: (from left to right with the sensor's head facing you)


(Vout) Pin 1 to pin 11(Arduino)
(GND) Pin 2 to GND(Arduino)
(Vcc) Pin 3 to 5v(Arduino)


Now open up granola cereal, wait no, I meant serial monitor. Aim your remote at the sensor and press the POWER button. You should see a list of numbers show. Now you can see we got the numbers:

16753245
4294967295
4294967295
4294967295

Notice you if hold down whatever button you were pressing that the second number just repeats itself.

16753245
4294967295
4294967295
4294967295
4294967295
4294967295
4294967295
4294967295

Note what happens if you press another button

16736925
4294967295
4294967295
4294967295
4294967295

You get a different first number, and the same second number!
Obviously, we just need to use the first number. Try hitting different buttons on the remote. You will notice that each different button has a different first number.

So what you need to do is to open up serial monitor press each button, carefully recording the first number. For example: I press the power button and the mode button, so in my text editor program, I'll type,

Power button = 16753245
Mode button = 16736925

And you do this for every button you need!
With this knowledge we can construct some code!

Step 7: Arduino Test Code

Upload this sketch to your Arduino.
http://www.mediafire.com/view/hmv13ynbihed0eg/Test_LED.ino

Step 8: More Code!

So what if we want each button on the remote to do a different function? Making a lot of 'if' statements would be way too much typing! So lets simplify this with a switch/case statement.

switch(results.value)
We are going to put this after the void loop and after the first if statement. Here's the whole thing-

void loop() 
   
  if (irrecv.decode(&results)) 
    {
      irrecv.resume();   // Receive the next value
    }
  
  switch(results.value)
   {
So now we need finish the code. If you don't know what the switch/case are see http://arduino.cc/en/Reference/SwitchCase
Here is the final code. You can keep on adding cases. Now where it says 'case 03' you change the '03' to whatever button number you wish. For instance, the first case could say:

case 16753245: 
// do this
break;
And we just keep on adding different button numbers for to do different things.

Step 9: Conclusion:

I tried to simplify this as much as I can so that you can be controlling your projects with your TV remotes tomorrow evening.
If you don't understand anything please ask me!

Don't forget to give me a vote!

Step 10: Troubleshooting

Sometimes things don't go as planned so I've added a this step to help those that can't get their project to work. I want all reader to be able to experience the awesomeness of this!!! :D

If the value you receive for results.value is letters and numbers like this 'FF01XE' then you must put a 0x in front of that code to get it work work, like this: 0xFF01XE. So you project will read:

