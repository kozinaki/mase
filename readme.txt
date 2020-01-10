==============================
 INDEX OF README FOR MASE:BC2
==============================
A. KNOWN BUGS
B. SOURCE CODE
C. CHANGELOG
D. CREDITS

===============
 A. KNOWN BUGS
===============

[B] Friend system does not work
[A] This hasn't been implemented (and probably never will be from me because it's not worth spending time on)

[B] Server queuing does not work
[A] This hasn't been implemented (properly) since the required packets could not be logged

[B] Global ranks does not work
[A] Same thing as Friend system

[B] Punkbuster enabled server doesn't show up as enabled in the server list (not really a bug since its intended)
[A] I permanently disabled Punkbuster to show up in the list, otherwise players who have Punkbuster disabled wouldn't be able to join ranked servers

[B] Unable to use character 'x' in user/persona name but it works on EA's server
[A] I did specify some forbidden characters to avoid problems with the file saving

[B] I still get connected to EA in the Client and the Hook recognizes the Client as a Server (check with setting "show_console=1" in the bfbc2.ini and look at the output in the cmd window)
[A] That might be the case for Steam or Origin versions, you can override automatic detection by setting "executable_type=client" or "executable_type=0" in the bfbc2.ini, then it should work (assuming you really have the latest version)



================
 B. SOURCE CODE
================

Requirements:
-------------
- Visual Studio 2010 or higher (Windows), gcc 4.7 or higher (Linux)
- Boost C++ libraries v1.54.0 (you might have build them with the parameter "cxxflags=-std=gnu++11" in Linux to avoid some linking issues with boost::filesystem)
- OpenSSL v1.0.1j
(newer versions might work too but the ones listed here were used to build the latest version of the emulator)

Usage:
------
- Project files are included with the source files in the "src" folder 
- [Windows] mase_bc2.sln/.vcxproj/.vcxproj.filters for VS2010
- [Linux] mase_bc2.cbp for C::B (should also be possible to generate a makefile from it)
- To be able to properly build it you'll need to change the Include and Linker directories in the project settings first, replace all occurrences of "$(BOOST_1_54_DIR)" and "$(OPEN_SSL_DIR)" with the locations where you installed these libraries

Final Notes:
------------
I have written ~95% of the code myself (regarding the source files that I included here, code from the external libraries doesn't count of course), the rest I either copied 1:1 or just slightly modified it for my needs.
The according code sections in the files are:
- base64 de/encryption (in Base64.cpp/h, credits for the original author are included in the .cpp file)
- Packet de/encryption and SSL certificate information (in SSL_cert.h and TcpConnections(SSL).cpp), I originally took it from the v10.0 emulator source from Domo/Freaky123 but I it seems they took it from aluigi's EAList tool (http://aluigi.altervista.org/papers/ealist.zip), so I guess credits go to him?
- Http server code, mostly taken from the boost::asio example page (http://www.boost.org/doc/libs/1_54_0/doc/html/boost_asio/examples/cpp11_examples.html), only dumped it down and slightly modified it for my needs

Also I would like to mention that when I wrote this project, I was rather new to coding bigger stuff in general (though it certainly was a great experience for me) and I would probably do quite a few things differently nowadays if I had the time and motivation to do it.
So if you consider any of the code that I have written as bad/terrible regarding performance/structure/functioning then feel free to change anything you want or if thats too much work for you, just don't use the entire emulator and look for a different solution (I'm not forcing you to use it).
With that being said though, if you do change something in the code and release it somewhere, I would very much appreciate it if you would at least credit me for my work :)

Last but not least, you may be wondering why I didn't include the source code for the hook.
The answer is simply because I don't think anything new would come out of it, the already implemented features should be more than enough to play around with and as far as I can tell there should be no bugs left (that could be fixed in the hook).
But if you are concerned about there being some "malicious" code in it, I can only assure you that the code is free from such things.
And if you don't believe me feel free not to use it, the hook is just meant as a bonus anyway and you can always do the required modifications yourself to redirect the game client/server to the emulator (as described in section A, point 3, b)



==============
 C. CHANGELOG
==============

### Beta 0.9

