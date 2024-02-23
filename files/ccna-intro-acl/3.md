
In real-life networking scenarios, creating ACLs and updating them as the business changes is a common occurrence. Sometimes, you need to open up ports for a new development lab for the business, or you need to restrict access because of security concerns. Implementing the wrong ACL is a common cause of network outages, so learning how they operate and what they do is important. Playing around in the lab is the best place to learn how ACLs work, so let's get started.

#### Step 1: Defining Your Requirements

In a typical network engineering environment ACLs do not drop out of thin air. You need to discuss with your relevant stakeholders about what they are trying to accomplish and narrow it down to which source and IP addresses they want to work with and which ports are being used.

ACLs are also sometimes used to enable access to your own network devices and lock access to other potential bad actors. The example we are working with in this tutorial will follow that scenario. We will create an ACL that allows host `H1` to access the hosts in the other subnet (`H3` and `H4`) but deny access to the other hosts in the same subnet (`H2`). This approach could be for security reasons or because the other hosts are not allowed to access the other subnet for business reasons.

In your Cisco Modeling Labs topology, you have four hosts:

- `H1` with an IP address of `172.16.10.11`
- `H2` with an IP address of `172.16.10.21`
- `H3` with an IP address of `172.16.20.31`
- `H4` with an IP address of `172.16.20.41`

There are also two switches, one for each subnet; `access-sw1` handles the 172.16.10.0 /24 subnet (`H1` and `H2`), and `access-sw2` handles the 172.16.20.0 /24 subnet (`H3` and `H4`). The ACLs will be applied to the `router1` router.

#### Step 2: Verifying Connectivity

Before you start creating ACLs, you need to verify that you have connectivity between the hosts. This will help you verify that your ACLs are working as expected. Because our goal is to have `H1` be able to access `H3` and `H4`, we need to verify that `H1` can ping `H3` and `H4` before we start creating ACLs.

1. Log in to `H1` and ping `H3` and `H4` to verify connectivity. You can do this by clicking the `H1` host icon in the topology and then clicking the **Console** button, and then **Open Console**. The login credentials will be `cisco` and `cisco` for all the devices in the topology.

To verify ping connectivity, you can issue the `ping` command with the IP address of the other hosts. For example, to ping `H3`, you would issue the following command:

```
ping 172.16.20.31
```

You are pinging from a Linux host for all our ping tests, so you will *need to press* **CTRL** + **C** to stop the ping (after a few seconds).

To ping `H4`, issue the following command:

```
ping 172.16.20.41
```

2. Log in to `H2` and ping `H3` and `H4` to verify connectivity. You can do this by clicking the `H2` host icon in the topology and then clicking the **Console** button, and then **Open Console**.

To ping `H3`, issue the following command:

```
ping 172.16.20.31
```

Again, because you are pinging from a Linux host for all our ping tests, you will need to press **CTRL** + **C** to stop the ping (after a few seconds).

To ping `H4`, issue the following command:

```
ping 172.16.20.41
```

> **Note:** We are not going to test connectivity from `H3` and `H4` to `H1` and `H2` because this tutorial is focused on ACLs (just on one side of the network) and not routing. We are assuming that the routing is already in place and working.

#### Step 3: Creating Your ACL

Now that you have verified connectivity, you can start creating your ACL. In this example, we are going to create a standard ACL that will allow `H1` to access `H3` and `H4` but deny access to `H2`. This will be accomplished by permitting access to `H1`; there is an implicit deny all at the end of the ACL, meaning that all other traffic will be denied (specifically `H2`).

1. Log in to `router1` and enter global configuration mode. You can do this by clicking the `router1` icon in the topology and then clicking the **Console** button, and then **Open Console**.

First, enter enable mode and then configuration mode:

```
enable
conf t
```

Next, you will create your ACL. Because we are creating a standard ACL, we will use the `access-list` command with a number between 1 and 99. We will use `1` for this example. The syntax for the command is:

```
access-list 1 permit host 172.16.10.11
```

Because it is just a single host, we can use the `host` keyword before the IP address for `H1`. If we wanted to use a subnet, we would need to use a wildcard mask.

Next, we need to add the ACL to the interface. We will add it to the `Gig 0/0` interface. The syntax for the command is `ip access-group 1 in` because we are applying it to the inbound traffic on the interface and using `access-list 1`. Remember that standard ACLs are only based on source IP address, so we are not specifying a destination IP address.

The full commands are:

```
interface Gig 0/0
ip access-group 1 in
end
```

#### Step 4: Testing Your ACL

Now that you have created your ACL, you need to test it. You can do this by logging back in to `H1` and `H2` and trying to ping `H3` and `H4` again.

1. Log in to `H1` and ping `H3` and `H4` to verify connectivity. You can do this by clicking the `H1` host icon in the topology and then clicking the **Console** button, and then **Open Console**.

To ping `H3`, issue the following command:

```
ping 172.16.20.31
```

Because you are pinging from a Linux host for all our ping tests, you will need to press **CTRL** + **C** to stop the ping (after a few seconds).

To ping `H4`, issue the following command:

```
ping 172.16.20.41
```

> The pings should be successful because we are allowing `H1` to access `H3` and `H4` and denying all other traffic.

2. Log in to `H2` and ping `H3` and `H4` to verify connectivity. You can do this by clicking the `H2` host icon in the topology and then clicking the **Console** button, and then **Open Console**.

```
ping 172.16.20.31
```

Because you are pinging from a Linux host for all our ping tests, you will need to press **CTRL** + **C** to stop the ping (after a few seconds).

To ping `H4`, issue the following command:

```
ping 172.16.20.41
```

> The pings should fail because we are denying `H2` access to `H3` and `H4` and denying all other traffic ().

Your output should look similar to the following:

```
H2:~$ ping 172.16.20.41
PING 172.16.20.41 (172.16.20.41): 56 data bytes
64 bytes from 172.16.20.41: seq=0 ttl=42 time=8.567 ms
64 bytes from 172.16.20.41: seq=1 ttl=42 time=3.262 ms
^C
--- 172.16.20.41 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 3.262/5.914/8.567 ms
H2:~$
H2:~$
H2:~$
H2:~$
H2:~$ ping 172.16.20.41
PING 172.16.20.41 (172.16.20.41): 56 data bytes
^C
--- 172.16.20.41 ping statistics ---
2 packets transmitted, 0 packets received, 100% packet loss
H2:~$
```

Notice that the first ping showed `0% packet loss` and the second ping showed `100% packet loss`; this is because the first ping was successful and the second was denied.
