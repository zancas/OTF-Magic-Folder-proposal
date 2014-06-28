.. -*- coding: utf-8-with-signature -*-

===================
 Executive Summary
===================

We will make an alternative to Dropbox that people in repressive countries
can use to collaborate and to share their work. Unlike Dropbox, Magic Folder
will protect the user’s files with end-to-end crypto, so that adversaries
who compromise a server cannot spy on the files or inject content into them.
Also unlike Dropbox, the entire stack of client and server will be Free and
Open Source, so that people whose are blocked from accessing Dropbox can
deploy Magic Folder using only their own local resources.

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

.. _1: Singel, R “Dropbox Lied to Users About Data Security, Complaint to FTC Alleges” Wired (2011) http://www.wired.com/2011/05/dropbox-ftc/

And one month later, the potential impact was demonstrated when Dropbox
accidentally turned off user verification, allowing anyone on the Internet to
read or edit the files of any Dropbox user for 4 hours. `2`_

.. _2: Parrish, K “Dropbox Accidently Turned Off Password for 4 Hrs” Tom's Guide (2011) http://www.tomsguide.com/us/dropbox-Arash-Ferdowski-cloud-storage-code-update-login,news-11576.html

In addition, since Dropbox and Google Drive are centralized services, it is
easy for repressive regimes to block access to those services. It is
impossible for users of those products to switch to another infrastructure
provider or to run their own infrastructure; instead they must rely on the
remote, proprietary, centralized services.

We propose to extend the Tahoe-LAFS storage system to into an easy-to-use
automatic file-synchronization system like Dropbox. Tahoe-LAFS is a
distributed storage technology in which all files are protected with
end-to-end crypto. It has won praise and support from academic computer
science researchers, Richard Stallman, the Electronic Frontier Foundation,
the U.S. Defense Advanced Research Projects Agency, the U.S. National
Security Agency, and the Open Internet Tools Project, among others.
Tahoe-LAFS is currently doesn’t offer the kind of intuitive, user-friendly
workflow and automatic synchronization that Dropbox and Google Drive do.

If we are successful, this technology will help empower people in repressive
countries to communicate, collaborate, and organize.
