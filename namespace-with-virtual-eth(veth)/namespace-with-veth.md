## Adding two namespaces & connecting them with one veth

1. Adding/Creating namespaces on the root namespace terminal (`ns1` & `ns2`)

```bash
sudo ip netns add ns1
sudo ip netns add ns2
```

2. Varify the namespaces

```bash
ip netns show
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/955d68db-d410-4138-8ad8-97acba4e6fc7)

3. Creating virtual ethernet cable

```bash
sudo ip link add ns1-veth0 type veth peer name ns2-veth0
```

4. Varify the ethernet cable/vitual interfaces

```bash
ip link list
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/7b907b12-7a4b-4b2e-a6cc-0a378af8c5f3)

5. Adding the cable to the namespaces as network interface (`ns1-veth0` to `ns1` & `ns2-veth0` to `ns2`)

```bash
sudo ip link set ns1-veth0 netns ns1
sudo ip link set ns2-veth0 netns ns2
```

Now if we check on root namespace for the links/interface. it won't be there anymore as it is now connected to the network namespaces.

```bash
ip link list
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/9ca4e369-3ccc-44aa-bf2b-fa00243b6aa5)

6a. Entering to the `ns1` namespace via `bash` for IP comfiguration

```bash
sudo ip netns exec ns1 bash
```

6b. Checking the current interface status

```bash
ip link list
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/66f30f9e-6abc-427a-bd69-bc9312b4bfb3)

Right now we see that `ns1-veth0` interface is attached and both `lo` & `ns1-veth0` interface is in DOWN state.
You will 1st assing an IP to `ns1-veth0` interface & then we will up the both interfaces.

6c. Assign an IP to `ns1-veth0` interface

```bash
ip addr add 10.10.10.1/24 dev ns1-veth0
```

6d. Check the interface for the IP address

```bash
ip addr show
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/3d421246-6220-4590-bf51-bbd7beb7852d)

As we can see the interface `ns1-veth0` got the IP we assinged.
Let's `UP` the interfaces

6e. Bring `UP` the interfaces

```bash
ip link set lo up
ip link set ns1-veth0 up
```

6f. Check the interface status

```bash
ip link list
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/20835e49-33f4-4230-aefb-e6b863c50cd6)

As we can see the interface `ns1-veth0` is in `LOWERLAYERDOWN` state which means the other end of the cable is still down. When we will up the other end of the cable, this interface will then state `UP` automatically.
Let's exit from `ns1` namespace using `exit` command or simply pressing `Ctrl + D`

7a. Entering to the `ns2` namespace via `bash` for IP comfiguration

```bash
sudo ip netns exec ns2 bash
```

7b. Checking the current interface status

```bash
ip link list
```

Output
![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/b65143f6-5919-4019-9488-02ef8c1eb26a)

Right now we see that `ns2-veth0` interface is attached and both `lo` & `ns1-veth0` interface is in DOWN state.
You will 1st assing an IP to `ns2-veth0` interface & then we will up the both interfaces.

7c. Assign an IP to `ns2-veth0` interface

```bash
ip addr add 10.10.10.2/24 dev ns2-veth0
```

7d. Check the interface for the IP address

```bash
ip addr show
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/f9562219-70d9-439c-9170-67334d4991e5)

As we can see the interface `ns1-veth0` got the IP we assinged.
Let's `UP` the interfaces

7e. Bring `UP` the interfaces

```bash
ip link set lo up
ip link set ns2-veth0 up
```

7f. Check the interface status

```bash
ip link list
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/9003c7cf-cd12-4e19-ba66-bb3f3ea92b00)

We can see that the interface `ns2-veth0` is in `UP` state. As we saw on `ns1` namespace after bringing `UP` the interface, `ns1-veth0` was in `LOWERLAYERDOWN` state. But as now the `ns2-veth0` is in `UP` state, the `ns1-veth0` will be in `UP` state.
We will check the status of the `ns1-veth0` interface from the root namespace.
Let's exit from `ns2` namespace using `exit` command or simply pressing `Ctrl + D`

8. Check the interface `ns1-veth0` on `ns1` namespace from root namespace

```bash
sudo ip netns exec ns1 ip link list
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/8d1f2876-33c4-45b2-8748-8209981d20fb)

9. Now let's check the connectivity of the namespaces from each other via `ping` command from the root namespace
(`ping` from `ns1` to `10.10.10.2` and `ping` from `ns2` to `10.10.10.1`)

```bash
sudo ip netns exec ns1 ping 10.10.10.2
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/d720f690-aae3-4275-a1fd-61e8cc079bdc)


```bash
sudo ip netns exec ns2 ping 10.10.10.1
```

Output

![image](https://github.com/saadmankarim/linux-network-namespace-commands/assets/16166697/ff3de481-0649-4e3f-a553-732e47a763fe)
