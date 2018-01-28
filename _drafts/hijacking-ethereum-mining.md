---
layout: article
title: "Hijacking Ethereum Mining"
description: "How Ethereum Miners are being hacked!"
---
During 2017, public attention turned to the money-crazed frenzy surrounding Bitcoin. Browser-based cryptocurrency
mining became a significant concern amongst many as malicious sites attempted to run JavaScript miners in the
user’s browser at their expense. Successful mining of any cryptocurrency depends principally on hash rate; a metric
difficult to maximise with JavaScript. This incredible searge in public attention in cryptocurrencies fueled monstrous
growth in their value, incentivising evermore nefarious and ingenious ways of mining for profit.

Another emerging trend that we can expect to see accelerate in 2018 is the prolific growth and effects of botnets.
As more and more, poorly secured, devices connect to the public Internet the possibilities for large scale attacks
become progressively more likely and dangerous. Botnets have been around for decades though, infecting vulnerable
computers and using them to launch DDOS attacks, store illegal material, send spam emails, and more. The sheer
scale of this new breed of botnet is what makes them so formidable and will prove to be a serious threat to public
infrastructure.

The financial incentive to infect devices for the purposes of cryptocurrency mining are clear. Mining is typically the
preserve of bespoke machines that are specifically built for their purpose and use high-end GPUs to bolster their hash
rate. Infecting a network of miners of this type would therefore have huge monetary value to an attacker. As
sophisticated miners of this type are optimised so aggressively for their purpose they will often leave parameters,
assumed to be correct, unchecked leaving them open for attack.

The source code for the [Mirai malware][1] that infected IoT devices with incredible success in 2016 was released and has
since been modified by many hackers for their own purposes. [Satori][2] is one such fork. NetLab360 first identified and
named the fork. Since their initial identification of devices infected with Satori, the security community was able
to sinkhole the botnet and hamper further infection of devices. The threat persisted however as it was only a matter of
time before further forks would be discovered in the wild.

Another variant of Satori as since emerged, aptly named [Satori.Coin.Robber][3] by NetLab360. The malware seeks to
connect to devices running the [Claymore Miner software][4] used to mine the Ethereum cryptocurrency. The mining
software provides a management facility through port 3333 which by default requires no authentication. This is a known
issue, [CVE-2017-16929][5], and Python source code that exploits the issue has since been [disclosed][6].

If an unprotected miner is discovered the Satori.Coin.Robber malware will attempt to replace the wallet address used to
store mined Ether with its own wallet address. As of this writing an [account][7] has received three payments from
compromised miners, amounting to 2.796 Ether which equates to roughly $3.5K.

This vulnerability will undoubtedly be fixed however, basic security practices must still be carried out to ensure that
the appropriate patches are applied and in a timely manner. In this case, compromised devices were improperly
configured and were therefore vulnerable. This is an alarming fact given that the owner of the miner benefits directly
from the security of their miner, and yet they have neglected it. Many services on the web do not and yet we expect
that they are appropriately secured.

[1]: https://en.wikipedia.org/wiki/Mirai_(malware)
[2]: http://blog.netlab.360.com/warning-satori-a-new-mirai-variant-is-spreading-in-worm-style-on-port-37215-and-52869-en/
[3]: http://blog.netlab.360.com/art-of-steal-satori-variant-is-robbing-eth-bitcoin-by-replacing-wallet-address-en/
[4]: https://github.com/nanopool/Claymore-Dual-Miner/releases
[5]: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-16929
[6]: https://www.exploit-db.com/exploits/43231/
[7]: http://dwarfpool.com/eth/address?wallet=B15A5332eB7cD2DD7a4Ec7f96749E769A371572d&allpayouts=1