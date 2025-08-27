# Step-by-step Guide for a Secure Web3 Setup

After posting [Better OpSec in 5 minutes](https://jstcz.eth.link/blog/2025/19/08/better-opsec-in-5-mins/#crypto-web3-specific-security), I received a bunch of positive feedback. Make sure to read the original post to have a great starting point for this guide.

[bguiz](https://blog.bguiz.com/) (also a huge thanks for reviewing this guide!) reached out and asked me to expand on the web3 part and create a step-by-step guide for a secure web3 setup. So that's exactly what you'll find in this guide.

It's no secret that if you own crypto, you become an attractive target for attackers. The crypto ecosystem has experienced over $4.7 billion in losses between 2024 and 2025, with 80% of losses resulting from operational security failures, not smart contract vulnerabilities. A lot of the losses are due to poor security practices and could be entirely prevented or drastically reduced with a good security setup.

This guide is primarily focused on Ethereum, but most of the security principles and tools discussed apply to anyone in crypto across all blockchains. The hardware wallets, phishing training, physical security practices, and general security mindset are universal.

I'll lay out a suggested security stack with concrete action items that you can follow step by step. I'm proposing one recommended approach rather than overwhelming you with options. There are also some suggestions for training you can do to be better at spotting phishing attempts and other security risks.

> Note: The recommended tools in this guide are ones I've personally used and would recommend to anyone. There are many other great tools out there, but I don't want to suggest anything I haven't tried myself.

## Setting Things Up

### **Software Wallet Security (Rabby)**

Rabby is an excellent choice for a software wallet due to its strong security features and user-friendly interface. It has both desktop and mobile apps, making it versatile for different use cases.

**Why You Need a Software Wallet**

- Most dApps require a browser wallet for easy interactions
- Daily transactions and mobile access
- Great interface for using hardware wallets with dApps
- Kept up to date with the latest updates and security features

**Setup Process**

1. Visit [rabby.io](https://rabby.io/) and install the extension (verify that you're on the official site)
2. Create wallet with strong password
3. Write down recovery phrase securely
4. Enable security features: transaction simulation, phishing detection, and address verification
5. Set up address book for trusted contacts

**Security Features**

- **Transaction Simulation:** Shows exactly what a transaction will do
- **Phishing Detection:** Warns about suspicious websites
- **Address Verification:** Shows ENS names and warns about similar addresses

**Best Practices**

- Keep only small amounts in accounts imported into the software wallet
- Mainly use software wallet as interface for interacting with dApps with your hardware wallet
- Always review transaction simulation
- Avoid giving unlimited ERC20 approval to dApps

**Action Items**

- Install Rabby extension
- Configure security features
- Connect hardware wallet
- Practice and get familiar with the interface on a testnet before using it with real funds

### **Hardware Wallet Setup (Trezor)**

Hardware wallets are one of the simplest and fastest ways to improve your web3 security. They are dedicated devices used for signing transactions. Even if your laptop is fully compromised, an attacker would still need physical access to your hardware wallet and its password to access your funds.

**Why Hardware Wallets Matter**

- Private keys never touch the internet
- Signing of transactions happens on a dedicated device
- Even if someone has access to your computer, they won't be able to access your crypto
- Well integrated with many wallets and services

**Setup Process**

1. Buy directly from [Trezor.io](https://trezor.io/) (never third-party and always verify that you're on the official site)
    - Trezor Safe 3 and 5 are recommended
2. Verify packaging integrity (tamper-evident seals)
3. Follow the setup wizard at [trezor.io/start](https://trezor.io/start)
4. Set PIN (6-9 digits) and memorize it
5. Generate recovery phrase (24 words) - don't take a picture or store it digitally
6. Enable passphrase for additional security
7. Test the recovery process completely before adding any funds

**Recovery Phrase Security**

- Write on paper (never digital)
- Store in fireproof/waterproof container
- Multiple secure locations
- Never photograph or scan
- Consider steel backup for fire protection
- Advanced: Share parts of the recovery phrase with trusted friends or family members for a more resilient setup

**Action Items**

- Purchase Trezor from official site
- Complete setup and test recovery
- Secure storage for recovery phrase
- Transfer any crypto that you plan on interacting with fairly regularly to the hardware wallet

### **Multi-Signature Wallet Setup (Safe)**

Multi-signature wallets require multiple approvals for transactions, providing protection against single points of failure. They can be used as an alternative to hardware wallets and are very convenient because if one of your signers is compromised, an attacker can't do anything with your funds. Instead of transferring all funds to a new address if you fear that your account might have been compromised, you can simply replace the compromised signer with a new one. The same is true if a key is lost or stolen.

**Use Multi-Signature For**

- Crypto that you plan on holding long-term
- Any amount of funds over $50,000
- Company funds and other accounts that shouldn't have a single point of failure

**Setup Process**

1. Visit [safe.global](https://safe.global/) and launch app (verify that you're on the official site)
2. Create Safe with 2-of-3 configuration (recommended for individuals)
3. Add owners: For example, hardware wallet, mobile wallet, backup device
4. Set threshold to 2 (requires two signatures)
5. Pay gas fees and deploy
6. Test with a small transaction to an account you control or to the multisig itself

Advanced: If you want additional security, you can use a 3-of-5 configuration instead, or even more owners. Instead of having control of all the signers yourself, you can add trusted friends or family members as signers and contact them individually to sign transactions. The trusted signers shouldn't know each other.

**Transaction Process**

1. Create transaction in Safe interface
2. Sign with first owner (hardware wallet)
3. Sign with second owner (mobile wallet)
4. Execute transaction with either of the owners
5. Verify transaction on Etherscan or other explorer

**Action Items**

- Create Safe with 2-of-3 configuration
- Make sure that at least the backup device is not easy to obtain in a worst case scenario
- Don't store the backup device in the same geographic location as the other signers
- Test with small transaction
- Transfer significant holdings to Safe
- Optional: Set up spending limits for each owner

### **Air-Gapped Transaction Signing (Advanced)**

**What is Air-Gapping?**

An air-gapped device has never been connected to the internet and never will be. Used solely for signing transactions via QR codes. The specific suggested setup below works really well as a way to make sure your phone is not a single point of failure and to make sure you can sign transactions safely anywhere you go.

Big shoutout to Hudson Jameson for introducing me to [airgap.it](http://airgap.it) a while back when we discussed our setups. Here’s a great [talk by Hudson on different wallet setups](https://youtu.be/e41HmRQKIts?si=S38eFs2sMRSBmvwY) from Protocol Berg v2.

**Use Air-Gapping For**

- A portable and very secure setup ideal for traveling
- When you mainly interact with the blockchain on your phone
- Wallets with more funds than you can afford to lose

**Setup Process**

1. Get an Android phone that supports [GrapheneOS](https://grapheneos.org/) (mostly Pixel phones)
2. Factory reset and never connect to internet
3. Download Air-Gap Vault APK from [airgap.it](https://airgap.it/) via USB
4. Follow the setup wizard for optimal security
5. Install Air-Gap Wallet on your everyday phone
6. Test with small transaction as per the Transaction Workflow section below

**Transaction Workflow**

1. **Online Device:** Create transaction → Export QR code
2. **Air-Gap Device:** Scan QR code → Review details → Sign
3. **Air-Gap Device:** Generate signed transaction QR code
4. **Online Device:** Scan signed QR code → Broadcast

**Action Items**

- Prepare air-gapped device
- Install Air-Gap Vault on air-gapped device
- Install Air-Gap Wallet on your everyday phone
- Test transaction workflow

## Additional Ways to Stay Safe

### **Phishing Training**

Crypto users are prime phishing targets, and with the extremely rapid evolution of AIs we're currently seeing, phishing attempts are becoming more and more sophisticated. There are several stories of people who've been working in crypto for many years who were inattentive for a brief moment and lost a lot of money. You can find such a story at the end of this guide.

Being able to recognize phishing and scam attempts is a crucial skill to have for anyone in and outside of crypto.

**Free Training Resources:**

- [Unphishable Web3 Challenges](https://unphishable.io/challenges) - Good, but requires MetaMask to be installed
- [Phishing Dojo](https://phishing.therektgames.com/trainings) - Good interactive training
- [OpenDNS Phishing Quiz](https://www.opendns.com/phishing-quiz/) - More examples

**Crypto-Specific Red Flags:**

- Fake wallet websites ([metamask.com](http://metamask.com/) vs [metamask.io](http://metamask.io/))
- Requests for private keys or seed phrases
- Urgency and time pressure
- Too-good-to-be-true offers
- Fake emails from exchanges, wallets, etc.
- Malicious browser and code editor extensions

If you’re unsure how to spot or be aware of above red flags, you should check out the free training resources.

**Action Items:**

1. Complete one of the phishing training resources
2. Bookmark trusted crypto sites
3. Never click links in emails/messages
4. Always verify URLs manually
5. Read my guide on [general OpSec](https://jstcz.eth.link/blog/2025/19/08/better-opsec-in-5-mins/)

### **Physical Security Best Practices**

**Appearance**

- Don't wear crypto-branded clothing in ways that display wealth
- Especially avoid wearing any crypto merch when outside of conference venues during big conference weeks
- Don't post about gains or display wealth indicators on social media
- Use aliases when possible

**Device Security**

- Never leave devices unattended
- Lock devices immediately when stepping away
- Set as short as possible auto-lock timeouts on all devices
- Use a dedicated device for crypto activities
- Don't have your laptop covered in crypto stickers
- Use [privacy screen protectors](https://panzerglass.com/pages/how-do-privacy-screen-protectors-work) to decrease “shoulder peeking”
- Use sliding webcam covers to avoid unwanted peeking in case someone has remote access

**Travel Security**

- Don't travel with hardware wallets unless absolutely necessary
- If work requires traveling with hardware wallets, use a dedicated work device (separate from personal)
- Blend in and look like a regular tourist or local
- Use VPN on all networks
- Optional: Bring a decoy phone

**Social Engineering Defense**

- Deflect questions and change the subject if someone seems overly eager to learn specific details about you or your crypto setup
- Trust your instincts if something seems off
- Consider having an alternative background or story to tell people you don't know

### **Privacy Improves Security**

Privacy and security go hand in hand. Good privacy is a key part of security, because it makes it harder for bad actors to identify who you are, how much money you have, where you live, etc. One approach to achieving this is by disconnecting accounts from each other. Instead of having 20 ETH in one account, you could have 2 ETH in 10 different accounts that are not linkable to each other. Consider not using addresses derived from the same BIP39 seed phrase for improved account separation. Tools like privacy pools can help achieve this.

**Privacy Pools**

As described above, it improves privacy and security to have funds distributed in accounts that are not connected. This can be done by using a tool like [Privacy Pools](https://privacypools.com/). Privacy Pools is a compliant version of Tornado Cash. You deposit funds into the pool and then withdraw them to a different address. You can do this for each dApp you interact with in order to have your funds separated from each other and to make you a less attractive target.

## Immediate Steps to Take For Significant Improvements

It can be intimidating to go out and buy a Trezor, YubiKey, and an Android phone at the same time. I recommend starting with the most essential steps, which provide a solid foundation for a secure setup. You can always add more complex tools to your stack once you become comfortable with the fundamental ones.

My recommendation for immediate actions to take:

- Order a Trezor
- [Set up Rabby Wallet](#software-wallet-security-rabby)
- [When Trezor arrives, set it up and remember to check packaging integrity](#hardware-wallet-setup-trezor)
- [Setup a Safe multisig wallet](#multi-signature-wallet-setup-safe)

## **Resources & Further Learning**

- [Security Alliance (SEAL)](https://securityalliance.org/) - Comprehensive suite for security in web3 by some of the most experienced security professionals
- [0xZak's Breakdown of Getting Hacked](https://x.com/0xzak/status/1955265807807545763) - Excellent breakdown and tips for dealing with being hacked

## **Closing Remarks**

Security is a journey, not a destination. The crypto landscape evolves rapidly, and so must your security practices.

**Key Principles**

1. **Defense in Depth:** Multiple layers of security
2. **Remove Single Points of Failure:** One slip up or a compromised device shouldn't result in your life savings being gone
3. **Zero Trust:** Verify everything, trust nothing
4. **Regular Maintenance:** Security requires ongoing effort
5. **Education:** Stay informed about new threats
6. **Practice:** Regular testing and drills
7. **Monitoring:** Set up monitoring for your accounts (use Etherscan or similar)

Remember: The cost of prevention is always less than the cost of recovery. In crypto, there often is no recovery at all.
