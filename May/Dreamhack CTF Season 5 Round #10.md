# Dreamhack CTF Season 5 Round #10 (🌱Div2)
**crypto: EVER-igen-LEVEL 2**
<br>
- 첨부 파일: 87752e5d-9ff2-4e17-aa89-03473c3c354e

![image](https://github.com/VKUOCA/CTF-Write-Up/assets/128664025/9a375456-626f-4c11-82c9-ddb53103af02)

> 풀이
<br>

압축파일 내에는 output.txt 파일과 prob.py이 존재한다.

먼저, 코드를 살펴보면

'''
import random  
from string import ascii_lowercase, ascii_uppercase, digits  

// 알파벳, 소문자, 대문자, 숫자를 포함하는 문자열을 생성
words = ascii_uppercase + ascii_lowercase + digits

# 비즈네르 암호를 구현하는 클래스를 정의
class Vigenere:
    def __init__(self, key):
        self._key = key

    # 문자를 지정된 거리만큼 이동시키는 함수를 정의
    def shift(self, a, d):
        if a not in words:
            return a
        index = words.index(a)
        return words[(index + d) % len(words)]

    # 평문을 암호화하는 함수를 정의
    def encrypt(self, pt):
        ct = ""
        for i in range(len(pt)):
            ct += self.shift(pt[i], self._key[i % len(self._key)])
        return ct

    # 암호문을 평문으로 복호화하는 함수를 정의
    def decrypt(self, ct):
        pt = ""
        for i in range(len(ct)):
            pt += self.shift(ct[i], -self._key[i % len(self._key)])
        return pt

# 주요 기능을 실행하는 함수
def main():
    # 16개의 난수로 구성된 키를 생성
    key = [random.randint(0, len(words)) for _ in range(16)]
    # 'secret' 파일을 읽어와 내용을 저장
    with open("secret", "r") as f:
        secret = f.read()
        
    assert "Vigenere" and "cipher" in secret
    # 생성한 키를 사용하여 Vigenere 객체를 생성
    cipher = Vigenere(key)
    # 평문을 암호화
    secret_enc = cipher.encrypt(secret)
    # 암호화된 결과를 출력
    print(f"my encrypted sentence > {secret_enc}")

if __name__ == '__main__':
    main()

'''
비즈네르 암호를 사용하여 주어진 평문을 암호화하는 코드인데.... output.txt도 살펴봐야 할 것 같다. 

output.txt
```
my encrypted sentence > 39 2YAx k5LgCy iP Aj9geVQy nEvXnd3Je c kX9P 8z uZ7dbErYRAyegw Ona3Js eRcKyO u7 ffFTl70 TH9ScB – M0HbNuV HDd 4yk TE c3uVU 8Y D1Q ZDNhBpRc 9WY27fjiF – dVWNBP D1Q NAZsM fW bPlBI N3e p8 6i97Nc. FmP HjIjVi 7Zu 4Zth9 bL kYD Gg0yZjc0 dBrYVQ, f8GQ0PD, Bu kGue 8BUlT9FmE0 UDCNp2vQi xd jkfm97 9uDfndFF egcc9CZ 27 mTE 2Y7u A8Zi fM N4Ks3 UVK Dg0. LTAabG 2RSDTqDu GP7QbLq. qy ZE2Xy GUpG kYD eYvEXf DJdMc fE Ep2DTjX4Zt dlk uObyx f DJq7ckZM0 "w8gse0WtBie" (L 4yk) FT LyZkB1 a29Tjc FmIjRSDDd yFQwj QfMvVi. 9I, Tjc0 dHoVj DSc zXfR. Ek{sreSsZxMq8OPf37BwUdvpZKzQ8oNg7Z8rwASqbxQsBl0g935rwDLQaNxNCnxK44g}

```
마지막 줄을 보면, Ek{sreSsZxMq8OPf37BwUdvpZKzQ8oNg7Z8rwASqbxQsBl0g935rwDLQaNxNCnxK44g} Flag형태와 비슷한 문자열이다.

EK{ -> DH{?

비즈네르 암호는 알파벳을 여러 줄에 걸쳐 이동시켜 암호화하는 방법이다. 첫 번째 줄은 1칸, 두 번째 줄은 2칸, 세 번째 줄은 3칸씩 알파벳을 뒤로 이동시켜, 각 줄마다 이동하는 칸 수가 하나씩 증가한다. 자세한 내용은 아래 사이트에서 표를 통해 확인할 수 있다.

ko.wikipedia.org/wiki/%EB%B9%84%EC%A6%88%EB%84%A4%EB%A5%B4_%EC%95%94%ED%98%B8

이후에 어떻게 접근해야 할지 모르겠다..




 
