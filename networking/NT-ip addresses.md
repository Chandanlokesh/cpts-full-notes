- network located can be identified by the so-called `Media Access Control` address (`MAC`). This would allow data exchange within this one network.
- Addressing on the Internet is done via the `IPv4` and/or `IPv6` address, which is made up of the `network address` and the `host address`.

### IPv4 Structure
- IP addresses is `IPv4`, which consists of a `32`-bit binary number combined into `4 bytes` consisting of `8`-bit groups (`octets`) ranging from `0-255`.

|**Notation**|**Presentation**|
|---|---|
|Binary|0111 1111.0000 0000.0000 0000.0000 0001|
|Decimal|127.0.0.1|
- The `IPv4` format allows 4,294,967,296 unique addresses.
- The IP address is divided into a `host part` and a `network part`.
- The `router` assigns the `host part` of the IP address at home or by an administrator. The respective `network administrator` assigns the `network part`. On the Internet, this is `IANA`, which allocates and manages the unique IPs

| **`Class`** | **Network Address** | **First Address** | **Last Address** | **Subnetmask** | **CIDR**  | **Subnets** | **IPs**        |
| ----------- | ------------------- | ----------------- | ---------------- | -------------- | --------- | ----------- | -------------- |
| `A`         | 1.0.0.0             | 1.0.0.1           | 127.255.255.255  | 255.0.0.0      | /8        | 127         | 16,777,214 + 2 |
| `B`         | 128.0.0.0           | 128.0.0.1         | 191.255.255.255  | 255.255.0.0    | /16       | 16,384      | 65,534 + 2     |
| `C`         | 192.0.0.0           | 192.0.0.1         | 223.255.255.255  | 255.255.255.0  | /24       | 2,097,152   | 254 + 2        |
| `D`         | 224.0.0.0           | 224.0.0.1         | 239.255.255.255  | Multicast      | Multicast | Multicast   | Multicast      |
| `E`         | 240.0.0.0           | 240.0.0.1         | 255.255.255.255  | reserved       | reserved  | reserved    | reserved       |

### Subnet Mask
- further separation of these classes into small networks is done with the help of `subnetting`. This separation is done using the `netmasks`, which is as long as an IPv4 address.

| **Subnetmask** |
| -------------- |
| 255.0.0.0      |
| 255.255.0.0    |
| 255.255.255.0  |
| Multicast      |
| reserved       |
### Network and Gateway Addresses

- The `two` additional `IPs` added in the `IPs column` are reserved for the so-called `network address` and the `broadcast address`.
- the `default gateway`, which is the name for the IPv4 address of the `router`

| **IPs**        |
| -------------- |
| 16,777,214 + 2 |
| 65,534 + 2     |
| 254 + 2        |
| Multicast      |
| reserved       |

### Broadcast Address
- The `broadcast` IP address's task is to connect all devices in a network with each other. `Broadcast` in a network is a message that is transmitted to all participants of a network and does not require any response.

| **Last Address** |
| ---------------- |
| 127.255.255.255  |
| 191.255.255.255  |
| 223.255.255.255  |
| 239.255.255.255  |
| 255.255.255.255  |
### Binary system

```shell-session
Values:         128  64  32  16  8  4  2  1
Binary:           1   1   0   0  0  0  0  0
```

|**Octet**|**Values**|**Sum**|
|---|---|---|
|1st|128 + 64 + 0 + 0 + 0 + 0 + 0 + 0|= `192`|
|2nd|128 + 0 + 32 + 0 + 8 + 0 + 0 + 0|= `168`|
|3rd|0 + 0 + 0 + 0 + 8 + 0 + 2 + 0|= `10`|
|4th|0 + 0 + 32 + 0 + 0 + 4 + 2 + 1|= `39`|
#### IPv4 - Binary Notation

```shell-session
Octet:             1st         2nd         3rd         4th
Binary:         1100 0000 . 1010 1000 . 0000 1010 . 0010 0111
Decimal:           192    .    168    .     10    .     39
```

### CIDR
- `Classless Inter-Domain Routing` (`CIDR`) is a method of representation and replaces the fixed assignment between IPv4 address and network classes (A, B, C, D, E).
- The division is based on the subnet mask or the so-called `CIDR suffix`, which allows the bitwise division of the IPv4 address space and thus into `subnets` of any size.

- Pv4 Address: `192.168.10.39`
    
- Subnet mask: `255.255.255.0`
CIDR: `192.168.10.39/24`
```shell-session
Octet:             1st         2nd         3rd         4th
Binary:         1111 1111 . 1111 1111 . 1111 1111 . 0000 0000 (/24)
Decimal:           255    .    255    .    255    .     0
```
