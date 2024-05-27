# NahamCon CTF 2024
**Forensics: Breath of The Wild-Easy**
<br>
- 첨부 파일: breath-of-the-wild.7z

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/b59b51d4-db09-4e80-9090-489a326bbbca)

> 풀이
먼저, 주어진 파일을 압축 해제한 후 FTK Imager를 통해 해당 파일을 열어보았다. 

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/d3966b5c-b6d7-44ef-9ef8-abbe7fe1f2f3)

'unallocated space'은 할당 되지 않은 공간이다. 할당 되지 않은 공간에 102,400byte? 의심스럽다. 
파일 헤더를 보면 


