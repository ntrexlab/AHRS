# AHRS_RS232_Arduino

## **Manual**
***
* ### 1.HardWare
    * #### 사용모델
         - AHRS - [MW_AHRSv1](http://www.devicemart.co.kr/goods/view?no=1310790)
         - Max232 - [MAX232 RS232 to TTL 모듈](http://www.devicemart.co.kr/goods/view?no=1064136)
         - DSub 커넥터 - [DS1033_09M(MALE)](http://www.devicemart.co.kr/goods/view?no=286)
         - Arduino - [Arduino Mega 2560](http://www.devicemart.co.kr/goods/view?no=34405)
         - JumperCable - [CH254(M/F)](http://www.devicemart.co.kr/goods/view?no=1321195)

        ![Ahrs_C](https://user-images.githubusercontent.com/85467544/120950931-bc137600-c782-11eb-9efa-0ca270e94458.png)

        
    * #### AHRS_USB_TTL 
         - CP2102 Micro USB to TTL 컨버터 모듈 - [SZH-CVBE-037](http://www.devicemart.co.kr/goods/view?no=1326839)
         - MAX3232 초소형 TTL to RS232 컨버터 모듈 - [SZH-CVBE-011](http://www.devicemart.co.kr/goods/view?no=1324909)

        ![USBTTL](https://user-images.githubusercontent.com/85467544/120979105-33133380-c7b0-11eb-9055-7e11b6c82ead.png)
***


* ### 2.Tool
    * #### 사용버전
        - Arduino - Arduino-1.8.15-window [설치경로](https://www.arduino.cc/en/software)
        - AHRS_UI_180808 [설치경로-관련자료](https://www.devicemart.co.kr/goods/view?no=1310790)
***

* ### 3. 개발 순서
    #### 1. AHRS_USB_TTL을 제작한다. 
    #### 2. AHRS_UI_180808을 설치한다.
    #### 3. AHRS_USB_TTL을 PC에 연결 후 AHRS_UI_180808을 실행한다.
    ![ahrs연](https://user-images.githubusercontent.com/85467544/120984205-660bf600-c7b5-11eb-887b-e4761f6ebaf7.png)
    #### 4. Connect 버튼(1)을 클릭한다.
    #### 5. Connection(2) 창이 뜨면 COM PORT의 연결 USB 번호를 설정한다.
    #### 6. Baudrate를 115200으로 설정한 후 Connect 버튼을 클릭한다.
    ![AHRS설](https://user-images.githubusercontent.com/85467544/120985366-825c6280-c7b6-11eb-905f-409e5ce0bf80.png)
    #### 7. Configure(3) 버튼을 클릭한다.
    #### 8. RS-232 Bandrate(4) 확인한다.
    #### 9. Transmission Period(5)의 값을 지정한다. -> 센서 값을 받는 간격을 설정(EX - 1000-> 1초) 
    #### 10. Select RS232 Data Type(6) 확인한다. -> 체크 상태면 센서 값을 계속 보내게 되어 해제해야 키워드 요청시 센서 값 확인 가능하다.
    #### 11. Arduino_Hardware를 제작한다.(Hardware)
    #### 12. Arduino 프로그램을 설치한다.
    ![아두이노](https://user-images.githubusercontent.com/85467544/121102517-bbd5b200-c838-11eb-9cf4-ec825570feec.png)
    #### 13. 11번에서 제작한 Hardware를 연결 후 보드,프로세서,포트를 제작한 하드웨어에 맞게 설정한다.
    #### 14. [코드](https://github.com/ntrexlab/AHRS/blob/main/AHRS_Serial_Text/AHRS_Serial_Text.ino)를 작성한다.
    
    
    ```c
    char inputData;
    char ahrsData;  
    String ahrsString = "";         
    boolean stringCheck = false;
 
    void setup(){

        Serial.begin(115200); //아두이노 통신
        Serial1.begin(115200); //아두이노와 Ahrs 통신, 하드웨어 제작 시 RX,TX를 19,18핀의 연결하여 Serial1을 사용.
   
    }
 
    void loop(){  

        if (stringCheck) {  //stringCheck가 true 값이면 실행.
            Serial.println(ahrsString); //ahrsString 값 화면으로 출력.
            ahrsString = ""; //ahrsString 초기화 
            stringCheck = false; //stringCheck를 false 값으로 저장.
 
        } 
    }

    void serialEvent(){
      
        while(Serial.available()){    //Serial 입력이 있으면 실행.
        inputData =(char)Serial.read(); //inputData를 입력 값으로 저장.
        Serial1.write(inputData);   //저장 값을 센서로 전송.
        }
    }

    void serialEvent1(){
           
        while(Serial1.available()) { //센서의 전송 값이 있으면 실행.
            ahrsData = (char)Serial1.read(); //ahrsData를 전송 값의 맞는 센서 값으로 저장.
            ahrsString += ahrsData;
            if (ahrsData == '\n') {  //ahrsData에 Null 값이 있으면 실행.
                stringCheck = true;  //stringCheck를 true 값으로 저장.
            }
        }
    }
    ```
***



