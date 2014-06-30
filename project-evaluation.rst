.. -*- coding: utf-8-with-signature -*-

====================
 Project Evaluation
====================

The evaluation of this project will be in whether it meets a set of technical
functionality, usability, and code-quality requirements. Functionality and
usability goals are the reason for the project's existence: to provide a safe
and usable way to share files.

The code-quality goals are necessary for long-term sustainability, for
widespread adoption of the result by the wider community of Free and Open
Source programmers, and they are also necessary for security.

Functionality
=============

The most important measure of success is if the resulting technology provides
the desired functionality (without sacrificing security). The desired
functionality is that the system can be used to share and synchronize files
and directories across multiple devices and users.

Despite the simplicity of the resulting user experience, to develop an
implementation of it is not easy. It requires the development of a novel
secure, decentralized file-synchronization protocol to provide this “simple”
functionality while preserving Tahoe-LAFS's security and fault-tolerance.

* *Data preservation*: the contents of a file will never be lost except when
  the user(s) have explicitly requested deletion or overwrite of that
  file. In particular, even simultaneous edit of the same file by different
  users at the same time will result in the preservation of all of the edits
  performed by all users (although possibly not all under the same old
  filename).

* *Least-Authority Access Control*: users can grant read-only access or
  read-write access to any subset of their files or any subtree of their
  directory structure.

* *Deduplication*: if the same file (the same sequence of bytes) is shared by
  multiple users or multiple devices, the ciphertext will be deduplicated so
  that only one copy of the file is stored on the servers, and only the first
  device has to do a complete upload of the data.

* *Stabilization*: when the user(s) have initiated changes to the system such
  as by saving new files or new versions of extant files into the Magic
  Folders, then after enough time has passed the system enters a quiescent
  state where no further changes to any of the Magic Folders will occur.

* *Forward Progress* and *Fairness*: if there is a continuous stream of new
  changes initiated such that the system never has a chance to quiesce, then
  at least such operations as it *can* complete it will complete, and display
  the results to the user.

  For example, if one file is repeatedly updated, as would be the case with a
  log file, then the system is required to continue making progress on
  synchronizing other files without waiting for that file to stop being
  updated. Likewise, if a file is repeatedly updating, the system is required
  to replicate *some* new version of the file instead of waiting to see what
  the "final" new version of the file will be.

Usability
=========

In order to be usable, it will have to be *understandable*, *reliable*, and
*performant*.

Understandable
--------------

The measure of understandability is whether the behavior of the system is
explainable by the user. For example, if a user has a file named “foo.doc”,
which upon being edited simultaneously by two users, is copied into two
files, named “foo.doc (John's conflicted copy 2014-01-02)” and “foo.doc
(Bill's conflicted copy 2014-01-02)”, then they will be able to understand
what happened.

If instead, the file has silently been overwritten with one or the other
version, they may not realize what has happened and may have difficulty
explaining the result `¹⁴`_, `¹⁵`_, `¹⁶`_.

.. Some users have even reported that terse distinct filenames such as “foo.doc.1” and “foo.doc.2” are confusing, but informative filenames such as “foo.doc (John's conflicted copy 2014-01-02)” and “foo.doc (Bill's conflicted copy 2014-01-02)” are not.

Reliable
--------

Users avoid tools that occasionally cause problems. In order for people to be
willing to rely on Magic Folders, they will require that it never
accidentally loses the user's data or their changes, and that once a user
requests an action, the technology never silently fails to complete it. If
Magic Folders were to occasionally silently fail to perform the requested
action, this would force the user to monitor whether the action completes and
repeat the request if it doesn't.

For example, if the user indicates that a file should be shared by dragging
and dropping it into a Magic Folder, then the file will *eventually* be
shared (when the network connection to the servers is working), even if the
network connection to the servers was not working at the time the
drag-and-drop was first performed, *without* the user having to repeat the
drag-and-drop action.

Performant
----------

Users prefer services that have low and steady latency. Two key metrics of
the performance of Magic Folders will be the latency of propagation of
updates, and the latency of stabilization/conflict-resolution. Latency will
necessarily be a sum of terms outside of our control, such as the performance
of the network, and terms inherent to our design. The *added* latency, not
caused by the performance of the network, will be considered acceptable if it
is less than 10 seconds, and excellent if it is less than 0.1 seconds (100
milliseconds).

Quality Requirements
====================

* *Unit tests*: all code contributed to the Tahoe-LAFS project is required to
  have thorough unit tests. To meet this standard, we develop the code and
  the unit tests together, using a code coverage tool to show us visually
  which lines of code are executed by the unit tests.

* *Documentation and open source publication*: We will contribute all of the
  implementation source code to the Tahoe-LAFS project under the terms of its
  Free Software/Open Source Licences. This maximizes the opportunities for
  peer review including security auditing by open source contributors, for
  benefit to the public, and for other works to be built on top of this
  one.

  To have a chance of acceptance into the Tahoe-LAFS project, we have to
  follow that project's coding standards and quality standards, including
  thorough developer-oriented and user-oriented documentation.

* *Statistics and logging*: the Magic Folder process exports operational
  statistics about performance and a record of exceptions or failures.

* *Failure handling*: handling of failures either by retrying or by raising
  an informative error message (in addition to the logging mentioned above).

