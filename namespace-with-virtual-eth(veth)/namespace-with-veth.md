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
3. Creating virtual ethernet cable

```bash
sudo ip link add ns1-veth0 type veth peer name ns2-veth0
```

4. Varify the ethernet cable/vitual interfaces

```bash
ip link list
```
