
For those pursuing their CCNA certification, the concept of ACLs is essential to understand. ACLs are one of the security building blocks that network engineers use on a regular basis. Imagine a network that had no restrictions. Traffic could flow in any direction, bad actors could come and go as they please, and the network would be a mess. ACLs are one of the tools that network engineers use to control traffic flow and keep the network secure.

ACLs are used to filter traffic based on a set of rules. These rules can be based on source and destination IP addresses, source and destination ports, and other criteria. ACLs can be used to permit or deny traffic. Denying the traffic results in it getting dropped or put in the Bitbucket. ACLs are used on routers, switches, and firewalls.

### Types of ACLs

There are two types of ACLs: *standard* and *extended*. Standard ACLs are used to filter traffic based on source IP address only. Extended ACLs are used to filter traffic based on source and destination IP addresses, source and destination ports, and other criteria. In most enterprise networks, extended ACLs are used more commonly because they have more things to customize.

#### Standard ACLs

Standard ACLs are used to filter traffic based on *source IP address only.* Standard ACLs are numbered 1 to 99 and 1300 to 1999. The number that you choose for the ACL is important because it determines the order in which the ACL is processed. The router starts from the top of the ACL first and then moves down the list until a matching statement is found. The packet being evaluated by the ACL is dropped when no match exists.

Defining an ACL on a Cisco IOS device does not change the behavior of the device until the ACL is applied, typically to an interface.

What does a [standard ACL](https://www.cisco.com/c/en/us/support/docs/security/ios-firewall/23602-confaccesslists.html#anc38) look like? The syntax options are:

```
access-list <access-list-number> {permit|deny} {host|source source-wildcard|any}
```

For example, if you wanted to create an ACL that allowed traffic from the host `10.10.1.10`, you would use the following command:

```
access-list 10 permit host 10.10.1.10
```

If you wanted to create an ACL that allowed traffic from an entire subnet, you would need to add a wildcard mask. Going into detail about calculating wildcard masks is beyond the scope of this tutorial, but you can read more about them [here](<https://community.cisco.com/t5/networking-knowledge-base/access-control-lists-acl-explained/ta-p/4182349>). For example, if you wanted to create an ACL that allowed traffic from the entire `10.10.5.X` /24 subnet you would use the following command:

```
access-list 10 permit 10.10.5.0 0.0.0.255
```

By default, there is an *implicit deny all* clause as a last statement with any ACL. That implicit clause will deny all traffic that is not explicitly permitted, meaning that you need to decide while drafting your ACLs if you want to implicitly deny all traffic or explicitly allow all traffic that does not fit the listed ACLs. If you want to explicitly allow all other traffic, you can add the following statement at the end of your ACL:

```
access-list 10 permit any
```

#### Extended ACLs

Extended ACLs on Cisco devices are used to filter traffic based on *source and destination IP addresses, source and destination ports, and other criteria.* Extended ACLs are numbered 100 to 199 and 2000 to 2699 or can also be named.

The general syntax structure of an extended ACL is:

```
access-list access-list-number {deny|permit} protocol source source-wildcard destination destination-wildcard
```

For example, you could create an ACL that permits all IP traffic from the source network `10.10.1.0/24` to the destination network `10.10.5.0/24` (using wildcard masks). The second line denies TCP traffic with source IP `10.10.1.0/24` and destination IP `10.10.5.0/24` on port 80 (HTTP). The last line permits all other IP traffic:

```
access-list 1 permit ip 10.10.1.0 0.0.0.255 10.10.5.0 0.0.0.255
access-list 1 deny tcp 10.10.1.0 0.0.0.255 eq 80 10.10.5.0 0.0.0.255
access-list 1 permit ip any any
```

You can also name an extended ACL, but this tutorial is just focusing on numbered ACL basics.

#### Applying the ACL to an Interface

After the ACL is created, it needs to be applied to an interface. The syntax for applying an ACL to an interface is:

```
interface type number
ip access-group {access-list-number | access-list-name} {in | out}
```

For example, if you wanted to apply the standard ACL `1` to the `GigabitEthernet 0/0` interface, you would use the following command:

```
interface GigabitEthernet 0/0
ip access-group 1 in
```

There is a lot more to understanding and mastering ACLs (especially practicing with wildcard masks), but this tutorial is designed to give you a basic understanding of how ACLs work and how to configure them. In the next topic, we will configure your first ACL.