[ADD] source code for the emulator which you can find in the 'src' directory, I probably won't continue working on it so feel free to mess around with it
[ADD] missing signal handler for Linux version (forgot to add it when I went through porting the required code)
[ADD] ranking_min_players, unlimited_ammo and health_mode options to the hook
[FIXED] problem with the signal handler for Windows version (would not exit on first Ctrl+C and crash on doing it a second time)
[CHANGED] from OpenSSL 1.0.1e to 1.0.1j (too lazy to update boost libraries as well)
[CHANGED] logbuffer size to 8192 (which basically means bigger packets will be cut off in the log but I decided its better than having a giant buffer for just a few messages to be displayed entirely)
[REMOVED] automatic creation of a default bfbc2/config.ini for the hook/emulator if if there is no file with such a name in the same directory (not sure why I thought this was a good idea in the first place, its just taking up space in the compiled library/executable)


### Beta 0.8

[ADD] Linux 32-bit version (can't guarantee that it works as good as the Windows version though)
[ADD] some code improvements in the hook and support for another version of the client than the patched retail (see Known Bugs if it still doesn't work)
[ADD] simple IP filter to differentiate between public and private IP's and pass them properly in the packets
[ADD] theoretical support for specifying hostnames instead of IP's in the config.ini (practically no testing was done)
[ADD] Display the number of current connections in the window title (Windows only since there is no similar solution possible in Linux)
[FIXED] socket on misc_port not working correctly
[REMOVED] Server limit and Client limit (though I can't tell at which point the emulator might get unstable since no testing has been done in that direction so if you are planning to connect a lot of Clients/Servers to the emulator you do this at your own risk)
[REMOVED] Age restriction in the Client when creating a new account


### Beta 0.7

[ADD] "Remember Password" and "Auto Login" features (see F.A.Q. for more details)
[ADD] automatic creation of a default bfbc2.ini for the hook if there is no file with such a name in the same directory
[ADD] "all_stats_unlocked" in the config.ini which (if enabled) toggles to a stats_unlocked template file that has the necessary values to unlock every weapon for a newly created persona (this replaces the previous global_template feature)
[FIXED] ports in "ECHO" packet being constant
[FIXED] some wrong content in the "NuLookupUserInfo" packet when the user was not found
[FIXED] Client login problems that some users experienced who previously played on Nexus's server
[FIXED] a major bug in the Stats system which would cause the emulator to stop displaying any messages and ultimatively not to respond to incoming connections any more
[FIXED] a bug in the config reading of the hook which would lead to incorrect values for the hook to use
[CHANGED] from OpenSSL 1.0.1c to 1.0.1e, from boost version 1.51 to 1.54 and replaced some standard library functions with the ones from boost
[CHANGED] some keynames in the config.ini, removed a few unimportant keys and made sure the emulator wont start up until the IP is set
[CHANGED] the filename "bfbc2cfg.ini" to just "bfbc2.ini" (the old file is obsolete and can be deleted if you use the newer hook) and made sure the user will be notified if no IP is set (users can still use the old hook if they prefer it for some reason)


### Beta 0.6

[ADD] customizable TOS which is stored in ".\templates\termsOfService" (original TOS is in termsOfService_orig, to use it replace the files)
[ADD] dinput8.dll for the Client and Servers which makes the hosts and lpatch modifications obsolete, 
this file is NOT required by the emulator and must be put into the client/server root directory to make use of it,
root directory is where the "BFBC2Game.exe"/"Frost.Game.Main_Win32_Final.exe" is located, see F.A.Q. section for more info
[ADD] check if the required subdirectories of the emulator ('database' and 'templates') are present
[ADD] check if the user who wants to delete a persona really is the owner of that persona
[FIXED] a possible ID mess up when removing personas
[FIXED] Server disconnect when something was written in chat with "ProfanityFilter enabled"
[FIXED] issue where the emulator would think the name length was out of boundaries if special characters were used and the name length was within the boundaries
[CHANGED] database saving method, temporary files will not be deleted if something goes wrong when loading/saving and now the .lst files are constantly updated (instead of .tmp)
[CHANGED] the UpdateStats method, not found values will no longer be written to "ignoredKeys" file (since they are all only used for webstats), 
perhaps they will be included in the stats template in future versions (if there is demand for it)
[CHANGED] config.ini options, added "window_buffer_size" which sets a fixed maximum for lines displayed in the console (dynamic is still possible), 
removed "theater_server_ip" and "theater_server_ip" since "emulator_ip" can easily be used instead and if misc_port is set to 0 the whole socket will be disabled
[CHANGED] Punkbuster filter settings, the emulator will never flag as "Punkbuster enabled" even if it is enabled, furthermore the according filter on the Client will be ignored,
this is done because the dinput8.dll will permanently disable Punkbuster on the Client and on the Server anyway so there is no sense in supporting it
[RELEASE] all versions from here on will be publicly released on private-servers.info and youtube.com


### Beta 0.5
	
[ADD] http protocol support for the emulator (plus an "http_enabled" option to turn the socket on/off)
[ADD] missing packet responds for bfbc2.gos, easo.ea.com and an additional connection
[ADD] "misc_port", "http_enabled" and "http_updater_respond" options to config.ini, check the comments of the options for more info about them
[ADD] "game", "version" and "installerConfig" files to templates folder, they are only accessed during http requests
[FIXED] a crash that could occur when a persona was created
[FIXED] once again possible memory leaks, improved critical sections and performance in some parts
[CHANGED] database saving method, now changes of personas/users are automatically updated in external temporary files which replace the original files on quit, 
previously changes were only applied on the original files on quit, however changes to these files while the emulator is running are not applied (and should not be done)
[CHANGED] user/persona name length boundaries to (min=3 and max=32)/(min=3 and max=20)
[CHANGED] maximum allowed client connections to 64
[RELEASE] Closed Beta release on private-servers.info


### Beta 0.4

[ADD] "Play Now" feature (it says "Connection lost" when there are no servers found but thats not the case and can be ignored)
[ADD] "display_database_table_info" option to config.ini which makes it easier to read database related stuff (use this only for debugging purposes)
[FIXED] some possible memory leaks and improved a few code sections
[FIXED] a weird bug where the client would endlessly request GetRankedStatsForOwners packets (for some reason I didn't notice that before)
[FIXED] warning message that would occur when a special UpdateStats packet from the server is being received
[FIXED] crash that would occur when the emulator tries to send something to the database after being connected 8+ hours without traffic
[FIXED] problem with some special characters in server names/descriptions, now all the characters that work on EA should also work here
[CHANGED] the database, completely removed everything MySQL related and replaced it with a self-written piece of code, 
this might not be more efficient for big datasets but should be for small ones (also this fixed the above mentioned database related bugs),
note that when database entries are displayed on the console they are not affected by "message_cut_off_length"
[CHANGED] the server limit from 2 to 4
[RELEASE] Closed Beta release on private-servers.info

### Beta 0.3

[ADD] account registration for new users is now entirely possible from the client (like it's normally done, except serial request)
[ADD] credits for the emulator (they are displayed instead of EA's license agreement when a user wants to create an account :p )
[ADD] automatic creation of the MySQL database if the specified database name is not found (the specified user must have the necessary rights to do this!)
[ADD] server queues where a client would be put in a queue when the server is full, however this feature is EXPERMIENTAL, I couldn't test it yet
[ADD] options "emulator_ip", "use_global_server_version" and "enable_server_filters" to the config.ini
[ADD] server filtering when a client searches for servers (all options should be working as intended)
[ADD] connection count limit for clients and server for security reasons, 
if testing goes well and people don't bitch too much about it then I will increase this limit in future versions
[FIXED] connection/packet problems when two clients would join the same server almost at the same time (finally!), now the emulator should be able to handle this stuff,
I couldn't try that with more people though (limited resources) but it should also work
[FIXED] not setting servers/clients to 'online=0' in the database that were still connected when quitting the emulator (this is a hacked fix, I hope to do it properly later...)
[CHANGED] MySQL database structure, also renamed many columns to give them a more suitable name
[RELEASE] Closed Beta release on private-servers.info


### Beta 0.2

[ADD] automatic base64 de/encoding of packets that are larger than the maximum packet data, the encoded data gets partitioned and can be safely sent/recieved, 
this was previously done manually (and the stats got collected a little faster that way) but since some packets are variable in size it's better this way
[ADD] Stats updating, this is still WIP though (and it requires the client to be connected to a ranked server)
[ADD] Dogtags saving in persona specific '.dog' files, this is also WIP
[ADD] "countryList" file in "templates" folder where all the available countries are specified when one wants to create a new account,
structure is like this: 'ISO_Code'='Country Name'
[ADD] "ignoredKeys" file in "templates" folder where all the keys get stored that are not client relevant (these are only useful for webstats)
[ADD] '[connection]' section to config.ini where one can specify the ports on which the emulator is listening for incoming connections and the according ip's that are being sent
[ADD] '[emulator]' section to config.ini where one can specify options that affect all clients that are connecting to the masterserver
[ADD] KICK packet when client leaves a server (the process seemed to work fine without that but it appears always in the official backend logs...)
[ADD] various safety checks when a client logs in and proper responses for each one (does the account exist, is the password correct, is nobody logged in with that account, etc),
this will also prevent that users can login with the server accounts (this is not even done on EA's server o.O), there are also checks when creating a persona
[FIXED] and improved the the server status update packets, players on the server are now shown correctly when players join or leave the game
[FIXED] a rare packet counter mixup that would occur when a client joins a server (and would deny him to join)


### Beta 0.1

[STATUS] rewrote the source code from scratch, only the SSL certificate, packet header encoding/decoding and base64 code remain untouched,
Packet class has a much better performance, Stats are more reliable and the overall stability has much improved (especially when (multiple) Clients are joining a Server)
[ADD] Ping and Memcheck mechanism for both Clients and Servers and for each of their connections (Theater and Plasma, Memcheck only for Plasma)
[ADD] colors to the console to differentiate between client and servers and plasma and theater of each one (see F.A.Q section for color reference)
[FIXED] some important packets that had the wrong content in it and sometimes caused unexpected results (crashes, disconnections, etc)
[FIXED] the log file writing, now it is not only much more reliable but also it should be guaranteed that everything gets written into the file before a quit/crash
[FIXED] the structure of the data that gets sent to the client/(game)server, it is now exactly like EA's packet structure
[CHANGED] the speration of theater and plasma in two different executables, now everything is in one executable,
this might not be a good way to deal with many Client and Server connections but is certainly easier to use for LAN games (which I am aiming for with this project)
[CHANGED] the number version of a connecting server is now a fixed version and not the real one (this ensures that every connected server is visible for all clients)
[CHANGED] the location of the "templates.txt" stats file into an own "templates" folder (renamed to "stats")
[CHANGED] the config.ini (adjusted to the rewritten source) and redid the whole reading/saving code for it as well (more reliable now)
[CHANGED] the GUI, mouse icon, exectuable icon and the Listbox for message logging are removed (that was just fancy stuff), everything is now displayed in console only


### Alpha 0.0.9

[ADD] Database update of the user/server whether he is online or not (if a server is not online he doesn't get listed in the server list)
[ADD] Specact and Vietnam DLC entitlements for all users, 
one can now view and play with Specact weapons in main menu and change to vietnam mode to join a server which runs in vietnam mode
[ADD] automatic creation of a default theater/plasma config.ini if it cannot be found (only MySQL information has to be manually inserted)
[FIXED] Stats are being sent correctly to the game server, they are not updated when a client leaves though


### Alpha 0.0.8

[ADD] successful connection from client to server!! It's even stable until client disconnects or map is changed (client remains connected to server though),
stats are not loaded yet to server so the stats are temporarily saved on server (you start from zero everytime), works yet only on localhost though
[ADD] a few events of the server are now updated (in database and therefore also on client) when anything changes (server on/offline, player joined/disconnected)
[FIXED] Server listing bug that wouldn't show any servers when user searches a second time and would also cause connection problems
[FIXED] again a problem where the emulator would answer to the wrong packets
[FIXED] a MySQL query crash that would come up when the server is not listed in database - he is now correctly added
[CHANGED] some packet responds of theater and plasma to work better and respond more appropriate
[RELEASE] Public release on thedefaced.net/private-servers.info, both Source Code and compiled binaries


### Alpha 0.0.7

[FIXED] Stats getting, the client now successfully gets all the stats from the emulator! :D
[FIXED] a bug where the emulator would answer to the wrong packets if he sends linked packets
[CHANGED] to a new base64 de-/encryption method, old one included strange newlines in the data that made it difficult to calculate size of encoded data
[CHANGED] again the Stats collecting method, it's now almost instant (on a decent PC) ;)
[CHANGED] Stats persona stats storing method while client is connected, 
they should be loaded now for every new client that connects and not eventually overwrite old ones (still need to try though)


### Alpha 0.0.6

[FIXED] small bug that wouldn't return the right stats value from the persona stats file
[FIXED] automatic timeout of connected client after some time  (disconnected due lack of activity)
[FIXED] a few console log bugs
[FIXED] Stats decoding method, now all of the recieving data is decoded properly
[CHANGED] Stats collecting method, it is now a lot more efficient
[CHANGED] Stats getting and saving method from the persona file, it hopefully works better and faster now


### Alpha 0.0.5

[ADD] Error messages for various events where the emulator would have crashed before without any indication what happened
[ADD] Font quality option "font_quality", look in the FAQ for the different options
[ADD] Option "message_cut_off_length" to absolutely set the message length to be displayed in the console, put -1 for no limit and 0 for empty lines,
useful for very long strings that would cause a nasty flickering (and perhaps perfomance issues?)
[ADD] an "About" MessageBox :p
[FIXED] Some eventual memory allocation problems
[CHANGED] restructured the packet recieving code of Plasma, it checks now first for packet type then TXN (if it exists)


### Alpha 0.0.4

[ADD] Option "file_create" for not creating a logfile
[ADD] Option "message_limit_length" to limit length of debug messages, put 0 for no limit (not recommended, messages over limit might not show up),
recommandable values are between 1000 and 3500 for default font (Tahoma, 14)
[FIXED] Heavy CPU usage of Theater after Server login
[FIXED] Annoying random crash on startup of Plasma (hopefully)
[FIXED] Some Bugs that came up with the GUI
[CHANGED] format of the debug output strings to fit for the listbox window (also server orientated)


### Alpha 0.0.3

[ADD] Icon (modified original :p ) and Bfbc2 cursor
[ADD] Font options "font_name", "font_size", "font_italic", "font_underlined", "font_striked" in config (only ANSI_CHARSET fonts are allowed)
[ADD] Changelog and version numbering
[FIXED] Small bugfixes here and there
[FIXED] Heavy CPU usage in idle - Windows handles this for us now
[CHANGED] Console window to a GUI window (style is server orientated ;) )
[CHANGED] removed unnecessary column in the database that was used by the emulator


### Alpha 0.0.2

[ADD] Game Server can connect to Emulator
[ADD] Multiple users can connect to Emulator
[ADD] Server is shown in Client server list (when Server is listed as online in database)
[ADD] Server details are also showing up in server list (though not every detail yet)


### Alpha 0.0.1

[ADD] Persona adding/removing
[FIXED] Login moreless (no Stats loading yet)
[FIXED] a few other client sided events
[STATUS] running again
[CHANGED] Switched to a local file saving for stats of the Personas
[RELEASE] inofficial release on battlefieldemulator.net


### Pre-Alpha 10.0

[STATUS] Not running but Base code is available (CPacket/CSocket/Threads/etc)
[RELEASE] Public release on sourceforge.net (by Freaky123 and Domo)



============
 D. CREDITS
============
Developers (Emulator+Hook):
- Triver

Packet Logging and Server stuff:
- Triver
- Rodney (after Alpha 0.8)
- bcool (up to Alpha 0.8)

Beta Testers:
- Rodney
- Jack
- Michl2007
- Happy Chicken

Special Thanks:
- Domo and Freaky123 (for sharing the Server Files and the v10.0 source code)
- Aluigi (for the tools that made it possible to view and record the BF:BC2 network traffic)
- DICE (for making the game)
