---
layout: post
title: 'What is a Red Team?'
date: '2022-01-07'
description: 'The term Red Teaming is frequently used in the realm of cyber security. Its meaning has evolved over time due to a variety of factors, including vendors misusing the term in marketing. As a foundation for the course content, I will make an effort to provide an accurate definition of red teams - what they are, what they do, and why they do it.'
coverimage: What_is_a_Red_Team.jpg
tags: penetration-testing red-teaming
published: true
posttype: article
categories: article
---

#### What is red teaming?
The term "Red Teaming" is frequently used in the realm of cyber security. Its meaning has evolved over time due to a variety of factors, including vendors misusing the term in marketing and a misunderstanding of compliance requirements. 

As a foundation for the course content, I will make an effort to provide an accurate definition of red teams - what they are, what they do, and why they do it (and perhaps just as importantly, what they are not). 

A good dictionary definition is provided by Joe Vest and James Tubberville:

> Red Teaming is the process of using tactics, techniques and procedures (TTPs) to emulate a real-world threat, with the goal of measuring the effectiveness of the people, processes and technologies used to defend an environment.

#### What are the benefits of red teaming?
Red teams offer an adversarial viewpoint by attacking an organisation's and defenders' assumptions. 

Assumptions like "We are secure because we patch," "Only X number of people can access that system," and "Technology Y would stop that" are dangerous and frequently fail to hold up to scrutiny. A red team can identify areas for improvement in an organisation's operational defence by challenging these assumptions.

#### Penetration testing vs. red teaming

Even though there is some overlap with penetration testing, I'd like to highlight some key differences.

A typical penetration test will focus on a single technology stack, either because it is part of the project lifecycle or as a compliance requirement, (e.g. monthly or annual assessments). The objectives are to identify as many vulnerabilities as possible, demonstrate how those vulnerabilities can be exploited, and provide some contextual risk ratings. The output is usually a report detailing each vulnerability and remediation action, such as installing a patch or reconfiguring some software. There is no explicit focus on detection or response, no assessment of people or processes, and no specific goal other than "exploit the system(s)."

In contrast, red teams have a clear objective defined by the organisation - whether that be to gain access to a particular system, email account, database or file share. After all, organisations are defending "something," and jeopardising the confidentiality, integrity, and/or availability of that "something" represents a real risk, whether financial or reputational. A red team will also emulate a real-life threat to the organisation. 

For example, a finance company may be at risk from known FIN groups. In the case of a penetration test, a tester will simply use their personal preferred TTPs, whereas a red team will study and re-use (where appropriate) the TTPs of the threat being simulated. This enables the organisation to develop detections and processes to combat the specific threat(s) they expect to face. Red teams will also look holistically at an organisation's overall security posture rather than being laser-focused on one specific area; This includes people, processes, and technology, of course. 

Finally, red teams put a heavy emphasis on stealth and the "principal of least privilege". To challenge the detection and response capabilities, red teamers must complete their objective without being detected. A part of this includes not pursuing high-privileged accounts (such as Domain Admin) unnecessarily; If "Bob from Accounting" can access the objective, then that will do.

#### Breach Model

The breach model outlines how the red team intends to gain access to the target environment. This is typically accomplished by attempting to gain access in accordance with the threat (for example, via OSINT and phishing); Or by the organisation providing the access (often called "assume breach").

There are pros and cons for each approach depending on the objective(s) of the assessment.

If no assume breach is chosen and the red team attempts to gain access, it is essential to have a contingency plan in place in the event that access is not gained within the predetermined time period. A possible compromise is to revert to an assume breach model if the team does not gain access within the first 25% of the engagement timeframe. This is critical because red team assessments are more concerned with detection and response than with prevention, and thus those aspects of the assessment are more critical than attempting to "prove" that a breach can occur in the first place.

#### Notifications and announcements

Red team assessments are often arranged by upper management in security or compliance roles, and they face a choice as to whether to notify the entire organisation or a subset of it about an upcoming engagement. They may choose to inform nobody, everyone, or only the appropriate security/support teams.

Not providing any notification allows everyone to react as they would on any given day, which results in the most authentic outcomes. However, security teams may feel as though they are being tested or not trusted by management, which can result in strained relationships and negative outcomes.

On the other hand, prior notification may cause security teams to be extra vigilant or to temporarily increase security measures, which does not reflect their normal security posture.

Ultimately, the "correct" decision should come down to the existing culture and relationships within the organisation.