
<div align="center">
<h1>Google Firebase + Raspberry Pi</h1>


<img src="rpi_fire.jpg" width="50%">

</div>

<h2>Parte 1: </h2> https://www.youtube.com/watch?v=Dbi7IgLoC1s&t=988s


<h3>HTML Javascript</h3>


```html
<pre>
<code>

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Firebase Led</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
    <script src="https://www.gstatic.com/firebasejs/3.6.1/firebase.js"></script>
    <meta name="author" content="Jefferson Rivera">
    <script>
      // Initialize Firebase
      var config = {
        apiKey: "AIzaSyD2QEHR9JO2KcrtQvjypI6iFIznX_Dc_Wg",
        authDomain: "testled-eb6bb.firebaseapp.com",
        databaseURL: "https://testled-eb6bb.firebaseio.com",
        storageBucket: "testled-eb6bb.appspot.com",
        messagingSenderId: "909652007958"
      };
      firebase.initializeApp(config);
    </script>
    </head>
  <body>
<br>
<br>
<br>
  <div class="container">
    <div class="row">
      <div class="col-md-6 col-md-offset-3">
        <div class="panel panel-success">
          <div class="panel-heading">Firebase Led</div>
          <div class="panel-body">
             <div class="checkbox">
               <label>
                <input type="checkbox" id="checkLed1">LED 1
              </label>
              <div class="checkbox">
               <label>
                <input type="checkbox" id="checkLed2">LED 2
              </label>
             </div>
          </div>
        </div>      
      </div>
    </div>
  </div>
  <script>
	/*
	  //Esqueleto de la BD
	  var db = firebase.database().ref('home').set({
	    led1:true,
	    led2:true
	  });
	*/

	var db = firebase.database().ref('home');
	// último estado

	db.on('value', function(data){
	   $("#checkLed1").prop('checked', data.val().led1);
	   $("#checkLed2").prop('checked', data.val().led2);
	});


	$("#checkLed1").click(function(){
	  var estado = $(this).is(':checked');
	  db.update({
	    led1:estado
	  });
	});

	$("#checkLed2").click(function(){
	  var estado = $(this).is(':checked');
	  db.update({
	    led2:estado
	  });
	});

  </script>
  </body>
</html>

</code>
</pre>
```
<hr>
<h3>Instalando lo necesario en Python</h3>
<pre>
<code>
$ sudo apt-get update
$ sudo apt-get install python-dev


$ wget https://bootstrap.pypa.io/get-pip.py
$ sudo python get-pip.py

$ sudo pip install pyrebase
</code>
</pre>


<h3>Código Python</h3>
<pre>
<code>
#Desarrollador por Jefferson Rivera
#riverajeferàgmail.com
import pyrebase
import RPi.GPIO as GPIO
from time import sleep

config = {
  "apiKey": "AIzaSyD2QEHR9JO2KcrtQvjypI6iFIznX_Dc_Wg",
  "authDomain": "testled-eb6bb.firebaseapp.com",
  "databaseURL": "https://testled-eb6bb.firebaseio.com",
  "storageBucket": "testled-eb6bb.appspot.com",
}

firebase = pyrebase.initialize_app(config)

db = firebase.database()

GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)
GPIO.setup(17, GPIO.OUT)
GPIO.setup(27, GPIO.OUT)


print "**********   INICIO  *************"

while True:
    salidaLed1 = db.child("home/led1").get()
    GPIO.output(17, salidaLed1.val())

    salidaLed2 = db.child("home/led2").get()
    GPIO.output(27, salidaLed2.val())
    sleep(1)

GPIO.cleanup()

</code>
</pre>

<h3>Plano</h3>
<div align="center">
<img src="plano.jpg" width="50%">
</div>


<h3>En el navegador</h3>
<div align="center">
<img src="test.png" width="50%">
<br>
<a href="https://testled-eb6bb.firebaseapp.com/" target="_blank">https://testled-eb6bb.firebaseapp.com/</a>
</div>


