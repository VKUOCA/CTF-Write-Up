# Dreamhack CTF Season 6 Round #7 (🚩Div1)
**crypto: Spooky Little Ghost**
<br>
- 첨부 파일: 92f7a1c3-345c-4fab-b63f-a4addd2c1cb0.zip

<img width="374" alt="image" src="https://github.com/user-attachments/assets/fa5f6fd8-3341-46c1-9136-e171c31f1860">

> 풀이
<br>

처음 ZIP 파일을 받아 압축을 풀고, deploy 폴더와 Dockerfile 파일을 확인했다. deploy 폴더에는 utils.py, cipher.py, flag, prob.py 파일들이 있었고, 

<img width="437" alt="image" src="https://github.com/user-attachments/assets/77b124ad-49d9-41de-94b2-0c18768674eb">

Dockerfile을 분석한 결과, prob.py에서 플래그를 암호화하고 네트워크를 통해 주고받는 구조임을 파악했다.

prob.py 파일에서는 사용자가 데이터를 암호화하거나 복호화할 수 있는 메뉴가 제공되었다.
```
#! /usr/bin/env python3
import os
from cipher import GHOST
from utils import *

def menu() -> int:
    print('1. encrypt')
    print('2. decrypt')
    print('3. flag')
    print('4. exit')
    return int(input('> '))

if __name__ == '__main__':
    with open('flag', 'rb') as f:
        flag = f.read()

    key = os.urandom(8)
    g = GHOST(key)

    while True:
        i = menu()
        if i == 1:
            msg = input('plaintext(hex)> ')
            enc = g.encrypt(bytes.fromhex(msg))
            print('ciphertext(hex)>', enc.hex())
        elif i == 2:
            enc = input('ciphertext(hex)> ')
            msg = g.decrypt(bytes.fromhex(enc))
            print('plaintext(hex)>', msg.hex())
        elif i == 3:
            new_key = xor_bytes(key, bytes.fromhex('deadbeefcafebabe'))
            new_g = GHOST(new_key)
            enc_flag = new_g.encrypt(flag)
            print('encrypted_flag(hex)> ', enc_flag.hex())
        else:
            break

```
GHOST 암호화 알고리즘을 통해 무작위로 생성된 8바이트 키로 플래그를 암호화하고, XOR 연산(deadbeefcafebabe)을 추가로 적용하는 구조였다. 

