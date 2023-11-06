#include <ThingSpeak.h>
#include <WiFi.h>
#include <WebServer.h>

const char* ssid = "wanted";  // Enter your SSID here
const char* password = "9907224577";  //Enter your Password here

//Thingspeak config
String request_string;
unsigned long myChannelNumber = 2;
String myAPIkey = "4RZEBLQAJ2LK0J9X";
String thingSpeakAddress = "api.thingspeak.com";

WiFiClient client;

WebServer server(80); 

String HTML = "<html><head><title>Guardian Scout</title><style>body{background-color:#f0f0f0;font-family:Arial,sans-serif;text-align:center;display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh;}input[type=\\\"text\\\"]{display:none;}#splitValues{text-align:center;margin:20px;}.value-box{display:inline-block;padding:10px;font-size:18px;color:#333;margin:5px;}.red{background-color:red;width:80px;height:40px;}.green{background-color:green;width:80px;height:40px;}.light-blue{background-color:lightblue;width:100px;height:60px;}.top-center{position:absolute;top:100px;left:50%;transform:translateX(-50%);}.middle-left{position:absolute;top:50%;left:0;transform:translateY(-50%);}.middle-right{position:absolute;top:50%;right:0;transform:translateY(-50%);}.bottom-center{position:absolute;bottom:0;left:50%;transform:translateX(-50%);}</style></head><body><input type=\\\"text\\\" value=\\\"1,1,1,0,plane\\\"><div id=\\\"splitValues\\\"></div><script>const inputElement=document.querySelector('input[type=\\\"text\\\"]');const splitValuesContainer=document.getElementById('splitValues');const inputValue=inputElement.value;const values=inputValue.split(',');splitValuesContainer.innerHTML='';values.forEach((value,index)=>{const valueBox=document.createElement('div');valueBox.textContent=value==='1'?'Yes':value==='0'?'No':value;valueBox.className=value==='1'?'value-box red':value==='0'?'value-box green':'value-box light-blue';if(index===0){valueBox.classList.add('top-center');}else if(index===1){valueBox.classList.add('middle-left');}else if(index===2){valueBox.classList.add('middle-right');}else if(index===3){valueBox.classList.add('bottom-center');}splitValuesContainer.appendChild(valueBox);});</script></body></html>";


//String HTML = "<html><head><title>Guardian Scout</title><style>body{background-color:#f0f0f0;font-family:Arial,sans-serif;text-align:center;display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh;}input[type=\"text\"]{display:none;}#splitValues{text-align:center;margin:20px;}.value-box{display:inline-block;padding:10px;font-size:18px;color:#333;margin:5px;}.red{background-color:red;width:80px;height:40px;}.green{background-color:green;width:80px;height:40px;}.light-blue{background-color:lightblue;width:100px;height:60px;}.top-center{position:absolute;top:100px;left:50%;transform:translateX(-50%);}.middle-left{position:absolute;top:50%;left:0;transform:translateY(-50%);}.middle-right{position:absolute;top:50%;right:0;transform:translateY(-50%);}.bottom-center{position:absolute;bottom:0;left:50%;transform:translateX(-50%);}</style></head><body><input type=\"text\" value="\1,1,1,0,plane\"><div id=\"splitValues\"></div><script>const inputElement=document.querySelector('input[type=\"text\"]');const splitValuesContainer=document.getElementById('splitValues');const inputValue=inputElement.value;const values=inputValue.split(',');splitValuesContainer.innerHTML='';values.forEach((value,index)=>{const valueBox=document.createElement('div');valueBox.textContent=value==='1'?'Yes':value==='0'?'No':value;valueBox.className=value==='1'?'value-box red':value==='0'?'value-box green':'value-box light-blue';if(index===0){valueBox.classList.add('top-center');}else if(index===1){valueBox.classList.add('middle-left');}else if(index===2){valueBox.classList.add('middle-right');}else if(index===3){valueBox.classList.add('bottom-center');}splitValuesContainer.appendChild(valueBox);});</script></body></html>";

