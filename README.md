# mutt-mailboxes

`mutt-mailboxes` is a mailbox sorter for the `mutt` sidebar, written in Python.
It is used with the `mutt` `mailboxes` command to put the mailboxes in your
preferred order.

It is distributed under the terms of the MIT license.

## Configuration

### mutt-mailboxes

There are (currently) no configuration files for `mutt-mailboxes`, mostly due
to being dissatisfied with `configparser` not having list support and not
wanting to include any dependencies not in the Python standard library.

Configuration is performed via command line arguments, of which there are 3:
  * `--base | -b`
    * Specify the root directory of your Maildirs. This argument is mandatory.
  * `--initial | -i`
    * Used to specify the mailboxes which will appear first in the mailbox list.
      This argument is optional and may be given multiple times.
  * `--exclude | -e`
    * Used to exclude mailboxes so that they don't appear in the list in `mutt`
      at all.
      This argument is optional and may be given multiple times.

### mutt

A very minimal configuration for `mutt` could be:

```
set sidebar_visible=yes
set sidebar_sort_method=unsorted # IMPORTANT: We're going to use `mutt-mailboxes` to sort it.
set sidebar_short_path=yes
set sidebar_folder_indent=yes
set folder="~/Maildir"

# Assumes that `mutt-mailboxes` is somewhere under PATH
mailboxes `mutt-mailboxes \
	--base ~/Maildir \
`
```

## Usage

The most basic usage of `mutt-mailboxes` would be to simply spew out a list of
your mailboxes in alphabetical order.

```shell
mutt-mailboxes \
	--base ~/Maildir
```

`mutt-mailboxes` would then throw out a list of mailboxes under `~/Maildir` in
alphabetical order in a format that the `mutt` `mailboxes` command understands.

For a simply mailbox, it might throw out:

```
+'Archive' +'Drafts' +'INBOX' +'Junk Mail' +'Trash'
```

You'll notice that these paths are relative to the path given as `--base`, as
such, `--base` should probably match your `mutt` `folder` setting.

Now, most people would probably like their `INBOX` to be listed as the very
first mailbox in the sidebar. We can do that as follows:

```shell
mutt-mailboxes \
	--base ~/Maildir \
	--initial INBOX
```

Which would return:

```
+'INBOX' +'Archive' +'Drafts' +'Junk Mail' +'Trash'
```

We see that `INBOX` has now moved to the first position.

Now, maybe you don't care about `Junk Mail` and you'd like `Drafts` to be ahead
of `Archive`. This is fine, `--initial` can be specified multiple times to
order multiple folders and we can exclude `Junk Mail` with `--exclude`.

```shell
mutt-mailboxes \
	--base ~/Maildir \
	--initial INBOX \
	--initial Drafts \
	--exclude "Junk Mail"
```

This gives us:

```
+'INBOX' +'Drafts' +'Archive' +'Trash'
```

You see that the initial folders appear in the order that the `--initial`
arguments were specified in.
`--exclude` may also be repeated to exclude more folders from the listing.
