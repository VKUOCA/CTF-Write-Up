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

＇Vera Crypt＇프로그램 사용해서 4~5번 마운트를 진행해보았지만 어떠한 정보를 얻을 수 없었다... 내가 마운트를 잘못 했나? 다른 방법을 찾아서 마운트를 다시 진행해보았다. 아! window에 파일을 마운트 하는 방법이 있었다.

다시 시도!
![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/df5339ee-97ee-4d4c-9afe-532224549c71)






참고 링크
https://keys.direct/blogs/blog/how-to-mount-a-drive-in-windows-10






