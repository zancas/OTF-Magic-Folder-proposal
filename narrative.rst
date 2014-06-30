.. -*- coding: utf-8-with-signature -*-


* ethical imperative: Human rights defenders have a "First do no harm" ethical imperative to protect the information of the people they work with.

* normalization: Human rights defenders, journalists, activists, and vulnerable populations use whatever tools have been featured in the media, e.g. Dropbox, rather than using specialized tools that are designed for their use case. Therefore to achieve the best outcomes the safe tool needs to be targeted at a wide user base, and be easy to discover and to use.

* sustainability: the Tahoe-LAFS project is a thriving open source community of independent hackers and organizations from around the world. Magic Folders will be exciting and welcome, and will attract support from many contributors. In addition, LeastAuthority intends to sell Magic Folder commercial services, and to continue to invest in improving it and producing complementary products and services.

===================
 Executive Summary
===================

Magic Folders will be an alternative to Dropbox that people in repressive
countries can use to collaborate and to share their work. Unlike Dropbox,
Magic Folders will protect the user’s files with end-to-end cryptography, so
that adversaries who compromise a server cannot spy on the files or inject
content into them.  Also unlike Dropbox, the entire stack of client and
server will be Free and Open Source, so that people who are blocked from
accessing Dropbox can deploy Magic Folders using only their own local
resources.

There are already tools available to people in politically repressive
countries to help them chat with one another and browse the web. But chatting
and web-browsing are only a subset of the things that we need in order to be
included in the free and open dialogue of the Internet. We also need to be
able to collaborate and to share our work with one another.

Dropbox is currently used extensively by human rights defenders, journalists,
and vulnerable people in repressive countries, both internally to ease the
transmission of data between colleagues, and to transfer topical data in a
timely way to safe repositories outside of the country. Danny O’Brien, of the
Electronic Frontier Foundation, confirmed that organizations like CPJ use
Dropbox in this way, and had heard of similar practices at Human Rights
Watch. Enrique Piracés, of Benetech, confirmed that Dropbox is used
extensively both by professional human rights defenders, by activists, and by
vulnerable populations in many nations.

Dropbox and other similar commercial services are intended for maximum
profitability and targeted at wealthy populations. They are not intended to
protect vulnerable or repressed users. All of the files of all Dropbox users
are exposed to the Dropbox servers and to the computers and laptops of
Dropbox’s employees. An adversary who exploited those could spy on or inject
content into any user’s files. The same is true of the other commercial
services, such as Google Drive.

In May of 2011, Dropbox was revealed to have lied when it claimed that its
users' files were inaccessible to Dropbox employees. `1`_

.. _1: http://www.wired.com/2011/05/dropbox-ftc/

¹ Singel, R “Dropbox Lied to Users About Data Security, Complaint to FTC Alleges” Wired (2011) http://www.wired.com/2011/05/dropbox-ftc/

And one month later, the potential impact was demonstrated when Dropbox
accidentally turned off user verification, allowing anyone on the Internet to
read or edit the files of any Dropbox user for 4 hours. `2`_

.. _2: http://www.tomsguide.com/us/dropbox-Arash-Ferdowski-cloud-storage-code-update-login,news-11576.html

² Parrish, K “Dropbox Accidently Turned Off Password for 4 Hrs” Tom's Guide (2011) http://www.tomsguide.com/us/dropbox-Arash-Ferdowski-cloud-storage-code-update-login,news-11576.html

In addition, since Dropbox and Google Drive are centralized services, it is
easy for repressive regimes to block access to those services. It is
impossible for users of those products to switch to another infrastructure
provider or to run their own infrastructure; instead they must rely on the
remote, proprietary, centralized services.

We propose to extend the Tahoe-LAFS storage system to into an easy-to-use
automatic file-synchronization system like Dropbox. Tahoe-LAFS is a
distributed storage technology in which all files are protected with
end-to-end cryptography. It has won praise and support from academic computer
science researchers, Richard Stallman, the Electronic Frontier Foundation,
the U.S. Defense Advanced Research Projects Agency, the U.S. National
Security Agency, and the Open Internet Tools Project, among others.
Tahoe-LAFS currently doesn’t offer the kind of intuitive, user-friendly
workflow and automatic synchronization that Dropbox and Google Drive do.

If we are successful, this technology will help empower people in repressive
countries to communicate, collaborate, and organize.

=======================
 Questions and Answers
=======================

Breathing Space or Convergence?
===============================

This technology changes the playing field by eliminating a centralized point
of vulnerability. Current file-sharing and synchronization tools are
vulnerable *both* at the endpoints where the users authenticate and perform
actions, *and* at the center, where the servers provide services and control
access.

With this technology, the vulnerability at the center is almost entirely
eliminated. The servers never have access to plaintext, encryption keys, or
signing keys, so there is no vulnerability to the servers with regard to the
confidentiality or data-integrity properties. In addition the remaining
functions of the servers are performed by a decentralized, dynamic set of
servers, reducing vulnerability to the servers for the availability and
reliability properties. I.e. the system is both cryptographically secure and
also robust against deletion, disruption, and denial-of-service attacks.

This negates the traditional attack of compromising the server in order to
gain access to *all* clients. Instead, an attacker will have to compromise
each client to gain access to that client. This changes the playing field.

How does this incentivize the adversary?
========================================

This incentivizes the adversary to focus their efforts on compromising the
endpoints, such as through phishing, social engineering, etc.

What is the asymmetry for this solution?
========================================

This solution focuses on reducing the concentration of vulnerability in the
central server(s). At a system-wide level, this solution asymmetrically
advantages the defenders, because it forces the attackers to expend more
resources if they want to attack a larger user base.

With this solution it is no longer the case that a single successful attack
(i.e., remote compromise of a server) gains power over *all* of the users of
a potentially large system such as Dropbox.

On the other hand, for a typical *individual* user, this solution doesn't
change their vulnerability much, because they are typically vulnerable to
attacks on their endpoint (P.C. or mobile device). This solution closes only
a single avenue of vulnerability (namely, an attack on their endpoint which
originates from the file-sharing/synchronization server), but it doesn't
protect them against other compromises of their endpoint.

How to defeat this effort?
==========================

To defeat this effort in the center would require breaking state-of-the-art
cryptography. Instead, focus your efforts on the endpoints and the human
factors. Access control is implemented in a decentralized way by sharing
secret encryption keys rather than by sending queries to a central server to
request access. Therefore, if you compromise the endpoints and gain a copy of
the encryption keys, there is no central access-control-server which has an
opportunity to detect that someone (you) just gained read access to some
data. In theory, the storage servers might be able to detect a pattern of
requests for ciphertext that could reveal your activities, but in practice
Tahoe-LAFS storage servers are typically not monitored closely, since they
store only ciphertext, and since they are unprivileged servers which can be
dynamically added and removed.

Another approach is focus on the human factors. If Tahoe-LAFS Magic Folders
are used casually for both sensitive and non-sensitive purposes (unlike
traditional secure file-sharing techniques such as encrypting with PGP), then
perhaps your targets will use them less carefully and make more
mistakes. Alternately, if Dropbox or Google Drive are easier to use than
Magic Folders is, then perhaps you can persuade your targets to use those
tools instead, especially if the more secure tool appears to be having
technical difficulties. So, consider performing a denial-of-service attack
which degrades the performance or reliability of the more secure tool, and
see if your targets switch over to using a file-sharing and synchronization
tool that you can break.
