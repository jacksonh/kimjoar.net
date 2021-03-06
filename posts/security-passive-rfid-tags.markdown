The Security of Passive RFID Tags
=================================

Essay in the course TTM4137 Wireless Network Security at NTNU. Chosen as curriculum in the course.

### Introduction

Radio-frequency identification (RFID) technology uses radio frequency signals to automatically identify objects. RFID tags are gradually being included into most objects on Earth — from library books to passports to animals and all other things imaginable. An RFID tag is a small wireless device that reacts to electromagnetic fields generated by an RFID reader<sup>2</sup>. RFID technology is one of the most promising technologies in the scope of ubiquitous computing<sup>1</sup>, and for every information technology being deployed in such great numbers, security is essential.

In this essay I will try to identify security threats and see how they can be guarded against.

### A basic understanding of RFID

RFID is a system for no-contact, non-line-of-sight and invisible identification<sup>3</sup>, and is comprised of three main components<sup>8</sup>:

* an RFID tag, which is attached to the object to be identified.
* an RFID reader, which is able to read and write to a tag.
* a data processing subsystem, which is connected to an RFID reader.

There are basically three types of RFID tags<sup>13</sup>: active, semi-passive and passive. The difference is that active tags have an on-tag power supply and actively send RF signals, while passive tags receive the power from an RFID reader. The semi-passive tag has both an on-tag power supply and rely on power from a reader.

A passive RFID tag has a short communication range, a very small memory footprint — typically just hundreds of bits — and very limited computational power. With these very limited resources it is difficult to implement strong cryptographic functions<sup>3</sup>.

The primary focus of this essay is the RFID tag itself, and not the entire system. I will focus on passive RFID tags since they are the most limited tags, and because both semi-passive and active tags can handle far more resource intensive security protocols.

### Threat Model

The Internet Security Glossary defines a security threat as "A potential for violation of security, which exists when there is a circumstance, capability, action, or event that could breach security and cause harm."<sup>4</sup>

To get a grasp of the security threats in RFID I will use the STRIDE threat model developed by Microsoft<sup>5</sup>. STRIDE includes the following six categories of threats:

* Spoofing — An attacker is successfully able to pose as an authorized user of a system.
* Tampering — An attacker is able to modify data.
* Repudiation — A user denies a situation and no proof exists to prove that the action was performed.
* Information Disclosure — Information is exposed to an unauthorized user.
* Denial of Service — Denying service to valid users.
* Elevation of privilege — An unprivileged user gains higher privileges in the system than what he is authorized for.

Categorizing security threats with STRIDE identifies potential strategies for mitigating them<sup>9</sup>.

### Threats

An example of a _spoofing_ attack is when an attacker masquerades as a legitimate tag. To guard against the threat of spoofing, authentication is needed. There exists several strong authentication protocols, such as EAP and Kerberos, but as mentioned the problem with RFID tags, and especially passive tags, is that they are very resource limited. This severely limits the set of possible authentication protocols that can be used. 

According to Peris-Lopez et al. a passive RFID tag roughly has between 5000 and 10000 logic gates<sup>1</sup>. 250 to 3000 of these gates are available for security measures. A typical implementation of AES needs in the order of 20000 logic gates. Lightweight authentication protocols specifically created for, or adapted to, RFID have been introduced by several authors<sup>6,7</sup>.

_Tampering_ with data can for example occur when an attacker is able to modify the tag in a passport to remove unwanted information. There are two kinds of protection against tampering<sup>10</sup>: tamper-evidence, in which the system detects tampering, and tamper-resistance, in which the system is able to resist tampering. 

In Electronic Product Codes (EPC), which are based on passive RFID tags, a 32 bit PIN is needed to get access to the internal memory of the tag<sup>11</sup>. Because of the difficulty of tampering with transmitted data and the length of the PIN needed, Garcia-Alfaro et al. classify integrity threats as unlikely<sup>11</sup> (but as a side-note, they don’t seem to have considered the fact that this approach is vulnerable to eavesdropping). Gandino et al. examines the most recent studies of tampering in RFID, and notes that data tampering is still in 2009 a critical threat in RFID based systems<sup>10</sup>.

A _repudiation_ is, for example, when a retailer denies receiving a certain pallet. To ensure that neither sender nor receiver can deny an action, a non-repudiation protocol is required<sup>3</sup>.

Mitigation techniques for repudiation include digital signatures, timestamps and audit trails<sup>9</sup>, but this is a challenge because of the limited computational power of RFID tags. The tag itself does not have enough memory to save an audit trail, so this must be done externally. This puts a much higher demand on the _data processing subsystem_.

