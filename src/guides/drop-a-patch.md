# Drop a patch

As we develop software sometimes we come up with ideas that we pretty quickly
decide aren't great. We generally want to throw this code away. In Git Patch
Stack because our patches represent logical changes we do this by simply
dropping a patch out of our stack.

## TL;DR

1. `gps rebase`
2. mark the commit we want to drop with `d` or `drop`
3. save and quit the editor

## WalkThrough

The `gps rebase` command is a convenience function that really runs an
interactive rebase of the stack on top of it's associated upstream, e.g.
`git rebase -i --onto origin/main origin/main main`.

So understanding how to drop a patch with this command is really simply
learning how to drop commits using git's interactive rebase.

Let's start with the following patch stack (`gps ls`).

![Starting patch stack](../images/guides/drop-a-patch/starting-patch-stack.png)

Let's say that "Add function B" is no longer need, and we just want to get rid
of it.

To do this we start by running `gps rebase` to kick off the interactive rebase.
It presents the following in our configured editor.

```
pick 57f6095 Add function A
pick 1a7252c Add function B
pick abf01ed Add function C

# Rebase 016b6ec..abf01ed onto 016b6ec (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
#         create a merge commit using the original merge commit's
#         message (or the oneline, if no original merge commit was
#         specified); use -c <commit> to reword the commit message
# u, update-ref <ref> = track a placeholder for the <ref> to be updated
#                       to this position in the new commits. The <ref> is
#                       updated at the end of the rebase
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

To drop the commit we simply mark it for dropping with either `d` or `drop`
instead of its default `pick`. If you forget the marking you can always
reference the comment Git includes in the interactive rebase buffer. So in this
case we mark the "Add function B" commit with `d` as follows.

*Note:* The stack is inverted when presented in an interactive rebase. So the
bottom most commit on the stack is actually the top most commit. This can be
confusing until you get used to it.

```
pick 57f6095 Add function A
d 1a7252c Add function B
pick abf01ed Add function C

# Rebase 016b6ec..abf01ed onto 016b6ec (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
#         create a merge commit using the original merge commit's
#         message (or the oneline, if no original merge commit was
#         specified); use -c <commit> to reword the commit message
# u, update-ref <ref> = track a placeholder for the <ref> to be updated
#                       to this position in the new commits. The <ref> is
#                       updated at the end of the rebase
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

We then save and quit the editor and it drops commits marked with `d` or `drop`
as part of the rebase. This leaves our patch stack as follows (`gps ls`).

![Patch stack after drop](../images/guides/drop-a-patch/patch-stack-after-drop.png)

Exactly the state we wanted it to be in.
