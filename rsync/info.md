Setting up an Rsync Daemon

First, using your preferred text editor, you’ll need to create the configuration file /etc/rsyncd.conf, if you do not have one already. Below is an example of our basic configuration parameters and explanations of each one.

pid file = /var/run/rsyncd.pid
lock file = /var/run/rsync.lock
log file = /var/log/rsync.log
port = 12000

[files]
path = /home/public_rsync
comment = RSYNC FILES
read only = true
timeout = 300

pid file: The process id file the daemon uses.

lock file: The daemon lock file.

log file: The location of the log file.

port: If you do not want the rsync daemon to run on its default port (873) then you may specify a new port here. Make sure this port is open in your firewall. Rsync uses the TCP protocol for its transfers.

[files]: This is the module name. The name used here is what you’ll be putting in the rsync pull command as the first part of the source (/files/../..). You can name it what you’d like and can have as many as you’d like.

path: The file path for files associated with this module.

comment: Descriptive comment for this module.

read only: This tells the daemon the directory for this module is read-only. You cannot upload to it. For upload only, use upload only = true.

timeout: In seconds, the rsync daemon will wait before terminating a dead conenction.

This is just a basic configuration. For a more detailed list of options, see the manual page.
Running Rsync as a Daemon

Now with this basic configuration we can start the daemon by itself by running the below:

rsync --daemon

You can verify the daemon is running with:

ps x | grep rsync

    If you have anything weird in the output, such as a statement stating unconfined, you may have SELinux blocking the daemon. You will need to work to add rsync to be accepted by SELinux in order for you to run the daemon.

.
Now that the rsync daemon is running, it’s ready to accept connections. If you are unsure how to do connect from an rsync client, review our guide on connecting with rsync.

To stop the daemon you can run a kill command.

kill `cat /var/run/rsyncd.pid`

Testing the Rysnc Directories

To test your connection to the rsync daemon and find which paths are available to you, simply connect from your client to the rsync host using the following method. This method runs only part of a pull command but will reveal paths for you.

rsync -rdt rsync://IPADDR:RsyncPort/

Find file path Find file path

This command will show which directories are open to you. If you do not know the file name you can repeat the process (adding onto the file path) until you find the intended file(s).

rsync -rdt rsync://IPADDR:RsyncPort/DirectoryName

More file paths discovered More file paths discovered

And once you find the file, you can complete the command and pull it in.

rsync -rdt rsync://IPADDR:RsyncPort/DirectoryName/File /DestinationDirectory/

Full path found Full path found