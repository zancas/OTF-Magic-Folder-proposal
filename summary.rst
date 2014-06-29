.. -*- coding: utf-8-with-signature -*-

===================
 Executive Summary
===================

Magic Folder will be an alternative to Dropbox that people in
repressive countries can use to collaborate and to share their
work. Unlike Dropbox, Magic Folder will protect the user’s files with
end-to-end cryptography, so that adversaries who compromise a server cannot
spy on the files or inject content into them.  Also unlike Dropbox,
the entire stack of client and server will be Free and Open Source, so
that people who are blocked from accessing Dropbox can deploy Magic
Folder using only their own local resources.

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

=========
 Metrics
=========

Functionality
=============

The most important measure of success is if the resulting technology provides
the desired functionality (without sacrificing security). The desired
functionality is that the system can be used to share and synchronize files
and directories across multiple devices and users.

Despite the simplicity of this user experience, to develop a technology that
implements it is not easy. It requires the development of a novel secure,
decentralized file-synchronization protocol to provide this “simple”
functionality while preserving Tahoe-LAFS's security and fault-tolerance.

In order to be usable, the result will have to be *understandable*,
*performant*, and *reliable*.

Understandability
=================

The measure of understandability is whether the behavior of the system is
predictable or explainable by the user. For example, if a user has a file
named “foo.doc”, which upon being edited simultaneously by two users, is
copied into two files, named “foo.doc (John's conflicted copy 2014-01-02)”
and “foo.doc (Bill's conflicted copy 2014-01-02)”, then they will be able to
understand what happened.

If instead, the file has silently been overwritten with one or the other
version, they may not realize what has happened and may have difficulty
explaining the result `¹⁴`_, `¹⁵`_, `¹⁶`_.

.. Some users have even reported that terse distinct filenames such as “foo.doc.1” and “foo.doc.2” are confusing, but informative filenames such as “foo.doc (John's conflicted copy 2014-01-02)” and “foo.doc (Bill's conflicted copy 2014-01-02)” are not.

Reliability
===========

Users avoid tools that occasionally cause problems. In order for users to use
the technology, they will require that it never accidentally loses the user's
data or their edits, and that once a user requests an action, the technology
never silently fails to complete it, which would force the user to monitor
whether the action completes and repeat the request if it doesn't.

For example, if the user indicates that a file should be shared by dragging
and dropping it into the magic folder, then the file will *eventually* be
shared (when the network connection to the servers is working), even if the
network connection to the servers was not working at the time the
drag-and-drop was first performed, *without* the user having to repeat the
drag-and-drop.

Performance
===========

Users prefer services that have low and steady latency. Two key metrics of
the performance of the Tahoe-LAFS Magic Folders will be the latency of
propagation of updates, and the latency of
stabilization/conflict-resolution. Latency will necessarily be a sum of terms
outside of our control, such as the performance of the network, and terms
inherent to our design. The *added* latency, not inherent to the performance
of the network, will be considered acceptable if it is less than 10 seconds,
and excellent if it is less than 0.1 seconds (100 milliseconds).
