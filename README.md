# Google-Compute-Engine-Keeping-Your-Process-Running-After-SSH-Logout-GNU-Screen
When using Google Compute Engine, you will need to SSH into your remote machine by using the gcutil.

So it would be something like:


        gcutil --service_version="v1beta15" --project="my_project" ssh  --zone="us-central2-a" "my_instance"

Great, we're in. But now, what if we want to run a certain process - for example, compile / build some code that's going to take a while? or maybe start our Erlang server? 

If we trust ssh then we are bound to fail - the connection will eventually break (we can try to keep it alive by automatic pinging and other methods, but what if we lose connectivity in our office / client?).

The solution: GNU screen. This awesome little tool let's you run a process after you've ssh'ed into your remote server, and then detach from it - leaving it running like it would run in the foreground (not stopped in the background).

So after we've ssh'ed into our GCE VM, we will need to:
1. install GNU screen: 


        apt-get update
        apt-get upgrade
        apt-get install screen

2. type "screen". this will open up a new screen - kind of similar in look & feel to what "clear" would result in. 
3. run the process (e.g.: ./init-dev.sh to fire up a ChicagoBoss erlang server)
4. type: Ctrl + A, and then Ctrl + D. This will detach your screen session but leave your processes running!
5. feel free to close the SSH terminal. whenever you feel like it, ssh back into your GCE VM, and type screen -r to resume your previously detached session.
6. to kill all detached screens, run:


        screen -ls | grep pts | cut -d. -f1 | awk '{print $1}' | xargs kill
