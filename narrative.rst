

There are already tools available to people in repressive countries to help
them chat with one another and browse the web, such as Text Secure
(<https://whispersystems.org>) and Tor (<https://torproject.org>). But
chatting and web-browsing are only some of what we need in order to take part
in the free and open dialogue of the Internet. We also need the ability to
collaborate and to share our work.

Dropbox is currently used extensively by human rights defenders, journalists,
and vulnerable people in repressive countries, both internally to ease the
transmission of data between colleagues, and to transfer topical data in a
timely way to safe repositories outside of the country. Danny O’Brien, of the
Electronic Frontier Foundation, confirmed that organizations like Committee
to Protect Journalists use Dropbox in this way, and had heard of similar
practices at Human Rights Watch. Enrique Piracés, of Benetech, confirmed that
Dropbox is used extensively both by professional human rights defenders, by
activists, and by vulnerable populations in many nations.

Dropbox and other similar commercial services are intended for maximum
profitability and targeted at wealthy populations. They are not intended to
protect vulnerable or repressed users. All of the files of all Dropbox users
are exposed to the Dropbox servers and to the laptops of Dropbox’s
employees. An adversary who exploited any of those could spy on or alter any
user’s files. The same is true of the other commercial services, such as
Google Drive.

In May of 2011, Dropbox was revealed to have lied when it claimed that its
users' files were inaccessible to Dropbox employees ¹.

And one month later, the potential impact was demonstrated when Dropbox
accidentally turned off user verification, allowing anyone on the Internet to
read or edit the files of any Dropbox user for 4 hours ².

In addition, since Dropbox and its competitors are centralized services, it
is easy for repressive regimes to block access to those services. This
provides repressive regimes with an easy method of forcing users of those
services to fall-back to alternative means of collaboration, possibly as a
prelude to exploiting those users, or for political or business purposes. In
June of 2014, Dropbox was found to have been blocked, again, throughout
China ³.

It is impossible for users who rely on those products to switch to another
service provider or to run their own server.

We propose to extend the Tahoe-LAFS secure storage system to into an easy
file-synchronization system like Dropbox. Tahoe-LAFS is a distributed storage
technology in which all files are protected with end-to-end cryptography. It
has won praise and support from academic computer scientists, Richard
Stallman, the Electronic Frontier Foundation, the U.S. Defense Advanced
Research Projects Agency, the U.S. National Security Agency, and the Open
Internet Tools Project, among others. Tahoe-LAFS currently doesn’t offer the
kind of intuitive, user-friendly workflow that Dropbox and Google Drive do.

If we are successful, this technology will help empower people in repressive
countries to communicate, collaborate, and organize, without incurring
unnecessary risk of being exploited or censored by repressive regimes.

¹ Singel, R “Dropbox Lied to Users About Data Security, Complaint to FTC Alleges” Wired (2011) <http://www.wired.com/2011/05/dropbox-ftc/>

² Parrish, K “Dropbox Accidently Turned Off Password for 4 Hrs” Tom's Guide (2011) <http://www.tomsguide.com/us/dropbox-Arash-Ferdowski-cloud-storage-code-update-login,news-11576.html>

³ Russell, J “Dropbox is blocked again in China, both apps and web-based service appear affected” (2014) http://thenextweb.com/asia/2014/06/19/dropbox-blocked-china-ending-four-month-period-access/

Normalization and Sustainability
================================

Human rights defenders have an ethical imperative to “First, do no harm” in
handling information about the vulnerable people with whom they work. At the
same time, they can't afford to learn about and train up on bespoke
tools. Human rights defenders, journalists, activists, and the people they
work with will tend to use whatever tools are normal in their communities and
make it easy to get their job done.

Therefore to have a a large positive impact we need *normalization* and
*sustainability*. We aim to make Magic Folders an easy, reliable tool for
daily work so that it can be the normal way to collaborate.

To address sustainability, we bring to bear practices from startup culture,
such as “Minimal Viable Product”, to reduce risks:

* Magic Folders could fail to fit the needs of users,
* Magic Folders could arrive too late to help,
* Magic Folders could prove economically unsustainable and could become
  abandoned or ill-maintained.

See the “Sustainability” section for more detail about our strategy to
mitigate such risks.

Technical Description
=====================

Leverage the existing functionality
-----------------------------------

The strategy for Magic Folders is to make a “Minimum Viable Product” by
leveraging Tahoe-LAFS's existing functionality. Tahoe-LAFS already has a
`tahoe backup` command which efficiently uploads any files which have been
changed since the most recent run of `tahoe backup`, and which preserves
backup copies of older versions of each file. Magic Folders will extend this
in two ways to make a minimum viable alternative to Dropbox:

1. Hook into the local filesystem so that whenever files or directories are
   changed in the local magic folder, this automatically triggers a run of
   `tahoe backup` to upload any new changes from that folder.

2. Monitor such changes being made to the remote magic folder, and download
   each new or changed file into the local one.

In order to shorten time-to-market, the process of monitoring for changes
from the remote side will be a simple polling loop that re-uses Tahoe-LAFS's
existing functionality, instead of a more efficient asynchronous notification
system that would require more development time.

How Do You Magically Link Two Folders?
--------------------------------------

A design question which centrally impacts both usability and security is how
the user specifies which folders are to be magically linked with which other
folders (on other devices). If the process of linking two remote folders to
each other is cumbersome or confusing, this may prevent users from adopting
Magic Folders. At the same time, if the process is centralized or spoofable,
then this would afford attackers the opportunity to exploit users by
injecting an attack into the process of linking two folders together.

We will design a simple process based on sending an short “invitation” code
from one device to the other and entering it into the second device. This is
a trade-off with good decentralization and security properties, with
hopefully acceptable usability, and (very importantly) with earliest
time-to-market for the Minimum Viable Product.

Managing simultaneous changes to shared files
---------------------------------------------

When two or more users are making simultaneous edits to shared files and
directories, this can lead to subtle and complicated problems. Our approach
is to follow the example of the early versions of Dropbox, which gained great
marketshare without attempting to automatically resolve such complicated
situations. Instead, the minimum viable product needs only two things:

1. To reliably detect and display to the user when such conflicts have
occurred, and

2. To provide enough information to the user that they can manually resolve
the conflict.

The latter information has to include access to previous versions of the
files.

Questions and Answers
=====================

Breathing Space or Convergence?
-------------------------------

This technology changes the playing field by eliminating a central point of
vulnerability. Current file-sharing and synchronization tools are vulnerable
*both* at the endpoints where the users authenticate and perform actions,
*and* at the center, where the servers provide services and control access.

With this technology, the vulnerability at the center is almost entirely
eliminated. The servers never have access to plaintext, encryption keys, or
signing keys, so users are not vulnerable to the servers with regard to
confidentiality or data-integrity. In addition the remaining functions of the
servers are performed by a decentralized, dynamic set of servers, reducing
vulnerability to the servers for availability and reliability. I.e. the
system is both cryptographically secure and is also robust against deletion,
disruption, and denial-of-service attacks.

This negates the traditional attack of compromising the server in order to
gain access to *all* clients. Instead, an attacker will have to compromise
each client to gain access to that client. This changes the playing field.

What is the asymmetry for this solution?
----------------------------------------

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
--------------------------

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
