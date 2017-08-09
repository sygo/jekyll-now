---
layout: post
title: Post Ethical Compromise - Let's Start Here
---

Few things are scarier that your run of the mill penetration test bringing to light a vulnerability that has been present for a long time and admitting that it is feasible that an attacker may have gained a foothold into the corporate environment or a [segment of the network](https://en.wikipedia.org/wiki/Network_segmentation) that was otherwise believed to be secure. Perhaps your [SOC](https://en.wikipedia.org/wiki/Network_segmentation) doesn't have visibility over that part of the network, or (and this is very often the harsh reality) there is no visibility over egress traffic, so if the attacker is already on the network your security analysts are completely blind-sided. Now what?

It's my personal belief that zeroing down on the potentially compromised server will likely be your best bet at any indication of real compromise.

If whomever you hired to do the pentest is experienced and practiced due diligence, there should be a detailed account of what actions were performed on the vulnerable host - gather that data, get acquainted with it and keep it in mind as the investigation develops. It's important to distinguish what your penetration tester did from what a real malicious actor may have done. If such record doesn't exist make note of the time and date the penetration tester performed the assessment on that host (preferably) or at the very least the time of the penetration test as a whole - whatever data you may have to distinguish the actions of the penetration tester from a real attacker will be crucial.

Before we dive into it, let me summarize what I personally recommend in these situations:

- Backup and/or image the host in it's current state.
- Keep the server on-line, make no changes to it (if you *do* have a malicious actor pivoting within your network the last thing you want is for him or her to start covering tracks)
- Perform the analysis on the backup/image after you upload it to your virtualization platform of choice (if you are setting it up on hardware that's fine too), network connectivity is not necessary, and it  may actually be detrimental since if there is any cnc traffic being sent from the host the receiving attacker will potentially notice the anomaly (incoming IP or MAC address mismatch, etc)

If you encounter no indication of previous compromise it's time to bring in your security operations team and allow them to have visibility over the segments of the network that may have been compromised - at this point it's still impossible to say if there has indeed been a compromise, as it's feasible that a skilled attacker will have hacked into that host, pivoted further into the network and potentially deleted any indication of compromise from the initially compromised host - but in the chance that there was, it's imperative to enable your SOC to analyze the traffic and determine if there is abnormal behavior on the network. At this point you'll be hard pressed to perform any further action: keep an eye on network traffic and be extra aggressive on any security events issued by your [IDS](https://en.wikipedia.org/wiki/Intrusion_detection_system) - if there is a time to be paranoid it's now.

Okay, the nitty gritty:

Again, all of the analysis is to be performed on the image/backup and not your production server:

- Review the system services software registry hives.
- Look at the ntuser.dat for user profile creation; take into consideration that a skilled attacker won't create users if there's the option to enable a previously disabled local user - user creation and deletion is often logged, user status change: less likely so.
- Pull the prefetch entries (show you application executions).
- Employ your packet sniffer of choice and look for abnormal traffic - it bears mention that just letting it sit there for a long period of time might yield some results, as the possibility of a call-back mechanism to the attacker is likely, configure a local IP address and let the packet sniffer do it's job... while you're at it:
- Check for abnormal scheduled tasks.
- Check the logs for anything at all that may seem suspicious, even time stamps: consider that if an attacker has been sanitizing log entries to hide their tracks, the hardest aspect to sanitize is time. If you see abnormal gaps in what should be recurring and consistent log entries it's probably worth digging further.

Hopefully that helps to steer folks in the right direction. The action items above are by no means a comprehensive list, the way to approach a compromised server can be as varied as the attack that breached it, and understanding what your penetration tester exposed is a great starting point - but also keep your security posture in check and take into consideration that a penetration test is not a security assessment, in all likelihood your pentester found a way in, exploited that vulnerability successfully and no other attack path was considered, so it's important to keep in mind the security life cycle of any given host and how a security assessment vs. a penetration test fall into that cycle; this is when a good dosage of humility comes into play and being cognizant that a host that had a critical vulnerability is a strong candidate for having other critical and high vulnerabilities. If there is a good time to apply the adage that "the best defense is a good offense", that time is now: be vigilant of that host and mitigate carefully. If there is an active malicious actor in your network and he or she isn't aware that the jig is up, use that leverage and don't let go of it until evidence of compromise materializes paired with a good grasp of the attacker's reach within the network, identification of data that may have been potentially damaged or exfiltrated and the method used to do so.

Often organizations, big and small, spend entirely too many resources and me trying to identify the threat actor, motives and even why the attack was staged... and those are all fair questions, but now it's time to identify a potential compromise, engage your blue team and mitigate the finding. Ego comes last.
