Go to https://jiandong.wang/NBT2cmds/ (given by the challenge description). If you view the source you can see that the
site uses a firebase database to store builds, the apiKey for which is stored in the source of the site. By looking further into the js
code we can see that builds are stored with this line:
```js
db.ref("user/"+auth.currentUser.uid+"/"+fileName).set(JSON.parse(output));
```
- `auth.currentUser.uid` is what it sounds like, we can change this to be anything we like so long as we remember it
- `fileName` is simply an identifier of the file
- `output` is the list of commands to be run by the computer

In order to decide what command to inject, we can look towards the [minecraft wiki](https://minecraft.wiki/w/Commands)'s command list.
The tellraw command can access minecraft storage, so we'll use that: `/tellraw username {"storage": "minecraft:secret", "nbt": "flag"}`.
The [tellraw documentation](https://minecraft.wiki/w/Commands/tellraw) explains the syntax, and the
[raw JSON string documentation](https://minecraft.wiki/w/Raw_JSON_text_format) explains the json string

Knowing these we can change the line to be this:
```js
db.ref("user/injector/injection").set(["/tellraw username {"storage": "minecraft:secret", "nbt": "flag"}"]))
```
Then it's as simple as logging onto the server, running `!get_computer` to first get a computer, then `!get_build injector/injection`
to run the command, you should see a message containing the flag.