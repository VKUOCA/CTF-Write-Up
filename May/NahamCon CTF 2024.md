# NahamCon CTF 2024
**Forensics: Breath of The Wild-Easy**
<br>
- 첨부 파일: breath-of-the-wild.7z

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/b59b51d4-db09-4e80-9090-489a326bbbca)

> 풀이
<br>
먼저, 주어진 파일을 압축 해제한 후 FTK Imager를 통해 해당 파일을 열어보았다. 

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/d3966b5c-b6d7-44ef-9ef8-abbe7fe1f2f3)

'unallocated space'은 할당 되지 않은 공간이다. 할당 되지 않은 공간에 102,400byte? 의심스럽다. 
파일 헤더를 보면 vhd file로 확인이 된다. 이후에 어떻게 풀어야 할지 몰라서 구글링을 해보았다.

가상 하드 디스크를 만들고 VHD파일을 마운트를 해줘야 된다는데... 일단 해보자!

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/d707eb93-9c26-4a1e-93a6-61dc3d22083c)

＇Vera Crypt＇프로그램 사용해서 4~5번 마운트를 진행해보았지만 어떠한 정보를 얻을 수 없었다... 내가 마운트를 잘못 했나? 다른 방법을 찾아서 마운트를 다시 진행해보았다. 아! window에서 파일을 마운트 하는 방법이 있었다.

다시 시도!

<img width="410" alt="image" src="https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/4729b759-c442-44e3-936d-000553033beb">
<img width="656" alt="image" src="https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/cb9e6363-86fd-4000-8346-e6d157272e28">

마운트를 진행해주고, 암호를 입력하라는 창에 문제에서 제시된 'videogames'입력 해주면 된다. 

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/3d2f7efc-33e2-4b9e-826f-0a28711df886)

여러 이미지가 존재하는데.. 자세한건 Autopsy 프로그램 사용해서 다운로드를 진행한 URL 알아봐야 할 것 같다. 마지막 스터디에서 배운 프로그램을 여기서 쓰네 ㅎㅎ

<img width="917" alt="image" src="https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/3c83b340-0857-40d8-a2eb-5c1a9345e6c0">

경로가 다 같은 걸로 확인이 된다? 아..! 아니네 아래로 스크롤을 내려보면 URL이 다른 하나가 존재한다. 

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/583d4b6f-1ac2-4889-a83f-1510729c741b)

```
HostUrl=https://www.gamewallpapers.com/wallpapers_slechte_compressie/01wallpapers/&#102;&%23108;&%2397;&%23103;&%23123;&%2356;&%2351;&%23102;&%2350;&%2398;&%2348;&%2397;&%2356;&%2399;&%23101;&%2351;&%2357;&%23102;&%2350;&%23101;&%2353;&%2398;&%2397;&%2349;&%23100;&%2354;&%2399;&%2355;&%2348;&%23101;&%2357;&%2355;&%23102;&%2350;&%2357;&%2349;&%23101;&%23125;

```
 URL 인코딩 문자들을 URL 디코딩으로 변환된 URL을 다시 원래의 형태로 되돌려야 될 것 같다. 

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/1f47990b-3f78-4880-ac07-7eab0d18aa31)

```
&#102;&#108;&#97;&#103;&#123;&#56;&#51;&#102;&#50;&#98;&#48;&#97;&#56;&#99;&#101;&#51;&#57;&#102;&#50;&#101;&#53;&#98;&#97;&#49;&#100;&#54;&#99;&#55;&#48;&#101;&#57;&#55;&#102;&#50;&#57;&#49;&#101;&#125;

```

HTML 문자 참조로 인코딩된 문자열을 디코딩하면 원래 문자열로 변환 되면서 플래그 확인!!!!!!!!!!

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/6a4572a5-269b-45f8-b4f3-66d6ce82bfa8)

💡 flag{83f2b0a8ce39f2e5ba1d6c70e97f291e}

















참고 링크
https://keys.direct/blogs/blog/how-to-mount-a-drive-in-windows-10






