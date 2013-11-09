# IMAPdedup
*A duplicate email message remover*

IMAPdedup is a Python script (imapdedup.py) that looks for duplicate messages in a set of IMAP mailboxes and tidies up all but the first copy of any duplicates found. 

To be more exact, it *marks* the second and later occurrences of a message as 'deleted' on the server.   Exactly what that does in your environment will depend on your mail server and your mail client. 

Some mail clients will let you still view such messages *in situ*, so you can take a look at what's happened before 'compacting' the mailbox.  Sometimes deleted messages appear in a 'Trash' folder.  Sometimes they are hidden and can be displayed and un-deleted if wanted, until they are purged.   

Whatever your system does, you will usually have the option to see what has been deleted, and to recover it if needed, using your email program, after running this script.

## How it works

By default, IMAPdedup will simply look for messages with a duplicate Message-ID header.  This is a string generated by email systems that should normally be unique for any given message, so unless you've got some rather unusual mailboxes, it's a pretty safe choice.  (Note that GMail, for example, *does* sometimes have some unusual mailboxes.)

If you have messages *without* a Message-ID header, or you don't trust it, there's an option (-c) to use a checksum of the To, From, Subject, Date, Cc & Bcc fields instead.

And if you want to add the Message-ID, if it exists, into this checksum, add the '-m' option as well. 


## Trying it out

If you want to experiment, create a new folder on your mail server, and copy some messages into it from your inbox.  Then copy some of them in a second time.  And maybe a third. That should give you a safe place to play!

## Simple use

You can list the full syntax by running 

    ./imapdedup.py -h

but the key options are described below.  You will of course need the address of your IMAP email server, and your username on that server.

Try starting with something harmless like:

    ./imapdedup.py -s imap.myisp.com -u myuserid -x -l

which prompts you for your password and then lists the mailboxes on the server. You can then use the mailbox names it returns in other commands. (The `-x` option specifies that the connection should use SSL, which is generally the case nowadays. If this doesn't work, you can leave it out, but you should probably also complain to your email provider because they aren't providing sufficient security!)

It's worth trying getting this list at least once because different mail servers structure their folders differently: mine thinks of all the folders as being 'within' the inbox, for example, so they're called things like 'INBOX.Drafts','INBOX.Sent', and those are the names I need to use when talking to the server.

Once you know your folder names, you can run something like

    ./imapdedup.py -s imap.myisp.com -u myuserid -x -n INBOX.Test

and the script will tell you what it would do to your *INBOX/Test* folder. 

The `-n` option tells IMAPdedup that this is a 'dry run': it stops it from *actually making* any changes; it's a good idea to run with this first unless you like living dangerously.  When you're ready, leave that out, and it will go ahead and mark your duplicate messages as deleted.  

The process can take some time on large folders or slow connections, so you may want to add the `-v` option to give you more information on how it's progressing.

You can specify multiple folders to work on, and it work through them in order and will delete, in the later folders, duplicates of messages that it has found either in those folders or in earlier ones.

## Acknowledgements etc

For more information, please see [the page on Quentin's site](http://qandr.org/quentin/software/imapdedup).

This software is released under the terms of the GPL v2.  See the included LICENCE.TXT for details.  

It comes with no warranties, express or implied; use at your own risk!

Many thanks to Liyu (Luke) Liu, Adam Horner and others for their contributions!

[Quentin Stafford-Fraser][1]

[1]:http://statusq.org

