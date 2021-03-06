﻿.. -*- coding: utf-8-with-signature -*-

============
 Objectives
============

Objective 1—Design local filesystem integration
===============================================

*Objective 1* is to write the design document for how to integrate with the
local filesystem on multiple platforms.

The integration has to be efficient; it can't rescan the entire local magic
directory repeatedly as this would cost too much system load for the user to
tolerate. It has to be scalable in supporting large numbers of files and deep
nested directory structure. It has to be reliable, because if this mechanism
failed to notice an update to a file or a newly added file, this could result
in mysterious data loss, which is unacceptable to users.

Tahoe-LAFS already has a working, but limited, implementation of this
filesystem integration. It works only on Linux, and doesn't monitor nested
subdirectories, but it is otherwise adequate, and we can learn from it when
designing its successor.

We also have an incomplete implementation — not yet committed to Tahoe-LAFS
trunk — of filesystem monitoring for Windows.

The sync client will still need to recursively scan the entire magic folder
sometimes. Due to platform limitations, queue overflows, the local sync
client being stopped and restarted, or other reasons, asynchronous
notification of local filesystem changes may not provide a complete set of
changes. To regain a complete state, the local sync client will need to scan
the entire magic folder in certain conditions, such as when the sync client
is started up.

To detect local filesystem changes on Windows there are at least two APIs
provided by Windows — `ReadDirectoryChangesW`_ and `NTFS change journals`_.

.. _ReadDirectoryChangesW: http://msdn.microsoft.com/en-us/library/aa365465%28v=VS.85%29.aspx
.. _NTFS change journals: http://msdn.microsoft.com/en-us/library/aa363798%28v=VS.85%29.aspx

To detect local filesystem changes on Linux there is an API named
`inotify`_. We already use it for the filesystem monitoring in the current
version of Tahoe-LAFS. This design is to make it scalable and reliable.

.. _inotify: http://en.wikipedia.org/wiki/Inotify

To detect local filesystem changes on Mac OS X there is an API named
`FSEvents`_. All BSDs including Mac OS X offer `kqueue/kevent`_. 

.. _FSEvents: http://en.wikipedia.org/wiki/FSEvents
.. _kqueue/kevent: http://en.wikipedia.org/wiki/Kqueue

The deliverable for *Objective 1* is a document explaining the interface
between the sync client and the local filesystem, how the sync client will
register to watch subdirectories as they are created or moved into the magic
folder, and when the sync client will need to perform a full rescan. It will
also explain what APIs or libraries the sync client will use to watch the
filesystem on the various supported platforms.

Objective 2—Implement local filesystem integration
==================================================

*Objective 2* is to implement filesystem integration on Windows, Linux, and
Mac OS X/BSD, as defined by *Objecive 1*. The deliverable for *Objective 2*
is a code-base (satisfying the requirements listed in `Project Evaluation`_)
for those platforms.

Objective 3—Design algorithm for remote-to-local sync
=====================================================

*Objective 3* is to write the design document for how to do remote-to-local
 sync. There are two parts to this algorithm:

1. How to efficiently determine which objects (files and directories) have to
 be downloaded in order to bring the current local filesystem into sync with
 the newly-discovered version of the remote filesystem.

It is necessary that this algorithm discovers all new changes committed by
the remote-side user, because failure to include any such change could lead
to data-loss, which is unacceptable. However, it is also important for the
algorithm to minimize reads (tests) and downloads, in order to have good
performance and to minimize unnecessary load on the network.

.. Go back and find our ideas from discussion of the Team Sync project. In that proposal I wrote " *** ^-- HERE BE DRAGONS. We have a few good ideas about how to subdue these ". Now I'd like to add those ideas to this document, maybe.

2. How to overwrite the (stale) local versions of those objects with the
newly acquired objects, while preserving backed-up versions of those
overwritten objects in case the user didn't want this overwrite and wants to
recover the old version.

.. Also needed. Also from the Team Sync proposal: " *** ^-- MORE DRAGONS. Yesterday Daira came up with a good hack to subdue this dragon, too. Also ellided. "

Objective 4—Implement algorithm for remove-to-local-sync
========================================================

*Objective 4* is to implement the remote-to-local sync algorithm as defined
by *Objective 3*. The deliverable for *Objective 4* is a code-base
(satisfying the requirements listed in `Project Evaluation`_).

