# HD Wallet Flow

HD 왈렛이 생성되는 원리

## 니모닉 코드 생성

1. 128(160, 192, 224, 256)길이의 임의의 엔트로피를 생성
2. 1.에서 도출한 엔트로피에 SHA-256을 한 결과의 앞 4(5,6,7,8)비트를 체크섬을 생성
3. 1.에서 도출한 엔트로피의 끝에 체크섬을 붙여서 132(165, 198, 231, 264)비트의 값 생성
4. 3.에서 도출한 값을 11비트 길이의 세그먼트로 나누면 12( 15 ,18 ,21 ,24)개의 세그먼트가 생성됨
5. 4.에서 도출한 세그먼트들에 BIP-39의 단어들과 맵핑

## 니모닉 코드로부터 마스터 시드 생성

1. 니모닉 코드를 이용하여 아래와 같은 PBKDF2 키스트레칭 과정을 거침
  ```sh
  Derived key = PBKDF2(
    Pseudorandom function,
    Password,
    Salt,
    The number of iteration,
    length of Derived key
  )
  ```
  
2. 실제로 시드가 생성되는 방식은 아래와 같음
  ```sh
  Seed = PBKDF2(
    HMAC-SHA512,
    {Mnemonic code words},
    "mnemonic" + {optional passphrase},
    2048,
    512
  )
  ```
  
3. 2.에서 도출된 마스터 시드를 이용하여 프라이빗 키를 생성
