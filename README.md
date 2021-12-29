# dropper_thing
A badly written dropper in c#. Takes the malware file in base64 format. No this isn't meant to be used in any "real" engagements as is, but can be built off of if you want to deal with my bad code. I have tested it and it does work, just never remembered to finish it and clean it up.

The idea was to run the dropper on startup and to wait a certain amount of time using sleep() checking if the file needed to be updated or not. In other words, install on startup using some method (scheduling tasks is better than registry or somehow running from a service), select a name from the running processes, select appdata or temp, decode b64, run malware, sleep, check if the dropper was updated or new file was added.