ipher.py 파일을 분석해 GHOST 암호화 알고리즘의 32 라운드 블록 암호화 방식을 이해했다. 각 라운드에서 비트 변환과 XOR 연산을 반복해 데이터를 처리하는 방식을 확인했다.
```
from utils import *

class GHOST:
    sbox = (
        (0xC, 0x4, 0x6, 0x2, 0xA, 0x5, 0xB, 0x9, 0xE, 0x8, 0xD, 0x7, 0x0, 0x3, 0xF, 0x1),
        (0x6, 0x8, 0x2, 0x3, 0x9, 0xA, 0x5, 0xC, 0x1, 0xE, 0x4, 0x7, 0xB, 0xD, 0x0, 0xF),
        (0xB, 0x3, 0x5, 0x8, 0x2, 0xF, 0xA, 0xD, 0xE, 0x1, 0x7, 0x4, 0xC, 0x9, 0x6, 0x0),
        (0xC, 0x8, 0x2, 0x1, 0xD, 0x4, 0xF, 0x6, 0x7, 0x0, 0xA, 0x5, 0x3, 0xE, 0x9, 0xB),
        (0x7, 0xF, 0x5, 0xA, 0x8, 0x1, 0x6, 0xD, 0x0, 0x9, 0x3, 0xE, 0xB, 0x4, 0x2, 0xC),
        (0x5, 0xD, 0xF, 0x6, 0x9, 0x2, 0xC, 0xA, 0xB, 0x7, 0x8, 0x1, 0x4, 0x3, 0xE, 0x0),
        (0x8, 0xE, 0x2, 0x5, 0x6, 0x9, 0x1, 0xC, 0xF, 0x4, 0xB, 0x0, 0xD, 0xA, 0x3, 0x7),
        (0x1, 0x7, 0xE, 0xD, 0x0, 0x5, 0x8, 0x3, 0x4, 0xF, 0xA, 0x6, 0x9, 0xC, 0xB, 0x2)
    )

    def __init__(self, key: bytes) -> None:
        self._block_size = 8
        self._round_keys = self._expand_key(key)
        self._state = []

    def _expand_key(self, key: bytes) -> list[list[int]]:
        assert len(key) == 8
        key_bits = bytes_to_bits(key)
        round_keys: list[list[int]] = []
        for i in range(0, 64, 32):
            round_keys.append(key_bits[i:i+32])
        return round_keys

    def _substitution(self, bit32: list[int]) -> list[int]:
        res: list[int] = []
        for i,j in enumerate(range(0,32,4)):
            res += int_to_bits(self.sbox[7-i][bits_to_int(bit32[j:j+4])], 4)
        return res

    def _round_function(self, round_n: int, is_enc: bool) -> None:
        round_key_idx = round_n%2

        state_hi = self._state[:32]
        state_lo = self._state[32:]

        state_lo = add_mod_2_32(state_lo, self._round_keys[round_key_idx])
        state_lo = self._substitution(state_lo)
        state_lo = rol11(state_lo)
        state_lo = xor_lst(state_lo, state_hi)

        if (is_enc and round_n == 31) or (not is_enc and round_n == 0):
            self._state[:32] = state_lo
        else:
            self._state[:32] = self._state[32:]
            self._state[32:] = state_lo  

    def _encrypt(self, plaintext: bytes) -> bytes:
        self._state = bytes_to_bits(plaintext)
        for i in range(32):
            self._round_function(i, is_enc=True)
        return bits_to_bytes(self._state)

    def _decrypt(self, ciphertext: bytes) -> bytes:
        self._state = bytes_to_bits(ciphertext)
        for i in range(31, -1, -1):
            self._round_function(i, is_enc=False)
        return bits_to_bytes(self._state)

    def encrypt(self, plaintext: bytes) -> bytes:
        assert len(plaintext)%self._block_size == 0
        ciphertext  = b''
        for i in range(0, len(plaintext), self._block_size):
            ciphertext += self._encrypt(plaintext[i:i + self._block_size])
        return ciphertext

    def decrypt(self, ciphertext: bytes) -> bytes:
        assert len(ciphertext)%self._block_size == 0
        plaintext  = b''
        for i in range(0, len(ciphertext), self._block_size):
            plaintext += self._decrypt(ciphertext[i:i + self._block_size])
        return plaintext

def test():
    import os
    trial = 0x1000
    for _ in range(trial):
        key = os.urandom(8)
        g = GHOST(key)
        plain = os.urandom(8)
        cipher = g.encrypt(plain)
        assert plain == g.decrypt(cipher)

if __name__ == '__main__':
    test()

```
nc localhost 1112 명령어로 컨테이너에 접속해 prob.py의 메뉴를 통해 암호화된 플래그를 얻었다.

<img width="366" alt="image" src="https://github.com/user-attachments/assets/b1f7769c-b8b3-45e5-ac25-5a0dd1ab9bdc">


암호화된 플래그를 XOR 키 deadbeefcafebabe로 역연산해 복호화를 시도했다. 그러나 복호화된 값은 의미 있는 텍스트 대신 9ae5c59cab93cad2와 같은 16진수 값이었다.

복호화된 값을 다양한 인코딩 방식으로 변환해봤지만, 의미 있는 플래그를 얻지는 못했다. 이를 통해 플래그가 바이너리 형식이거나 추가적인 처리가 필요하다는 점을 알 수 있었다.



 
