

Objective 1—Design local filesystem integration
===============================================

*Objective 1* is to write the design document for how to integrate with the
local filesystem on Linux and Windows platforms.

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
provided by Windows — `ReadDirectoryChangesW` ¹ and `NTFS change journals` ².

To detect local filesystem changes on Linux there is an API named
`inotify` ³. We already use it for the filesystem monitoring in the current
version of Tahoe-LAFS. This design is to make it scalable and reliable.

The deliverable for *Objective 1* is a document explaining the interface
between the sync client and the local filesystem, how the sync client will
register to watch subdirectories as they are created or moved into the magic
folder, and when the sync client will need to perform a full rescan. It will
also explain what APIs or libraries the sync client will use to watch the
filesystem on the various supported platforms.

¹ ReadDirectoryChangesW <http://msdn.microsoft.com/en-us/library/aa365465%28v=VS.85%29.aspx>

² NTFS change journals: <http://msdn.microsoft.com/en-us/library/aa363798%28v=VS.85%29.aspx>

³ inotify <http://en.wikipedia.org/wiki/Inotify>

Objective 2—Implement local filesystem integration
==================================================

*Objective 2* is to implement filesystem integration on Linux and Windows, as
defined by *Objecive 1*. The deliverable for *Objective 2* is a code-base
(satisfying the requirements listed in the “Project Evaluation” section) for
those platforms.

Objective 3—Design algorithm for remote-to-local sync
=====================================================

*Objective 3* is to write the design document for how to do remote-to-local sync. There are two parts to this algorithm:

1. How to efficiently determine which objects (files and directories) have to
   be downloaded in order to bring the current local filesystem into sync
   with the newly-discovered version of the remote filesystem.

It is necessary that this algorithm discovers all new changes committed by
the remote-side user, because failure to include any such change could lead
to data-loss, which is unacceptable. However, it is also important for the
algorithm to minimize reads (tests) and downloads, in order to have good
performance and to minimize unnecessary load on the network.

.. Go back and find our ideas from discussion of the Team Sync project. In that proposal I wrote " *** ^-- HERE BE DRAGONS. We have a few good ideas about how to subdue these ". Now I'd like to add those ideas to this document, maybe.

2. How to overwrite the (stale) local versions of those objects with the
   newly acquired objects, while preserving backed-up versions of those
   overwritten objects in case the user didn't want this overwrite and wants
   to recover the old version.

To ensure this recoverability property, the download process needs to backup
the current (stale) local object, then overwrite it atomically (using the
local filesystem's mv/rename operation) with the newly downloaded version.

.. Also needed. Also from the Team Sync proposal: " *** ^-- MORE DRAGONS. Yesterday Daira came up with a good hack to subdue this dragon, too. Also ellided. "

Objective 4—Implement algorithm for remove-to-local-sync
========================================================

*Objective 4* is to implement the remote-to-local sync algorithm as defined
by *Objective 3*. The deliverable for *Objective 4* is a code-base
implementing this and satisfying the requirements listed in the “Project
Evaluation” section.

Objective 5—Design folder-linking user interface
================================================

*Objective 5* is to write the design document for how users can conveniently
and securely indicate which folders on some devices should be magically
linked to which folders on other devices.

As described in the “Narrative” section, this is a critical usability and
security issue for which there is no known perfect solution, but which will
hopefully be amenable to a "good enough" trade-off solution. The deliverable
for *Objective 5* is a document explaining this design and justifying its
trade-offs in terms of security, usability, and time-to-market.

Objective 6—Implement folder-linking user interface
===================================================

*Objective 6* is to implement the folder-linking process as defined by
*Objective 5*. The deliverable for *Objective 6* is a code-base implementing
this and satisfying the requirements listed in the “Project Evaluation”
section.
