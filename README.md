# BACKUP YOUR SAVES AND WORLD

# Notes
- Tested on saves generated on Palworld version v0.1.2.0. v0.1.3.0 didn't fix the issue
- Shutdowns in the instructions below are probably not necessary, but were done when fixes were implemented and further testing hasn't been done
- Fixes were implemented on a server running on windows from the Palworld Dedicated Server tool from the steam app
- Fixed corrupted saves were people that weren't the host, not tested if its the host thats corrupted



# Determine player's GUID
This section will go over identifying a player's GUID which will be used in subsequent sections and can be skipped if already known.
Palworld player save files use the following format:  `Player's GUID in hexadecimal + 24 zeros.sav`. Here are example filenames.

    ```
    3EAD1F3B000000000000000000000000.sav
    B32DE5E5000000000000000000000000.sav
    ```

## Determine player's GUID through level.sav
A player's GUID can be determined through the map's `level.sav` file.
- Shutdown server
- Convert the server's `level.sav` to a json file with [palworld-save-tools](https://github.com/cheahjs/palworld-save-tools) this will result in a file called `level.sav.json`. This process may take a while.
- In the resulting `level.sav.json` file search for the section containing the list of players in a guild. This can be done either by searching for a player's in game nickname, a player's GUID in hexadecimal, or a guild name. The section should look similar to below
![alt text](/res/sample_guild_players_section.png)
- In this section make note of the corrupted player's `player_uid`. This is their GUID in hexadecimal.



# Recover almost everything except the player's level and stats
- Shutdown server
- Using the `player_uid`/`GUID` identified in the previous section locate their save file.
- Temporarily move the corrupted save file somewhere and have the player join the server and create a character. Remember players always have the same `player_uid`/`GUID`.
- Convert the newly created `.sav` file to json with with [palworld-save-tools](https://github.com/cheahjs/palworld-save-tools)
- Search for `InstanceId` and copy the value for `Guid` thats there
- Convert the old `.sav` file to json with [palworld-save-tools](https://github.com/cheahjs/palworld-save-tools)
- In the old `.sav.json` file search for `InstanceId` and replace the value for `Guid` thats there with the one you copied from the new `.sav`
- Convert the old save `.json` file back to a `.sav` file with with [palworld-save-tools](https://github.com/cheahjs/palworld-save-tools) and place it back into the server
- At this point the corrupted player should be able to join once the server is running and will have everything, but their level and stats restored.
- Might be necessary to have them join at this point to verify they have their stuff


# Fix hold left click bug
The bug prevents the user from holding left click to continuously swing.

The issue appears to be caused by the corrupted player's `player_uid`/`GUID` being linked to multiple `instance_ids` in level.sav. Once in the player's data structure and again in a guild's data structure. This can be fixed by replacing the instance in the guild data structure to match the player's new instance id.
- Shutdown server
- Open `level.sav.json` search for corrupted player's old `InstanceId`. There should only be one result. It should be in the "individual_character_handle_ids" section of a guild's section. An example of this is shown below.
![alt text](/res/sample_guild_character_ids_section.png)
- Replace that `InstanceId` with the `InstanceId` from the newly created `.sav.json` file from before



# Manually set player level and statuses
> Note not tested

- Shutdown server
- Convert `level.sav` file to json with with [palworld-save-tools](https://github.com/cheahjs/palworld-save-tools)
- Determine player's in game username and search for that in the file
- Right above the name should be a section called `Level` and `Exp`
- Change those values accordingly.


# Bugs
- Seemingly players that have been fixed this way level very fast?


# Credits
- cheahjs for creating [palworld-save-tools](https://github.com/cheahjs/palworld-save-tools)
- People in this [thread](https://steamcommunity.com/app/1623730/discussions/0/4132682727131950820/?ctp=4#:~:text=Izumemori-,Jan%2022%20%40%201%3A15pm,-3), specifically Izumemori for explaining the glitch and process of recovering everything, but the player level and status.- People in this [thread](https://steamcommunity.com/app/1623730/discussions/0/4132682727131950820/?ctp=4#:~:text=Izumemori-,Jan%2022%20%40%201%3A15pm,-3), specifically Izumemori for explaining the glitch and process of recovering everything, but the player level and status.
When someone leaves the guild I think the games "refreshes" the list of people on the server. People experiencing the glitch are missing their main chunk of player data from level.sav