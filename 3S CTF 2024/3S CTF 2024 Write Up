# 3S CTF 2024 Write Up
Forensics:
<br>
### Wat ch??
- 첨부 파일: wat_ch_.zip

![image](https://github.com/user-attachments/assets/198a2a1e-dc27-49d0-b04a-74405c011388)

> 풀이

1. 어플리케이션 개수

직접 파일의 개수를 확인하기에는 정확도 떨어져 파일 내의 개수를 출력하는 리눅스 명령어를 사용해 확인해 보았다. 
확인 된 결과는 아래와 같다. 

![image](https://github.com/user-attachments/assets/d08cfcfb-47a4-4319-8412-b7504605a79f)

💡 45

<br>

2. 3번째 리마인더 작성 시간(HH:MM:SS)

리마인더가 저장 된 경로를 찾아 확인해보면 데이터베이스 파일이 존재한다.

![image](https://github.com/user-attachments/assets/55cf69d2-78b2-482e-b18f-a7e2c63cf94c)

먼저, 테이블 목록과 각 테이블의 구조를 확인 했다. 

![image](https://github.com/user-attachments/assets/47f4aabd-c8ba-4009-8d9c-364b928afc6a)

테이블의 구조를 보면 파일의 최종 수정 시간을 나타내는 "modtime"이 있는걸 확인할 수 있었다. 

이 "modTime" 값의 파일을 조회해 3번째 리마인더 값을 알아 보았다. 다음 명령어를 실행하기에 위해 아래의 링크를 참고해 작성했다.

'https://like-or-like.tistory.com/62'

<SELECT modTime FROM reminderInstance ORDER BY instanceId LIMIT 1 OFFSET 3;>

이 명령어를 입력해 보았지만 결과 값이 출력이 되지 않았다. 다시 reminderInstance 테이블에 어떤 데이터가 있는지 확인해 보았다.

<SELECT * FROM reminderInstance LIMIT 10;> 

![image](https://github.com/user-attachments/assets/ac0259de-bd04-4e1b-b1b1-d54c416d5b13)

데이터베이스에서 세 번째 리마인더는 "스윙"이며 modTime 값이 1698587148096인 Unix epoch 밀리초로 사람이 읽을 수 있는 형식으로
나타내면, 2023-10-29 22:45:48이다.

💡 22:45:48

![image](https://github.com/user-attachments/assets/c88af6c9-4995-43e0-b0a9-e375141cbc62)

<br>

3. 음악 파일명(@@@.mp3)
음악 파일명은 "Over the Horizon"이며, 아티스트는 Samsung으로 확인이 된다.
파일 경로는 "D:\3S_CTF\Forensic\wat_ch_\apps_rw\com.samsung.w-music-player\data\local_queue_data" 이다.

💡Over the Horizon

<br>

4. 김소리의 거주지(countryName cityName)

작년 SKS 보안 세미나에서 스마트워치 포렌식 주제 내용 중 거주지를 확인할 수 있는 방법이 날씨 앱을 확인하면 사용자의 위치를 확인 할 수 있다는 말씀을 해주신 기억으로...

날씨 앱의 .weather.db 파일의 테이블 목록를 확인해보면, 김소리의 거주지 정보나 관련 데이터를 포함하고 있을 만한 테이블을 확인할 수 있었다.

![image](https://github.com/user-attachments/assets/9c68b93e-b4a4-4db6-affb-1039e9dfc021)

CITY_TABLE에 김소리의 거주지가 있지 않을까? 그래서 CITY_TABLE 조회 해보니 아래와 같이 대한민국 하남시로 확인이 된다.
sqlite3 "D:\3S_CTF\Forensic\wat_ch_\apps_rw\com.samsung.weather\shared\data\db\.weather.db" "SELECT * FROM CITY_TABLE;"

![image](https://github.com/user-attachments/assets/867a4438-422b-4f5c-b7cf-78226a649b7c)

💡대한민국 하남시

<br>

5. 1695650088에 뉴스를 발행한 author

이 문제는 간단하게 DB 열어 살펴 보았다. 

![image](https://github.com/user-attachments/assets/115c3542-f752-48cb-99b9-d59a5d87995a)

💡세계일보

flag 형식: 3S{1_2_3_4_5}

형식에 맞춰 나열해 보면 3S{45_22:45:48_Over the Horizon_대한민국 하남시_세계일보}이다. 
하지만 당일 대회에 flag 값을 확인해 보았지만 틀렸다... 어디서 틀렸을까? 

<br>

###  BrokenHearted
- 첨부 파일: BrokenHearted.zip
  
![image](https://github.com/user-attachments/assets/2b1e7d18-c521-4fc2-bddc-8ae0bc3dc863)

주어진 파일에는 이미지 파일과 텍스트 파일이 있다. 텍스트 파일에 Why don't you search for file carving? 라는 힌트로 
foremost를 사용해 추출해 보았다. 그리고 추출된 파일 형식은 모르기 때문에 all로 입력해 보았다. 
<foremost -t all -i CTF.jpg>

![image](https://github.com/user-attachments/assets/a204afd4-e236-4ea5-83f5-4fef8d2185b6)

분석된 결과, 디렉토리에 output 폴더가 생성된다. 생성된 폴더에는 이미지, 압축된 파일, 텍스트 파일이 있다.

압축된 파일에는 flag.bmp, Hint1.jpg, Hint2.jpg 이미지 파일들이 확인이 된다. 이 파일 중 Hint2.jpg는 이미지 확인이 되지 않는다. 

![image](https://github.com/user-attachments/assets/4bff8c7a-a386-4832-8c54-fd08c943dbb6)

![image](https://github.com/user-attachments/assets/b1aca4c4-4b00-4897-96b9-1804ba20899b)

![image](https://github.com/user-attachments/assets/020b01f4-cb24-4e17-8e02-af2d5742ab0a)

이 파일들을 HxD 툴을 통해 좀 더 알아 본 결과, 각 Hint1,2.jpg 파일에 또 다른 힌트가 있었다. "Maybe..you can search for LSB Steganography..and..Always check the end carefully","you should do is combining LSB"
두 개의 메세지로 봐서는 LSB가 중요한 의미인 것 같다. LSB 스테가노그래피는 파일의 각 바이트의 최하위 비트를 사용하여 데이터를 숨기는 기법이라고 한다. 

flag.bmp의 마지막 헤더에 Look at the different Hex values on lines 00000100 to 00005000. The flag begins with FE and consists of a total of 216 bytes. 라는 메세지가 있다..

![image](https://github.com/user-attachments/assets/e56fb982-9e68-4c35-a066-d158999aeafa)

flag.bmp 파일의 0x100 위치에서 시작하여 216바이트(27줄 * 8바이트)를 추출하면 이 데이터를 통해 숨겨진 메시지를 찾을 수 있을 것 같다.

주어진 조건에 따라 0xFE를 '0'으로, 0xFF를 '1'로 변환한 후 2진수를 ASCII 코드로 변환하는 Python 코드를 작성해 플래그를 찾을 수 있었다. 

![image](https://github.com/user-attachments/assets/7d36d280-0fe4-4e29-91c4-a86312edc53c)

💡Flag: 3S{f06ens1cs_1s_Re6lly_FUn}

<br>

###  DUM DUM :P

- 첨부 파일: dump.zip

![image](https://github.com/user-attachments/assets/d9b499b4-20d6-4b1c-9764-b63981dcf1c0)

binwalk 명령어를 사용하여 dump.bin 파일을 분석해보니 여러 파일들이 있다. 

![image](https://github.com/user-attachments/assets/f5f2ebd0-b922-44f1-a956-f15188049f3c)

이 숨겨진 파일을 binwalk --dd ".*" dump.bin 명령어로 추출을 시도해 보았다.  

![image](https://github.com/user-attachments/assets/f534d8e6-ba66-401c-a128-3bacded0004f)

압축된 파일부터 이미지까지 여러 다양한 파일이 존재한다.
![image](https://github.com/user-attachments/assets/4b3a6b04-d4ee-4e5f-886f-fa5152fdcc54)

압축된 파일을 먼저 확인 해보면, 7개의 텍스트 파일이 있다. 첫번째 파일에서 플래그를 찾을 수 있었다.

![image](https://github.com/user-attachments/assets/d47c2d44-9b43-462c-bc85-99f79f2f453f)

💡Flag: 3S{EA5Y_DUMP_F1L3_G00D_:P}

<br>

### Royal Family
**OSINT:
첨부 파일: Royal_Family.png

![image](https://github.com/user-attachments/assets/07424657-9e1a-445c-b87d-33fa3ca6fab4)

Google에서 이미지로 검색할 수 있다. 이 검색을 통해 비행기의 모델명 airbus a400m atlas임을 알 수 있었다. 

https://www.flyfinland.fi/view/50479 실제로 이 비행기가 7월에 운행이 되었는지 한번 더 확인할 수 있는 정보를 찾았다. 

이 정보로 좀 더 알아 본 결과, 항공사 사이트 마다 시간이 조금씩 오차가 있다.

![image](https://github.com/user-attachments/assets/c807597f-7c29-4d40-aba9-5bf53aa36d3d)

![image](https://github.com/user-attachments/assets/fdb69d68-00eb-4710-afdc-c538cb0266a6)

![image](https://github.com/user-attachments/assets/36a56c7f-e325-49ae-b9d1-aef57e379f93)

이 비행기 시간들을 여러번 조합해 시도 한 끝에 플래그를 찾을 수 있었다. 

![image](https://github.com/user-attachments/assets/bbcfb450-ae59-4800-8257-acbae5fe27c3)

비행기 모델명_Registration_실제 비행 시간_실제 출발시간_실제 도착시간 

💡Flag: 3S{airbusa400matlas_ZM403_01:26_12:49_15:14}








 
