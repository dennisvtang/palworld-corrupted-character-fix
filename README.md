Shutdowns are probably not necessary, but you do you in terms of how safe you want to be

# Useful information about a player's identification
guid
instance id
guild id
`GUID in hexadecimal + 24 zeros.sav`

# Determine corrupted player's save
- Determine which save file belongs to the corrupted person
- Unpack level.sav
This will take a while this is the map compressed





# Recover almost everything except the player's level and stats
- Temporarily move the corrupted save file somewhere and have the player join the server and create a character. The new character will have the same GUID in hexadecimal as the previous one.
- Shut down server and convert the new `.sav` file to json
- Search for `InstanceId` and copy the value for `Guid` thats there
- Convert the old `.sav` file to json. Search for `InstanceId` and replace the value for `Guid` thats there with the one you copied from the new `.sav`
- Convert the old save `.json` file back to a `.sav` file and place it back into the server
- Have them join and verify that they have everything, but their level and stats restored.




# Manually set player level
- Shutdown server

- Convert `level.sav` file to json
- Determine player's in game username and search for that in the file
- Right above the name should be a section called `Level`
- Change that


# Fix autoswing bug
The issue appears to be caused by the corrupted player's GUID being linked to multiple instance_ids in level.sav. Once in the player's data structure and again in the guild's data structure. This can be fixed by replacing the instance in the guild data structure to match the player's new instance id.

The issue appears to be caused by the corrupted player's data in the world saying it belongs to a guild, but in the data structure for the guild

If not implemented the fixed users will have issues related to holding down left click
- Open `level.sav.json` search for corrupted character's InstanceID guid.
 There should only be one

- source
<!-- https://steamcommunity.com/app/1623730/discussions/0/4132682727131950820/?ctp=4 -->



# Bugs
- Seemingly players that have been fixed this way level very fast?
- [bug related to left click?](https://steamcommunity.com/app/1623730/discussions/0/4132682727131950820/?ctp=5#:~:text=Izumemori%27s%20solution)