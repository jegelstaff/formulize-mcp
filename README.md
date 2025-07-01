# formulize-mcp
A local MCP server for connecting to Formulize systems on the web. 

Release 1.2.2\
July 1 2025\
Happy Canada Day!

## Step 1 - Node.js

You don't need to download anything from here (but you could, see below). You just need to have [node.js installed on your system](https://nodejs.org/en/download).

Once you do, all that's left is for you to configure your AI assistant(s).

## Configuration

### 1. Get a key for your Formulize system

Once you have node.js installed, you need to get an API key from your Formulize system, which will allow the MCP server to access Formulize. 

- If you are the webmaster, this is easy. Go to the _Manage API Keys_ page on the admin side, and issue yourself a key.
- If you are not the webmaster, ask the webmaster to issue you a key.

Once you have the key, configure your AI assistant:

### 2. Configure your AI assistant

Things are changing fast in the world of MCP. In the future, this local MCP server won't even be necessary, we hope. Microsoft wants to own the configuration of this on Windows by building it all into the OS. We'll see how that goes.

For now, here's some examples of .json files that will allow different AI assistants to use this local MCP server:

#### Claude Desktop

You need to modify the ```claude_desktop_config.json``` file. Where is it? 

- Windows: ```%APPDATA%\Claude\claude_desktop_config.json```
- macOS: ```~/Library/Application Support/Claude/claude_desktop_config.json```
- linux: ```~/.config/Claude/claude_desktop_config.json```

```json
{
  "mcpServers": {
    "Formulize": {
      "command": "npx",
      "args": [
        "-y",
        "formulize-mcp"
      ],
      "env": {
        "FORMULIZE_BASE_URL": "https://<your.formulize.site.url>/mcp/",
        "FORMULIZE_API_KEY": "<your api key from your formulize site>"
      }
    }
  }
}
```

#### VSCode (with Copilot)

There are a few ways, but the easiest is probably to just make a ```.vscode``` folder inside whatever project you're working in, or make a new one if you just want to use the AI and Formulize. Inside that folder, make a ```mcp.json``` file that looks a lot like the Claude version:

```json
{
  "servers": {
    "Formulize": {
      "command": "npx",
      "args": [
        "-y",
        "formulize-mcp"
      ],
      "env": {
        "FORMULIZE_BASE_URL": "https://<your.formulize.site.url>/mcp/",
        "FORMULIZE_API_KEY": "<your api key from your formulize site>"
      }
    }
  }
}
```

The only difference is one calls it _servers_ and the other calls it _mpcServers_! Guys, get your acts together!

Other AI assistants are similar, if they support MCP at all. If they do support it, they probably only support tools. _Claude Desktop_ and _Copilot in VSCode_ support tools, resources and prompts, abeit in rather different ways.

__Your mileage may vary!__

### Environment Variables

- FORMULIZE_BASE_URL - Required - The URL for the mcp folder of your Formulize system, ie: https://yoursite.com/mcp/
- FORMULIZE_API_KEY - Required - Your API key for your Formulize system
- FORMULIZE_DEBUG - Optional - Either _true_ or _false_. Defaults to _false_.
- FORMULIZE_TIMEOUT - Optional - Timeout in milliseconds. Defaults to _30000_.

All values __must be enclosed in double quotes__.

## (Optional) Installation

There is no installation step required, but you might prefer to install the server permanently, and not have npx running it all the time.

If you do, the installation is super simple of course:

```bash
npm install -g formulize-mcp
```

If you do this, then you need to change the .json config file for your AI assistant:

- ```npx``` becomes ```formulize-mcp```
- ```args``` becomes ```[]``` (ie: empty, no args)

## (Very optional) Compiling it yourself

Maybe you don't want to to rely on mysterious precompiled code from an NPM registry. But this is open source after all, and for a reason, so you do you.

### 1. Download the latest release from this Github project

### 2. Go to the folder where you downloaded it

```bash
cd folder/with/the/files
```

On Windows, make sure you're doing this in a Command Prompt or other window that as _Administrator_ privileges!

### 3. Install the MCP SDK first

```bash
npm install @modelcontextprotocol/sdk
```

If you get import errors, try installing the latest MCP SDK:

```bash
npm install @modelcontextprotocol/sdk@latest
```

### 4. Install remaining dependencies and build the project

```bash
npm install
npm run build
```

If you do all this, then you need to change the .json config file for your AI assistant: 

- ```npx``` becomes ```node```
- ```-y``` argument is removed
- ```formulize-mcp``` becomes full path to the compiled .js file.

On Windows, don't forget to use double slashes ( \\\\ ) for directory separators in the path. :(
