# ESP8266ZabbixSender
Library to realize the zabbix-sender on ESP8266-Arduino

# How to install library to Arduino IDE
## Use git command
(Windows and default arduino setting)

    cd %USERPROFILE%\Documents\Arduino\libraries
    git clone https://github.com/erscl/ESP8266ZabbixSender.git
## Use ZIP import function of the Arduino IDE
Google it

# Usage
    #include <ESP8266ZabbixSender.h>
    
    ESP8266ZabbixSender zSender;
    
    /* WiFi settings */
    String ssid = "AP_SSID";
    String pass = "AP_PASSWD";
    
    /* Zabbix server setting */
    #define SERVERADDR 192, 168, 35, 14  // Zabbix server Address
    #define ZABBIXPORT 10051			 // Zabbix erver Port
    #define ZABBIXAGHOST "IOTBOARD_00"   // Zabbix item's host name
    
    boolean checkConnection();
    
    void setup() {
    	Serial.begin(115200);
    	WiFi.mode(WIFI_STA);
    	WiFi.disconnect();
    	delay(100);
    	WiFi.begin(ssid.c_str(), pass.c_str());
    	while (!checkConnection()) {
    	}
    	zSender.Init(IPAddress(SERVERADDR), ZABBIXPORT, ZABBIXAGHOST);  // Init zabbix server information
    }
    
    void loop() {
    	static int counter1 = 0;
    	static int counter2 = 0;
    	checkConnection();							   // Check wifi connection
    	zSender.ClearItem();						   // Clear ZabbixSender's item list
    	zSender.AddItem("counter1", (float)counter1);  // Exmaple value of zabbix trapper item
    	zSender.AddItem("counter2", (float)counter2);  // Exmaple value of zabbix trapper item
    	if (zSender.Send() == EXIT_SUCCESS) {		   // Send zabbix items
    		Serial.println("ZABBIX SEND: OK");
    	} else {
    		Serial.println("ZABBIX SEND: NG");
    	}
    	counter1 += 1;
    	counter1 *= 2;
    	delay(1000);  // wait 1sec
    }
    
    boolean checkConnection() {
    	int count = 0;
    	Serial.print("Waiting for Wi-Fi connection");
    	while (count < 300) {
    		if (WiFi.status() == WL_CONNECTED) {
    			Serial.println();
    			Serial.println("Connected!");
    			return (true);
    		}
    		delay(500);
    		Serial.print(".");
    		count++;
    	}
    	Serial.println("Timed out.");
    	return false;
    }

Refer to the following repository for more information.  
https://github.com/erscl/esp8266_zabbixtemp
