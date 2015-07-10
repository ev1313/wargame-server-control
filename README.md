Wargame server control
===========================
A Python 3 script for Wargame server automation

Features:
------------------
 - Enables you to implement custom logic on server events
 - Tracks current information about players on a server

Requirements:
-----------------
 - mcrcon
 - Python 3
 - Wargame series game

Usage:
------------------
 - Place it in the folder with your wargame server
 - Set up a path to mcrcon
 - Run script
 
Principle of work:
-------------------
 - Script reads server config and finds the last line, when there was nobody on the server
 - Then it starts to accumulate infomation about present state, by analyzing later log entries
 - Each log line is compared with registered masks (in register_events) and, if matches, fires a service event handler
 - Service event handler updates internal information structures about current game state
 - When it finishes, the info gather run consider being passed (info_run = false)
 - From now on script opens log and reads logs newly written lines (if any)
 - Now, when log line is matched against the mask, service handler runs user handler after inself.
 - User handlers are supposed to contain functions that perform actions (so they will not be performed during information gathering)
 
Workflow:
-------------------
 - User operates, depending on the information about current state of the game 
 (gamestate var), players information (players dictionary, keys - player id) and information of event happened (handler function arguments)
 - User code intended to be placed in functions in "Custom actions" section
 
 
Notes:
------------------
 - Events are registered on the proxy service handlers to hide all service calculation, that user should not be bothered with. 
 If needed, event could be registered on user function directly