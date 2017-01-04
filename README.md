## coreos-baremetal-mac

This repository is a hack putting together various scripts and
configuration files that ease setting up a CoreOS cluster on 
macOS using iPXE with bootcfg/dnsmasq, similarly to what
[coreos-baremetal] does. There is absolutely no guarantee,
however, feel free to submit any patches you consider valuable.

### Usage

First of all, you must `./setup.sh` to create the 
bridge adapter that the VMs will use, enable NAT and set a 
LAN boot ROM for VirtualBox's virtio-net network driver that
is compatible with iPXE and bzImage. Due to the [limited space]
available for the ROM (0xE000 bytes), the provided ROM has been
built and shrunk manually to fit exactly that space. This script
only needs run once.

By default, three nodes are configured to start. You may change
that by editing `nodes.env` and `etc/dnsmasq/vboxnet0.conf`.
- node1 | 172.18.0.21 | 52:54:00:a1:9c:ae
- node2 | 172.18.0.22 | 52:54:00:b2:2f:86
- node3 | 172.18.0.23 | 52:54:00:c3:61:77

Download the desired CoreOS images with the `get-coreos` script.
You may want to store them in `assets/coreos/<version>` for instance.

Start both [bootcfg] and [dnsmasq] using the Procfile with the
help of [foreman] or [goreman]. You'll need to configure bootcfg
with the appropriate profiles / groups based on your utilization.
The configuration will automatically be stored in the `data/` folder.

Now simply use the `./vbox` CLI tool to manage your cluster. The
most common parameters are `create` and `destroy`.

### Common issues

bootcfg / dnsmasq does not start: the bridge may not be available yet.
Start VirtualBox and any VM to force VirtualBox creating it.

[coreos-baremetal]: https://github.com/coreos/coreos-baremetal
[limited space]: https://gist.github.com/robinsmidsrod/4001104
[bootcfg]: https://github.com/coreos/coreos-baremetal/blob/master/Documentation/bootcfg.md
[dnsmasq]: http://www.thekelleys.org.uk/dnsmasq/doc.html
[foreman]: https://github.com/ddollar/foreman
[goreman]: https://github.com/mattn/goreman