const int trigPin1 = 13;
const int echoPin1 = 12;
const int trigPin2 = 14;
const int echoPin2 = 27;
const int trigPin3 = 15;
const int echoPin3 = 4;
const int trigPin4 = 5;
const int echoPin4 = 18;
const int trigPin5 = 19;
const int echoPin5 = 21;


//define sound velocity in cm/uS
#define SOUND_VELOCITY 0.034
#define CM_TO_INCH 0.393701

long duration1;
long duration2;
long duration3;
long duration4;
long duration5;
float distanceCm1;
float distanceCm2;
float distanceCm3;
float distanceCm4;
float distanceCm5;

bool front=false,left=false,right=false,back=false;
String down="plane"; 

float height=170;
String data="Deafult text";
const int buzzer = 22;
void setup() {
Serial.begin(9600); // Starts the serial communication
pinMode(trigPin1, OUTPUT); // Sets the trigPin1 as an Output
pinMode(echoPin1, INPUT); // Sets the echoPin1 as an Input
pinMode(trigPin2, OUTPUT); // Sets the trigPin1 as an Output
pinMode(echoPin2, INPUT);
pinMode(trigPin3, OUTPUT); 
pinMode(echoPin3, INPUT);
pinMode(trigPin4, OUTPUT); 
pinMode(echoPin4, INPUT);
pinMode(trigPin5, OUTPUT); 
pinMode(echoPin5, INPUT);
pinMode(echoPin5, INPUT);
 pinMode(buzzer, OUTPUT);


//wifi setup
 WiFi.begin(ssid, password);
   // Check wi-fi is connected to wi-fi network
  while (WiFi.status() != WL_CONNECTED) {
  delay(1000);
  Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected successfully");
  Serial.print("Got IP: ");
  Serial.println(WiFi.localIP());  //Show ESP32 IP on serial

  server.on("/", handle_root);

  server.begin();
  Serial.println("HTTP server started");
  delay(100); 
}
void loop() {
  server.handleClient();
// Clears the trigPin1
digitalWrite(trigPin1, LOW);

delayMicroseconds(2);             
digitalWrite(trigPin1, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin1, LOW);

duration1 = pulseIn(echoPin1, HIGH);

distanceCm1 = duration1 * SOUND_VELOCITY/2;
digitalWrite(trigPin2, LOW);
delayMicroseconds(2);
digitalWrite(trigPin2, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin2, LOW);
duration2 = pulseIn(echoPin2, HIGH);
distanceCm2 = duration2 * SOUND_VELOCITY/2;

digitalWrite(trigPin3, LOW);
delayMicroseconds(2);
digitalWrite(trigPin3, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin3, LOW);
duration3 = pulseIn(echoPin3, HIGH);
distanceCm3 = duration3 * SOUND_VELOCITY/2;

digitalWrite(trigPin4, LOW);
delayMicroseconds(2);
digitalWrite(trigPin4, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin4, LOW);
duration4 = pulseIn(echoPin4, HIGH);
distanceCm4 = duration4 * SOUND_VELOCITY/2;


digitalWrite(trigPin5, LOW);
delayMicroseconds(2);
digitalWrite(trigPin5, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin5, LOW);
duration5 = pulseIn(echoPin5, HIGH);
distanceCm5 = duration5 * SOUND_VELOCITY/2;

// checking surounding area
    if(distanceCm2<=150 )
    {
       Serial.println("\n Some thing is in front");
       for(int i=1;i<10;i++)
       {
       tone(buzzer, 200*i);
       delay(20);
       noTone(buzzer);
       delay(20);
       
       }
                 
            
       front=true;
    }else
    {
      front=false;
      
    }
    if(distanceCm3<=150 )
    {
       Serial.println("\n Some thing in in left! ");
       left=true;
    }else{
      left=false;
    }
    if(distanceCm4<=150 )
    {
       Serial.println("\n Some thing is in right! ");
       right=true;
    }else{
      right=false;
    }
    if(distanceCm5<=150 )
    {
       Serial.println("\n Some thing is in the back ");
       back=true;
    }else{
      back=false;
    }
    
//Getting vertical value
    if(distanceCm1<=(height-5) && distanceCm1>=(height-30))
    {
      Serial.println("\n up Stairs ahead ");
      down="upstair";
    }else if(distanceCm1>=(height+5) && distanceCm1<=(height+25) )
    {
      Serial.println("\n down Stairs ahead ");
      down="downstair";
    }else if(distanceCm1>=(height+25))
    {
       tone(buzzer, 1000); // Send 1KHz sound signal...
               delay(1000);        // ...for 1 sec
            noTone(buzzer);
      Serial.println("\n pit ahead");
      down="pit";
    }else if( distanceCm1<=(height-30)){
         down="wall";
    }else{
      down="plane";
    }

///we have to send front,left,right,back,and down 
Serial.println("___");
String comma=",";
 data=front+comma+left+comma+right+comma+back+comma+down;
 HTML = "<html><head><title>Guardian Scout</title><style>body{background-color:#f0f0f0;font-family:Arial,sans-serif;text-align:center;display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh;}input[type=\"text\"]{display:none;}#splitValues{text-align:center;margin:20px;}.value-box{display:inline-block;padding:10px;font-size:18px;color:#333;margin:5px;}.red{background-color:red;width:80px;height:40px;}.green{background-color:green;width:80px;height:40px;}.light-blue{background-color:lightblue;width:100px;height:60px;}.top-center{position:absolute;top:100px;left:50%;transform:translateX(-50%);}.middle-left{position:absolute;top:50%;left:0;transform:translateY(-50%);}.middle-right{position:absolute;top:50%;right:0;transform:translateY(-50%);}.bottom-center{position:absolute;bottom:0;left:50%;transform:translateX(-50%);}</style></head><body><input type=\"text\" value=\""+data+"\"><div id=\"splitValues\"></div><script>const inputElement=document.querySelector('input[type=\"text\"]');const splitValuesContainer=document.getElementById('splitValues');const inputValue=inputElement.value;const values=inputValue.split(',');splitValuesContainer.innerHTML='';values.forEach((value,index)=>{const valueBox=document.createElement('div');valueBox.textContent=value==='1'?'Yes':value==='0'?'No':value;valueBox.className=value==='1'?'value-box red':value==='0'?'value-box green':'value-box light-blue';if(index===0){valueBox.classList.add('top-center');}else if(index===1){valueBox.classList.add('middle-left');}else if(index===2){valueBox.classList.add('middle-right');}else if(index===3){valueBox.classList.add('bottom-center');}splitValuesContainer.appendChild(valueBox);});setTimeout(()=>{window.location.reload(1)},500);</script></body></html>";


   Serial.println("down:"+down+" front:"+front+" Left:"+left+" right:"+right+" back:"+back); 
Serial.print("device1 distance :");
Serial.println(distanceCm1);
Serial.print("device2 distance :");
Serial.println(distanceCm2);
Serial.print("device3 distance :");
Serial.println(distanceCm3);
Serial.print("device4 distance :");
Serial.println(distanceCm4);
Serial.print("device5 distance :");
Serial.println(distanceCm5);

// to push data in ThingSpeak servers
  if (client.connect("api.thingspeak.com", 80)) {
    request_string = "/update?";
    request_string += "api_key=";
    request_string += myAPIkey;
    request_string += "&";
    request_string += "field1";
    request_string += "=";
    request_string += left;
    request_string += "&";
    request_string += "field2";
    request_string += "=";
    request_string += right;
    request_string += "&";
    request_string += "field3";
    request_string += "=";
    request_string += front;
    request_string += "&";
    request_string += "field4";
    request_string += "=";
    request_string += distanceCm3;
    request_string += "&";
    request_string += "field5";
    request_string += "=";
    request_string += back;
//    Serial.println(String("GET ") + request_string + " HTTP/1.1\r\n" +
//                 "Host: " + thingSpeakAddress + "\r\n" +
//                 "Connection: close\r\n\r\n");
    client.print(String("GET ") + request_string + " HTTP/1.1\r\n" +
                 "Host: " + thingSpeakAddress + "\r\n" +
                 "Connection: close\r\n\r\n");

  }

}

void handle_root() {
  Serial.print(data);
  server.send(200, "text/html", HTML);
}
