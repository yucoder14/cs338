Wednesday, 2024-10-16
Changwoo Yu  

SCENARIO #1: RESPONSIBLE REPORTING OF SECURITY VULNERABILITIES 
===============================================================

You have discovered a bug in the InstaToonz music-sharing app. This bug is a nasty one that would allow an attacker to read the contents of all the private InstaToonz direct messages for anyone who has ever posted a public InstaToonz message. This bug threatens the privacy of hundreds of millions of InstaToonz users.

You want to report this bug to InstaToonz, Inc. to protect their customers, but you know that the last time somebody reported a security bug to them privately, InstaToonz sued the bug-reporter in North Carolina and also called in the FBI, causing the person significant hassle and expense. The case was briefly a cause célèbre in the tech world, with calls for boycotts and state and Congressional action. Eventually, after a fair amount of sabre-rattling, InstaToonz dropped the suit. But at the same time, they released a statement articulating their belief that all security researchers (which InstaToonz always put inside scare quotes) are engaging in attempted thievery of trade secrets. After a brief investigation upon being first contacted by InstaToonz, the FBI declined to pursue the matter further. InstaToonz has refused all demands that they establish a bug bounty program.

For the sake of this assignment, I'm going to assume that this happened in the U.S.

Main Ethical Question
=====================

The main ethical question posed by this scenario is matter of reporting the security bug that threatens the privacy of InstaToonz users. The difficulty of the scenario lies in the detail that InstaToonz actively denounces its users from discovering and reporting the bugs. Previously, they have sued the bug-reporter until they were shunned upon by the tech world and dropped the suit. They also have denounced the efforts of security professionals for attempting to steal their "trade secrets". Lastly, they refused to establish a bug bounty program, expressing their disapproval of any form of hacking in general. 

Disregarding InstaToonz recalcitrant tendencies, the *right* thing to do would be to report the security bug that has the potential to invade user's privacy. However, now considering the uncooperative nature of InstaToonz, one could start asking oneself the question: *is it worth it?* Although such a question conflicts with the ethical interest, self interest is a motivation factor that we cannot ignore. Having observed InstaToonz reaction to previous reporting of a bug, assuming that their administrative philosophy has not changed significantly since, the reporting of the new *nasty* but could incite a similar reaction from the company. 

Another interesting question that we can pose: *will it make any difference?* If the company is driven by profits rather than ethics, it is very possible that, minding the seriousness of the bug, they will try to ignore the bug report. The anticipation of the cost and labor required the fix the bug could discourage the company from ever making the change. 

Yet another interesting question is the discoverability of the security bug. If the process of finding the security bug is *easy*, meaning others can easily find and exploit the bug, then it would be worthwhile to endure the potential lawsuits and press coverage to report the bug. However, if it is obscenely difficult exploit it, *will more harm be caused by reporting the bug?* Can you trust that, even if you report the matter privately to InstaToonz, will their IT infrastructure be secure enough to protect the vulnerability before it gets leaked?  

Stakeholders
============

The Bug-reporter
----------------

The Bug-reporter has the right to report or not to report the security bug. Even if one of the choices conflicts with the ethics, it is up to bug-reporter to decide whether to report the bug. The question of obligation is independent of bug-reporter's rights.

InstaToonz
----------

InstaToonz has right to their software under creative license. If the software is not open source, then any hacker would be breaking the law by trying to remove software protection to peek inside InstaToonz software. 

Also, InstaToonz has the right to sue you. Is it ethical to do so? The answer is, again, independent of InstaToonz's rights. 

Users
-----

Users has right to privacy and confidentiality. While it is their choice to share their data through public posts, their right to privacy should be respected when it comes to private direct messages. If the users use the *private* message expecting privacy, they should get privacy.

Missing Details
===============

It would be helpful to know InstaToonz's willingness to fix bugs if they are discovered by hackers. While they have explicitly expressed the detestation of any discovery of bugs by suing the bug-reporter who reported the security bug *privately*, the scenario does not provide any details on whether that security bug was eventually addressed. Are they avoiding discovery of bugs because it creates a costly nuisance but possess the capability to deliver security patches in timely manner? Or are they avoiding discovery of bugs because they do not have the capacity to fix the bugs? 

Does the bug involve the encryption and copy-protection of the music shared by InstaToonz users? 

Actions and Consequences
========================

Action 1: Report The Bug Privately (Coordinated Vulnerability Disclosure)
-------------------------------------------------------------------------

By disclosing the *nasty* security bug privately to InstaToonz before publicly announcing it, you are granting InstaToonz the time to analyze your accusations and decide how to address the problem. If InstaToonz attempts and succeeds to fix the bug, then you just have prevented bad actors from exploiting the vulnerability. This choice agrees with the ethical interest, for you are raising awareness to a potentially harmful security bug to the owner of the software. Nevertheless, you cannot control the reaction from InstaToonz. They could sue you as we have seen.

Action 2: Report The Bug Publicly (Full Disclosure)
---------------------------------------------------

By disclosing the security bug publicly, you are alerting not only the security experts but also the bad actors. While such a choice will force a reaction from InstaToonz, you risk creating more harm by bringing awareness of the security bug to bad actors. It is very possible that the bug will be exploited before a security fix can be delivered

Action 3: Ignore
----------------

By choosing to ignore the security bug, you are not causing any immediate harm. However, you cannot 
prevent the same discovery from being made by bad actors, who will definitely exploit the vulnerability to steal private information of the users. Because you did not report the bug, you won't be sued by InstaToonz. If you do not exploit the vulnerability, then you are not causing additional harm. In the long run, such negligence can foster an industry where people simply overlook any vulnerabilities, which could create an environment where users can no longer trust any software they are using. 

Legality: If You Choose to Report
---------------------------------

If the bug involves encryption and copy-protection of the music shared by InstaToonz users, you might be accused of breaking the Digital Millennium Copyright Act. However, you can state your case by referring to Sec. 103(f) of DMCA: 

> (f) Reverse Engineering.— (1) Notwithstanding the provisions of subsection (a)(1)(A), a person who has lawfully obtained the right to use a copy of a computer program may circumvent a technological measure that effectively controls access to a particular portion of that program for the sole purpose of identifying and analyzing those elements of the program that are necessary to achieve interoperability of an independently created computer program with other programs, and that have not previously been readily available to the person engaging in the circumvention, to the extent any such acts of identification and analysis do not constitute infringement under this title. 

If you have lawfully obtained InstaToonz's software and reverse-engineered it for the sole purpose of *interoperability* of the software at the time of your discovery of the bug, you could argue that you have not committed any felony. 

ACM Code of Ethics and Professional Conduct
===========================================

Design and Implement Systems that are Robustly and Usably Secure
----------------------------------------------------------------

InstaToonz has failed to ensure a robust security and criminalize vulnerability reporting. Then, under the ACM Code, they have failed to abide by the Code. By choosing to not report the bug, you are also failing to abide by the Code. 

By choosing to report, you are following the Code.

Foster public awareness and understanding of computing, related technologies, and their consequences
----------------------------------------------------------------------------------------------------

By refusing to address security bugs, InstaToonz is discouraging the public from understanding how their technology works and how the bugs could impact its users.

FINAL JUDGMENT
==============

You should share the vulnerability privately with InstaToonz. 
