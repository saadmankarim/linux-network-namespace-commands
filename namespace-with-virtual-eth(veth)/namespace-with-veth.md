## Adding two namespaces & connecting them with one veth

**Note**

> Please prefix following commands with `sudo` if we're not logged in as a root user.

Adding/Creating namespaces on the root namespace terminal (`ns1` & `ns2`)

```bash
ip netns add ns1
ip netns add ns2
```

Varify the namespaces

```bash
ip netns show
```
