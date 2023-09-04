## 가상IP와 NAT

### 가상IP (Virtual IP)

- 외부와의 IP 통신을 위해 발급받는 가상의 IP 주소

### NAT

- 내부 / 외부간의 네트워크 통신을 위해 IP 주소를 변환해주는 작업

## SNAT과 DNAT

### SNAT (source NAT)

- 내부 -> 외부로 통신할때, 내부의 정보를 노출하기 않지 위해 사설 IP -> 공인 IP로 변환하는 작업

### DNAT (destination NAT)

- 외부 -> 내부로 통신할때 내부의 리소스로 리디렉션하기 위해 공인 IP -> 사설 IP로 변환하는 작업

##  활용경험

- WEB - WAS 를 각각 2개씩 구성하여 트래픽을 처리하는 Application에서 
  호출시에는 하나의 IP를 사용해서 내부Application에 request를 보내야하기 때문에, **DNAT**이 필요함.
- DNAT를 사용하려면 공인 IP 주소인 **Virtual IP** 가 우선적으로 필요하다. 

