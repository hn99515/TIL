## BLOCKCHAIN

BLOCK + CHAIN

## CHAIN

Hash Function으로 만든다

## Cryptography

![](C:\Users\multicampus\AppData\Roaming\marktext\images\2023-03-02-09-51-51-image.png)

## SHA256

256은 비트

`Hello SSafy! -> BF21D9C9B5EE25E28C7EADA0F9C709FEC09016A8E0AC3F695530C1B1E205E4C0 Hello SSafy! -> BF21D9C9B5EE25E28C7EADA0F9C709FEC09016A8E0AC3F695530C1B1E205E4C0 Hello SSafy! -> 67FD612D914172387FCCEA7B87013779A732D6ECA794299596E7691C588D4E5F`

검증

## HASH Function

y = f(x)

y = 2x + 1

x가 1이면 y는 3

x가 2이면 y는 5

…

그럼 y가 7이면 f(x)는?

브루트 포싱 : 쓸만한것을 모두 입력해보는 해킹 방법

## 대칭키 암호화 Symmetric Encryption

AES

ARIA, SEED

취약점 : 키가 털리면 다 털림

## 공개키 암호화 Asymmetric Encryption

RSA(소인수분해 이용), ECC

https

잠그는데만 쓰는 공개 키

여는 데만 쓰는 개인 키(비밀 키)

aws 공통때 사용한 암호화 방식 - RSA

요청한 사람한테 온건지 인증하는게 필요해서 전자 서명 생김

## Digital Signature

공개키 알고리즘에 해시함수의 무결성을 더한 알고리즘을 더했다.

취약점 개인 키 관리, 비용과 느리다

ECDSA 타원형 암호화?이용하면

삼성 블록체인 키스토어

안전하게 키 보관

# BLOCKCHAIN NETWORK

인터넷 네트워크

web, P2P, 어둠의 웹

삼겹살 셀프코너

무인 셀프 삼겹살 집

입장하자마자 장부 - 다른 사람들이 소주 먹는다니까 장부 기입함

P2P - 모두가 블록체인의 정보를 가지고 있다 - transaction 주문 발생시 확인 - 소유주, 잔고 등을 확인 후 풀에 들어가고 합의를 하는데 이를 하기 위해 채굴을 한다. 이 연산이 GPU연산이 빠르다. 10분에 1개 CPU중심이었다.

## Data in Block

블록이 100번째인데 80번째 블록의 거래 정보를 해킹하려고 하면 해시가 다 바뀌면서 전체 다 바뀌어서 다 해킹을 진행해야 하기 때문에 이론적으로 블록체인 해킹이 어렵다고 표현

최근 블록은 valid되기 때문에 담기지 않는다

51% consensus → 블록생성 성공 → 정식적인 새로운 블록 생성

## APPLICATIONS

1. 결제
2. 데이터 Storage

암호화폐(송금 내역), 스마트 계약, 물류관리, 지역화폐, 문서관리, 의료정보관리, 저작권관리, 한정판 디지털상품(NFT), 소셜미디어관리, 게임아이템관리, 전자투표, 신원확인 등

1. World Computing

## NFT (Non-Fungible Token)

고유성(Unique)

유일성(Only One)

희소성(A scarcity value)

## Fully Decentralized VS Semi-Decentralized with Cemtralized Proxy

# Summary

1. 블록체인은 보안성, 투명성, 무결성, (탈중앙화)의 특징을 가지고 있고, 이 특징으로 여러가지 어플리케이션을 활용할 수 있다.
2. 블록체인의 네트워크는 기본적으로 (P2P) 네트워크에서 작동한다.
3. 암호화에는 복호화 가능 암호화, 단방향 암호화로 나눠지고, 단방향 암호화는 해시함수로, (SHA256), keccak256등이 있다.
4. 암호화에 서명과 확인을 이용해 메시지의 무결성과 송신자를 보증하는 알고리즘을 (전자서명)이라고 한다.
5. 이 (ECC) 암호화 알고리즘은 공개키 암호화로 타원곡선 함수를 이용하여 암호화한다.
6. ECC + 전자서명 = ECDSA