An _information disclosure_ attack is, for example, when a thief queries tags carried by potential victims to determine what they have in their bags. To guard against this, it should not be possible for the thief to determine the object he is trying to identify. Authentication will make it significantly more difficult to determine the identity, but it is still possible to track the specific tag, since it always will have the same identification. For example a thief queries a tag on an expensive watch in a store, and thereby learns its unique identificator. He then waits outside the shop, querying all the tags that pass. When someone buys the watch, the thief will know who. To remedy this Good et al. suggest that RFID tags should be relabeled on checkout<sup>14</sup>. Juels names other possible solutions, including killing the RFID tag or rotating between a collection of pseudonyms to identify it<sup>13</sup>.

Since RFID if based on radio communication, an example of a _denial of service_ attack is to shield a tag with a Faraday Cage, and thereby make it unreadable. Another possibility is to send radio signals that collide with legitimate RFID signals, making it impossible for the reader to communicate with the tag. Since all wireless devices are subject to radio jamming, this is not an issue that is specific to RFID <sup>14</sup>. Sarma et al. suggest a method requiring physical contact for critical functionality, which will help defend against denial of service attacks<sup>8</sup>.

Since RFID is such a limited system, most passive tags only have two types of access: no access and access to everything, i.e. two states: locked and unlocked. A tag only enters the unlocked state when it receives an appropriate command. _Privilege elevation_ means putting a tag into the unlocked state. The success of this attack is dependent on the choice of anti-tampering approaches<sup>10</sup>. The privilege elevation threat is far more important when considering the entire RFID system.

### Conclusion

By using the STRIDE model, we have been able to get a better understanding of the security of passive RFID tags, and identified several major and critical security threats there. According to Rathinasabapathy and Rajendran a major challenge with RFID is the immaturity of the industry, and that the standards are being developed while RFID is being globally deployed<sup>12</sup>.

RFID is already widely used, and will be globally deployed into more and more areas of life in the coming years. It is therefore important to know that there are security threats related to the use of RFID, and that many of them are major.

### References

1. Peris-Lopez, P. et al. _RFID systems: A survey on security threats and proposed solutions_. Lecture Notes in Computer Science, Springer, vol. 4217, no. , pp. 159-170, 2006. 
2. Damgard, I. and Pedersen, M.O. _RFID security: Tradeoffs between security and efficiency_. Lecture Notes in Computer Science, vol. 4964, no., pp. 318, 2008.
3. Thompson, D.R. and Chaudhry, N. and Thompson, C.W. _RFID Security Threat Model_. In Proc. Acxiom Laboratory for Applied Research (ALAR) Conf. on Applied Research in Information 	Technology, Conway, Arkansas, Mar. 3, 2006.
4. Shirey, R.. _Internet Security Glossary, RFC 2828_. The Internet Society, May 2000
5. Hernan, S. and Lambert, S. and Ostwald, T. and Shostack, A. _Uncover Security Design Flaws Using The STRIDE Approach_. MSDN Magazine-Louisville, CMP Media Inc., pp. 68-75, 2006.
6. Lee, Y. and Verbauwhede, I. _Secure and low-cost RFID authentication protocols_. In Proc. 2nd IEEE International Workshop on Adaptive Wireless Networks (AWiN), pp. 1-5, 2005.
7. Vajda, I. and Buttyán, L. _Lightweight authentication protocols for low-cost RFID tags_. In Second Workshop on Security in Ubiquitous Computing – Ubicomp 2003, 2003.
8. Sarma, S.E. and Weis, S.A. and Engels, D.W. _RFID systems and security and privacy implications_. Lecture Notes in Computer Science, Springer, pp. 454-469, 2003.
9. Thompson, D.R. and Di, J. and Sunkara, H. and Thompson, C. _Categorizing RFID privacy threats with STRIDE_. In Proc. ACM Symposium on Usable Privacy and Security (SOUPS), Carnegie Mellon University, Pittsburgh, Pennsylvania, July 12-14, 2006.
10. Gandino, F. and Montrucchio, B. and Rebaudengo, M. _Tampering in RFID: A Survey on Risks and Defenses_. Mobile Networks and Applications, Springer, pp. 1-15, 2009.
11. Garcia-Alfaro, J. and Barbeau, M. and Kranakis, E. _Security threats on EPC based RFID systems_. International Conference on Information Technology: New Generations (ITNG 2008), 	IEEE Computer Society, Las Vegas, Nevada, USA. 2008.
12.	Rathinasabapathy, G. and Rajendran, L. _RFID Technology and Library Security: Emerging Challenges_. Journal of Lib. Inf. & Comm. Technology, vol. 1, no. 1, 2009.
13.	Juels, A. _RFID Security and Privacy: A Research Survey_. IEEE Journal on Selected Areas in Communications, vol. 24, no. 2, pp. 381-394, 2006.
14.	Good, N. et al. _Radio frequency identification and privacy with information goods_. Workshop on Privacy in the Electric Society, ACM, ACM Press, 2004.
15.	Weis, S. et al. _Security and privacy aspects of low-cost radio frequency identification systems_. Lecture Notes in Computer Science, Springer, pp. 201-212, 2004.
