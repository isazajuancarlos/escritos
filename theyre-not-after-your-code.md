# They're not after your code

### Why the big attacks of 2025–2026 hit everything that's massive —npm, the AUR, the Snap Store, GitHub, WordPress— and what that says about where the truth of your software should live

*By Juan Carlos Isaza — lawyer and programmer. Xiliux.*

---

There's a question that dismantles most of the badly framed fear about open-source security: **why would anyone hack GitHub just to get the code?**

Open source is already open. Anyone can clone it this afternoon, read all of it, study it. You don't have to force any door to *read* what is, by definition, published. If the prize were the code, there would be no attack: there would be a `git clone`.

The prize is something else, and understanding it properly changes the whole conversation.

## The prize isn't reading your code. It's getting the world to run theirs with your name on it.

Classic information security worries about **confidentiality**: that no one reads what they shouldn't. But the attacks that have actually done damage at scale these past two years aren't about confidentiality. They're about **integrity**: they don't want to see your code, they want to *change* it —or change something you depend on— so that millions of machines run their payload without suspecting a thing, shielded by the trust you already earned.

The target is never the end victim. It's the **point of concentrated trust**: the single node that thousands or millions of projects hang from. And the propagation isn't done by the attacker —it's done by the distribution infrastructure itself: the package manager, the automatic update, the transitive dependency. The attacker chases no one. The victims come on their own to update.

## The same attack, six different layers

When you put the big incidents of 2025–2026 side by side, you stop seeing six attacks and start seeing **one**, applied to six layers of the same chain of trust:

- **npm.** The *Shai-Hulud* worm campaigns and their *CHAINDROP* variant compromised, depending on who's counting, between 400 and more than 2,200 packages and artifacts over the course of 2026. It's a **self-propagating** worm: with a maintainer's stolen credentials, it automatically backdoors every other package that person can publish, and it even looks up its command server in an Ethereum smart contract. ([Elastic Security Labs](https://www.elastic.co/security-labs/shai-hulud-chaindrop-npm-supply-chain), [Unit 42](https://unit42.paloaltonetworks.com/monitoring-npm-supply-chain-attacks/))

- **AUR (Arch Linux).** In July 2025, packages like `librewolf-fix-bin` were uploaded that installed the *Chaos RAT* trojan: the `PKGBUILD` cloned an attacker-controlled repository and ran it during the build, because `makepkg` runs **without sandboxing**. ([BleepingComputer](https://www.bleepingcomputer.com/news/security/arch-linux-pulls-aur-packages-that-installed-chaos-rat-malware/))

- **Snap Store (Canonical).** It's worth being precise: there was no breach of Canonical's infrastructure. There was **hijacking of legitimate publisher accounts** —expired domains that attackers re-bought to seize the email and, with it, the Snapcraft account— to push fake crypto wallets that steal the seed phrase. Trust was the door. ([Help Net Security](https://www.helpnetsecurity.com/2026/01/21/linux-malware-snap-store/))

- **GitHub.** Maintainer accounts taken over, orphan commits that bypass code review, abused continuous-integration workflows. In one case, the compromised account of an employee at a large distribution was used to push code without passing through any review at all.

- **WordPress and PHP.** Around **70% of websites whose server-side language is known run PHP** ([W3Techs](https://w3techs.com/technologies/details/pl-php)), and WordPress alone is close to **43% of the entire web**. Its real attack surface isn't the core —reasonably audited— but the ecosystem of **third-party plugins and themes**: tens of thousands, of uneven quality, many abandoned, all running with the core's permissions. The same pattern as the AUR and npm, but at the scale of half the internet.

- **Artificial intelligence, which isn't one more layer but the accelerant of all of them.** In November 2025 the first cyber-espionage campaign conducted mostly by an AI was documented: a state-backed group automated between **80% and 90%** of the tactical operations —reconnaissance, exploitation, credential theft, exfiltration— against some thirty organizations, evading the model's safeguards by disguising each step as routine security testing. ([The Hacker News](https://thehackernews.com/2025/11/chinese-hackers-use-anthropics-ai-to.html))

## It's not an army with a general. It's a swarm with an incentive.

It's tempting to conclude there's a single, coordinated network behind all this. The evidence says something more uncomfortable —and harder to fight—. The very researchers who track these campaigns leave attribution open and warn that **copycat activity** makes it increasingly hard to attribute the waves to a single group.

In other words: no puppeteer is needed. What exists is a **market with a shared incentive**. The supply-chain vector works so well —one compromise, thousands of victims downstream— that every actor migrates toward it and imitates whoever succeeded. AI accelerates that imitation to near-instant. The result *looks* coordinated, but the mechanism isn't conspiracy: it's **selection**. The best-performing vector is the one everyone copies.

This should reassure no one. A swarm with no center can't be decapitated —there's no leader to arrest—, it replicates faster than any coordination could, and the incentive driving it doesn't go away by patching one actor.

## The underlying law: scale is the vulnerability

If it has to be said in one sentence: **what makes a component valuable —that millions trust it— is exactly what makes it a target.** Popularity and security are in direct tension. Every new project that depends on you raises your value as a target. That's why "everything massive" gets attacked: not because it's more poorly built, but because it concentrates more trust per unit of attacker effort.

And here enters a dimension that, as a lawyer, I can't overlook. Supply-chain integrity is ceasing to be a purely engineering problem and becoming an **enforceable best practice**. Data-protection frameworks —Colombia's Law 1581 of 2003, and its equivalents around the world— hold whoever processes personal data responsible for its integrity and security, and push in that direction: verifying what you depend on becomes part of the expected diligence. "I trusted the package" is, each time, a weaker defense; choosing what you trust starts to count.

## What I did, with its price

A while ago I made an uncomfortable decision for my own work: **the source of truth for my code stopped living on a public platform.** It lives in local mirrors, and the deliverables travel signed and verifiable, without depending on a remote registry serving me the right file.

I don't tell this as a universal recipe or as a boast. I tell it with its cost, which is real: I lost discoverability, I lost the free external backup a large platform gives, and I lost funding opportunities that require exposing the code in the open. I'd do it again, because I work in security and cryptography, and for a project like that, having your code extracted or altered isn't an incident: it's an argument against the product. But anyone weighing this path deserves to see the full invoice, not a postcard.

## What anyone can do

If the attack is against **concentrated, self-distributing trust**, the defense isn't raising a taller wall. It's **reducing and verifying trust**:

1. **A source you can verify yourself.** Reproducible signatures, checksums you can check. Not "trust that the registry served you the good file": check it.
2. **Don't depend on a single distributor.** The point that, when it falls, falls for everyone, is the point you shouldn't have.
3. **Minimize third-party surface.** Every dependency is a trust node you don't control. The safest dependency is the one you didn't add.

None of this is free or comfortable. But it's consistent with what the evidence of these two years is shouting at us: scale concentrates trust, concentrated trust is the target, and the only real way out is to make that trust **verifiable and distributed** again.

They're not after your code. They're after the trust others place in it. And that trust, today, you have to be able to prove.

---

*Juan Carlos Isaza is a lawyer and programmer. He builds [Quipu](https://xiliux.com), a verifiable post-quantum encryption and encoding library with a reproducible, network-free supply chain. He writes about software security under the Xiliux brand.*
