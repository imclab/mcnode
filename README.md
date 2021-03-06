# MCNode

A Python API for running and extending a Minecraft server.

# Examples

## Make and Extend a Server
This downloads the Minecraft server jar, and starts up a server with the specified options.
GreetBot will also greet people by name when they log in.

```python
# minecraft_server.py

from mcnode import MCNode
from greet_bot import GreetBot

node = MCNode({ 
    	'server_jar_url' : 'https://s3.amazonaws.com/MinecraftDownload/launcher/minecraft_server.jar',
		'server_directory' : './server/',
		'server_jar_path' : 'minecraft_server.jar',
		'init_memory' : '512M',
		'max_memory' : '1024M'
})

node.add_bot(GreetBot(node))
node.read()
```

```python
# greet_bot.py

from mcbot import MCBot

class GreetBot(MCBot):
	def on_connect(self, data):
		self.node.say('Welcome, %s' % (data[3]))
```

Then, in a command line, run `python2 minecraft_server.py`
> Note: the `python2` command may differ by system. Check `python`, `python2.7`, etc. It just needs to be Python 2 (probably 2.7)

# Installation

### Fetch from Github
> `git clone git://github.com/laneaasen/mcnode.git`

### Install pexpect (bundled with MCNode)
> ```bash
cd mcnode/include/pexpect/
sudo python2 setup.py install
```
>> Note: the `python2` command may differ by system. Check `python`, `python2.7`, etc. It just needs to be Python 2 (probably 2.7)

### Run the Example Server
> ```bash
python2 main.py
```

### Log into the Server on Minecraft
> It will be running on `localhost`. To configure the Minecraft server itself, open up the server directory (`server/` by default) and edit `server.properties`. 

# The Full API

## Communicating with the Server

#### `node.tell(message)`
> Passes an arbitrary string into the server.
> Functions like `node.say()`, `node.ban()` and `node.tp()` all extend `node.tell()`.

#### `node.say(message)`
> Wrapper for Minecraft's say function.
> ```python
def say(self, message):
	return self.tell('say ' + message)
```

## Killing the Server

#### `node.stop()` 
> Politely asks the server to stop.
> ```python
def stop(self):
    return self.tell('stop')
```

#### `node.terminate()`
> Uses `SIGTERM` (or Windows equivalent) to shut down the server. Usually won't cause data loss, but `node.stop()` is still advised.

#### `node.kill()`
> Uses `SIGKILL` (or Windows equivalent) to kill the server. This is not advised as it may cause data loss.
