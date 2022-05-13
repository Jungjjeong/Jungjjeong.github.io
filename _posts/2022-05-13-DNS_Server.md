---
layout: single
title: "[네트워크] 도메인과 호스팅, DNS 서버"
categories: [CS, Network]
last_modified_at: 2022-05-13
excerpt: "도메인과 호스팅, DNS 서버"
header:
  teaser: https://user-images.githubusercontent.com/72294509/168244718-7cdbb136-00bf-483e-9426-4f27b6517e18.png
---

![notion-043](https://user-images.githubusercontent.com/72294509/168244718-7cdbb136-00bf-483e-9426-4f27b6517e18.png){: .align-center}

<br><br><br>

# ⓿ 도메인과 호스팅?

도메인 이름, 호스팅 많이 들어봤지만 두 개념은 완전히 다릅니다.

간단히 말해 **도메인**은 <u>자신의 사이트의 이름을 지정하는 것,</u>

**호스팅**은 <u>내 사이트가 위치하는 공간을 빌리는 것을 지칭</u>합니다.

<br><br>

## ① 호스팅

홈페이지, 사이트들은 결국은 **데이터의 집합체**이며, 이를 <u>물리적으로 저장할 컴퓨터가 필요</u>합니다.

이런 컴퓨터들을 **호스팅 서버**라 하며, 컴퓨터의 공간의 일부를 비용을 받고 빌려주는 것을 **호스팅**이라 합니다.

<br>

## ② 도메인

위에서 살펴봤듯, 호스팅 서버는 인터넷 회선이 연결된 컴퓨터를 지칭합니다.

따라서 `12.345.678.912`와 같은 형태의 **IP 주소**를 가지고 있습니다. 그리고 <span style="background-color:#FFF2D1">이러한 IP 주소가 웹 사이트의 실제 주소</span>입니다.

<br>

하지만 실생활에서 기억하고 접속하기에는 너무 복잡합니다.

따라서 나온 것이 **도메인**입니다.

`ICANN`이라는 기관에 편입된 `IANA`라는 범 국제적인 기관이 조금 더 쓰기 편한 문자로 된 도메인을 만들고 관리하게 되었습니다.

<br>

> <span style="background-color:#FFF2D1">**ICANN**(Internet Corporation for Assigned Names and Numbers)</span><br><br>
> 국제 인터넷 주소 관리 기관<br>
> 인터넷 DNS의 기술적 관리, 인터넷에서의 이름과 주소가 사용되는 방식 규정 정책 담당

> <span style="background-color:#FFF2D1">**IANA**(Internet Assigned Numbers Authority)</span><br><br>
> 인터넷 할당 번호 관리 기관<br>
> 루트 DNS(최상위 도메인), IP 주소 지정 및 기타 인터넷 프로토콜 리소스 담당

<br><br><br>

# ❶ DNS 서버란?

**DNS** : 도메인 이름 시스템 (Domain Name System)

사람이 읽을 수 있는 <span style="background-color:#FFF2D1">**도메인 이름**을 컴퓨터가 읽을 수 있는 **IP 주소**로 <u>변환</u></span>하는 <span style="background-color:#FFF2D1">데이터베이스 시스템</span>입니다. (반대의 경우도 가능합니다.)

즉 웹 사이트의 IP 주소와 도메인 주소를 이어주는 역할을 수행합니다.

이 안에서 <span style="background-color:#FFF2D1"><u>해당 역할을 수행하는 서버</u>를 **DNS 서버**</span> 라고 합니다.

<br>

도메인과 IP 주소는 아무런 설정을 해주지 않으면 아무런 관련이 없습니다.

따라서 DNS 서버에 특정 IP주소는 특정 도메인 이름과 같다는 기록을 저장해주고, 인터넷 사용자들이 도메인 주소를 검색했을 때 IP 주소로 연결되도록 해줍니다.

![image](https://user-images.githubusercontent.com/72294509/167993120-24854813-6130-4b24-ba64-b049a1477492.png){: .align-center}

<br><br>

> <span style="background-color:#FFF2D1">하나의 도메인에 여러 IP?</span>
>
> 로드 밸런싱 기능을 하는 DNS 서버를 중간에 두거나,<br> 로드 밸런서를 이용해서 가능합니다.

> <span style="background-color:#FFF2D1">로드 밸런싱(로드 밸런서)란?</span>
>
> 서버에 부하가 걸리는 것을 분산 처리하는 기술<br> 라운드 로빈, 최소 연결 방식 등의 트래픽 배분 알고리즘 사용.

<br><br><br>

# ❷ DNS 서버 동작 원리

**TLD DNS 서버**를 기준으로 동작 원리를 알아보겠습니다.

1. DNS 서버로 조회를 요청한 <span style="background-color:#FFF2D1">도메인 주소가 전달</span>됩니다.
2. 서버 내부에서 <span style="background-color:#FFF2D1">도메인 주소를 토대로 매칭되는 IP 주소를 찾아냅니다.</span><br>
   (Jiyoung.com = 12.456.789.123)
3. <span style="background-color:#FFF2D1">해당 IP 주소를 갖고 있는 호스팅 서버의 주소를 응답</span>으로 보내줍니다.<br>
   (호스팅 서버 : 해당 웹 사이트 데이터가 저장된 장소)

<br><br>

# ❸. DNS 서버의 종류

도메인의 수는 정말 많습니다.

따라서 **DNS 서버 종류** 또한 <u>계층화되어, 변환 작업을 단계적으로 처리</u>합니다.

![도메인 네임 스페이스](https://i0.wp.com/hanamon.kr/wp-content/uploads/2022/04/%E1%84%83%E1%85%A9%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB-%E1%84%82%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%B7-%E1%84%89%E1%85%B3%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3-Domain-Name-Space.png?resize=768%2C697&ssl=1){: .align-center}

![도메인 종류](https://i0.wp.com/hanamon.kr/wp-content/uploads/2022/04/DNS-%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%80%E1%85%AA%E1%84%8C%E1%85%A5%E1%86%BC.png?resize=2048%2C920&ssl=1){: .align-center}

[1]. <span style="background-color:#FFF2D1">**Root DNS 서버**</span>

- `ICANN이 직접 관리`하는 존엄 서버로 `TLD DNS 서버 IP들을 저장`해두고 안내

[2]. <span style="background-color:#FFF2D1">**TLD(Top Level Domain) DNS 서버**</span>

- `도메인 등록 기관(Registry)이 관리`하는 서버로 `Authoritative DNS 서버 주소를 저장`해두고 안내하는 역할
- 보통 도메인 판매 업체의 DNS 설정이 변경되면 도메인 등록 기관(Registry)로 전달이 되기 때문에 어떤 도메인 묶음이 어떤 Authoritative 서버에 속하는지 알 수 있습니다.

[3]. <span style="background-color:#FFF2D1">**Authoritative(= SLD/ Second Level Domain) DNS 서버**</span>

- `실제 개인 도메인과 IP 주소의 관계가 기록/저장/변경`되는 서버
- 일반적으로 호스팅 업체의 네임 서버를 말하지만, 개인 DNS 서버를 구축한 경우에도 이에 해당합니다.

[4]. <span style="background-color:#FFF2D1">**Recursive DNS 서버(= Resolver)**</span>

- `인터넷 사용자가 가장 먼저 접근하는 DNS 서버`
- 위 3개의 DNS 서버를 매번 거쳐 IP 주소를 받아온다면 매우 비효율적인 작업이므로, 한번 거친 데이터를 일정 기간(TTL/Time to Live) 동안 캐시의 형태로 저장해두는 서버
- 직접 도메인과 IP 주소의 관계를 기록/저장/변경하지 않고 캐시만을 보관
- 대표적으로 `KT/LG/SK와 같은 ISP(통신사) DNS 서버`가 있습니다.

<br><br><br>

# ❹ 전체 동작 원리

![동작원리](https://gentlysallim.com/wp-content/uploads/2021/03/210111_03_2.jpg){: .align-center}

1. 사용자가 `Jiyoung.com` 검색
2. 브라우저에서 `Jiyoung.com`을 사용하고 있는 통신사인 `SK DNS 서버(Recursive DNS 서버)`에게 도메인 주소에 해당하는 IP 주소를 요청 <br>(브라우저의 기본 DNS 설정값은 **ISP DNS 서버**)
3. **ISP DNS 서버**에서 캐시 데이터가 없음을 확인하고 **Root DNS 서버**에게 해당 도메인 네임 조회 요청 <br>(캐시가 있다면 9번으로 건너 뜀)
4. **Root DNS 서버**는 **TLD DNS 서버**만 관리하기 때문에, `.com` 도메인을 관리하는 `TLD DNS 서버 주소` 안내
5. **ISP DNS 서버**는 `.com TLD DNS 서버`에게 어디로 가야하는지 다시 조회 요청
6. **COM TLD DNS 서버**는 `가비아 DNS 서버(Authoritative DNS 서버)`에서 해당 도메인이 관리되고 있음을 조회한 후 다시 안내
7. **ISP DNS 서버**는 **가비아 DNS 서버**에게 다시 조회 요청
8. **가비아 DNS 서버**는 `Jiyoung.com = 12.456.789.123`임을 확인하고 IP를 알려줌. <br>동시에 **ISP DNS 서버**는 <u>해당 정보를 캐시</u>로 기록
9. **ISP DNS 서버**는 IP 주소 `12.456.789.123`을 <u>브라우저에게 안내</u>
10. 브라우저는 `12.456.789.123` IP 주소를 갖고 있는 호스팅 서버에게 웹 사이트 출력 요청 <br>(리소스 요청 및 브라우저 렌더링 과정 거침/ 클라이언트-서버 통신)
11. 웹 사이트 렌더링 완료

<br>

> 2번에서 ISP 서버를 통한 캐시가 있는지 확인하기 전에<br>`브라우저 캐시, OS 캐시, 라우터 캐시도 확인`한 후 ISP 캐시를 확인하게 됩니다.

<br><br>

---

### 출처

[gentlysallim-DNS는 무엇이고 네임서버는 무엇일까](https://gentlysallim.com/dns%EB%9E%80-%EB%AD%90%EA%B3%A0-%EB%84%A4%EC%9E%84%EC%84%9C%EB%B2%84%EB%9E%80-%EB%AD%94%EC%A7%80-%EA%B0%9C%EB%85%90%EC%A0%95%EB%A6%AC/)

[gentlysallim-DNS와 호스팅](https://gentlysallim.com/%eb%8f%84%eb%a9%94%ec%9d%b8-%ed%98%b8%ec%8a%a4%ed%8c%85-%eb%ad%90%ea%b0%80-%eb%8b%a4%eb%a6%84-%ea%b0%9c%eb%85%90-%ec%a0%95%ec%9d%98-%ec%95%8c%ec%95%84%eb%b3%b4%ea%b8%b0/)

[Google domains](https://support.google.com/domains/answer/4544245?hl=ko)

[hanamon- 도메인 네임 시스템 개념부터 작동 방식까지](https://hanamon.kr/dns%eb%9e%80-%eb%8f%84%eb%a9%94%ec%9d%b8-%eb%84%a4%ec%9e%84-%ec%8b%9c%ec%8a%a4%ed%85%9c-%ea%b0%9c%eb%85%90%eb%b6%80%ed%84%b0-%ec%9e%91%eb%8f%99-%eb%b0%a9%ec%8b%9d%ea%b9%8c%ec%a7%80/)
