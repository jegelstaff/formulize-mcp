# formulize-mcp
A local MCP server for connecting to [Formulize](https://formulize.org) systems on the web. Use your AI assistant to get information from Formulize, and send information to it. Use AI to create forms, create entries, examine the configuration, summarize data... The sky's the limit!

Release 1.3.1\
Author: Claude (mostly Sonnet 4, and a little Opus 4)\
Architect: Julian Egelstaff\
July 10 2025

You probably already have a [Formulize](https://formulize.org) system, otherwise why are you here? But in case you didn't know...

[Formulize](https://formulize.org) is an open source data management platform. With [Formulize](https://formulize.org), you can create web-based forms, connect them together to make unique apps, and publish the data with interactive reports.

[Formulize](https://formulize.org) is quickly configured, and reconfigured, so it adapts as your needs change and your data grows. It's also completely free and open source.

## Step 1 - Node.js

You don't need to use ```npm```, or download anything from Github (but you could, see below). You just need to have [node.js installed on your system](https://nodejs.org/en/download).

Once you do, all that's left is for you to configure your AI assistant(s).

## Step 2 - Configuration 

### First, get a key for your Formulize system

Once you have node.js installed, you need to get an API key from your Formulize system, which will allow the MCP server to access Formulize. 

- If you are the webmaster, this is easy. Go to the _Manage API Keys_ page on the admin side, and issue yourself a key.
- If you are not the webmaster, ask the webmaster to issue you a key.

Once you have the key, configure your AI assistant:

### Second, configure your AI assistant

Things are changing fast in the world of MCP. In the future, this local MCP server may not even be necessary. We hope it becomes easy to connect AI assistants directly to remote Formulize instances. Microsoft wants to own the configuration of all this on Windows by building it all into the OS. We'll see how that goes. Everyone is trying to stake their claim.

In the meantime, you have to configure each AI assistant individually. Here's some examples of .json files that will allow different AI assistants to use this local MCP server, with no installation required:

#### Claude Desktop

You need to modify the ```claude_desktop_config.json``` file. Where is it? 

- Windows: ```%APPDATA%\Claude\claude_desktop_config.json```
- macOS: ```~/Library/Application Support/Claude/claude_desktop_config.json```
- linux: ```~/.config/Claude/claude_desktop_config.json```

```json
{
  "mcpServers": {
    "My Formulize MCP Server": {
      "command": "npx",
      "args": [
        "-y",
        "formulize-mcp"
      ],
      "env": {
        "FORMULIZE_URL": "https://<your.formulize.site.url>",
        "FORMULIZE_API_KEY": "<your api key from your formulize site>",
        "FORMULIZE_SERVER_NAME": "My Formulize MCP Server"
      }
    }
  }
}
```

#### VSCode (with Copilot)

There are a few ways, but the easiest is probably to just make a ```.vscode``` folder inside whatever project you're working in. Or make a new project if you just want to use the AI and Formulize. Inside the ```.vscode``` folder, make a ```mcp.json``` file, that looks a lot like the Claude version:

```json
{
  "servers": {
    "My Formulize MCP Server": {
      "command": "npx",
      "args": [
        "-y",
        "formulize-mcp"
      ],
      "env": {
        "FORMULIZE_URL": "https://<your.formulize.site.url>",
        "FORMULIZE_API_KEY": "<your api key from your formulize site>",
        "FORMULIZE_SERVER_NAME": "My Formulize MCP Server"
      }
    }
  }
}
```

The only difference is one calls it _servers_ and the other calls it _mpcServers_! Guys, get your acts together!

Other AI assistants are similar, if they support MCP at all. If they do support it, they probably only support tools. _Claude Desktop_ and _Copilot in VSCode_ support tools, resources and prompts, abeit in rather different ways.

__Your mileage may vary!__

### Environment Variables

- FORMULIZE_URL - Required - The URL for your Formulize system, ie: https://myformulize.com
- FORMULIZE_API_KEY - Required - Your API key for your Formulize system.
- FORMULIZE_SERVER_NAME - Recommended - The name of your server. Although the name may be stated already higher up in the .json file, including the name as an environment variable will help the AI understand your system.
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

### 1. Get the code

[Download](https://github.com/jegelstaff/formulize-mcp/releases), use git, whatever you prefer.

### 2. Go to the folder with the code

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
- ```formulize-mcp``` becomes the full path to the compiled .js file (ie: ```C:\\yourdir\\dist\\formulize-mcp.js```).

On Windows, don't forget to use double slashes ( \\\\ ) for directory separators in the path. :(
