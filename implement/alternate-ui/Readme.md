# Alternate UI for EVE Online

![Alternate UI for EVE Online, this part shows the Overview.](./../../guide/image/2020-01-30.eve-online-overview-alternate-ui-and-game-client.png)

The alternate UI is a web-based user interface for the EVE Online client. Because of the HTML based rendering, this user interface is better accessible with screen-readers.

The alternate UI also lets you play the game from other devices that cannot run the EVE Online client but have a web browser. This way, you can play the game from your android smartphone or iPhone. This remote-play is possible because of the division into a frontend and backend, which communicate only via HTTP. The backend runs on the same machine as the EVE Online client and runs an HTTP server. The web-based frontend then connects to this HTTP server to read the game client's contents and send input commands.

This tool also shows the UI tree from the game client and presents the properties of the UI nodes in text form.

![Alternate UI for EVE Online, Visualization of the UI tree](./../../guide/image/2020-07-12-visualize-ui-tree.png)

There can be more than a thousand nodes in the UI tree, even in simple scenarios. And each of the nodes, in turn, can have many properties. So we have tens of thousands of properties in the UI tree when more objects are on the screen.

This quantity might make for a confusing impression, so I introduced a way to better focus on what is of interest to you in a given moment: You can expand and collapse individual nodes of the UI tree. For a collapsed node, it only shows a small summary, not all properties. When you get the first memory reading, all nodes are displayed collapsed, so only the summary of the root node is shown. You can expand that, and then the children of the root node. This way, you can descend into the part you are interested in.

There are two ways to get a memory reading into this interface:

+ Load from a live EVE Online client process. This user interface offers you input elements to interact with input elements in the EVE Online client. Note: When you send an input to the EVE Online client this way, the tool will switch the input focus to the EVE Online window and bring to the foreground. In case you run this user interface on the same desktop as the EVE Online client: To avoid interference between web browser window and game client window, place them side-by-side, so that they don't overlap.

+ Load from a file: You can load memory readings in JSON format you have saved earlier. Since this memory reading does not correspond to a live process, we use this option only to explore the general structure of information found in the game client's memory.

## Setup

The setup section describes preparations to perform once per system (Windows instance).

### Install .NET Core SDK 3.1

Install the .NET Core SDK 3.1, from [https://dotnet.microsoft.com/download/dotnet-core/3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1)

When this software is not installed, the program might exit with a message like this:

> It was not possible to find any compatible framework version  
> The specified framework 'Microsoft.AspNetCore.App', version '3.1.0' was not found.

### Download Web Server Software

Download the Zip-Archive from https://github.com/elm-fullstack/elm-fullstack/releases/download/v2020-11-14/elm-fullstack-bin-f5cb6c6d5932197897806abf3a14efc71ce067b7-win10-x64.zip and unpack it.

## Usage

To start the software:

+ Start PowerShell.
+ In the next command, we use the `elm-fullstack.exe` file we got from the Zip-Archive in the setup section. Below is an example of the complete command; you only need to replace the file path to the executable file:

```PowerShell
."C:\replace-this-the-path-on-your-system\elm-fullstack.exe"  run-server  --public-urls="http://*:80"  --deploy-app-from=https://github.com/Arcitectus/Sanderling/tree/a8273bfa657e11593c70c928b5bd478f1903cf60/implement/alternate-ui/source
```

+ The command starts a web server and the shell window will display an output like this:

```txt
I got no path to a persistent store for the process. This process will not be persisted!
Loading app config to deploy...
Loaded source composition 2ce1c99d13f6532c2019ba97071912e090e992a6b027dacc932980aeb2cb0f3a from 'https://github.com/Arcitectus/Sanderling/tree/a8273bfa657e11593c70c928b5bd478f1903cf60/implement/alternate-ui/source'.
Starting the web server with the admin interface...
info: Kalmit.PersistentProcess.WebHost.StartupAdminInterface[0]
      Begin to build the process volatile representation.
[...]
info: Kalmit.PersistentProcess.WebHost.StartupAdminInterface[0]
      Started the public app at 'http://*:80'.
Completed starting the web server with the admin interface at 'http://*:4000'.
```

+ As the program keeps running, it will eventually write more to the same shell window, so the last output there can become something else.
+ With the command above, the program will try to use network port 80 on your system. In case this network port is already in use by another process, the command fails. In this case you get an error message containing the following text:

> System.IO.IOException: Failed to bind to address http://[::]:80: address already in use.

After starting the web server, you don't need to look at the shell window anymore, but leave it in the background. Closing the shell window would also stop the web server process.

Use a web browser (tested with Chrome and Firefox) to navigate to http://localhost:80/
There you find the Alternate EVE Online UI.

At the top, you find a section titled 'Select a source for the memory reading'. Here are two radio buttons to choose between the two possible sources:

+ From file
+ From live game client process

### Reading from file

Here you can load memory readings from JSON files.
After loading a memory reading, you can inspect it:

> Successfully read the reading from the file. Below is an interactive tree view to explore this reading. You can expand and collapse individual nodes.

### Reading from live process

When reading from a live process, the system needs to perform a setup steps, including the search for the root of the UI tree in the EVE Online client process. During the setup stage you will see diverse messages informing about the current step.

The memory reading setup should complete within 20 seconds. 

If no EVE Online client is started, it displays following message:

> Looks like there is no EVE Online client process started. I continue looking in case one is started...

As long as reading from live process is selected, the program tries once per seconds to get a new memory reading from the game client.

When setup is complete you see following message:

> Successfully read from the memory of the live process

Below is a button labeled:

> Click here to download this reading to a JSON file.

The memory reading file you can download here is useful for collaboration: In the 'Reading from file' section, people can load this file into the UI to see the same memory reading that you had on your system. 

Under the save button, you get tools for closer examination of the memory reading:

> Below is an interactive tree view to explore this reading. You can expand and collapse individual nodes.

### Enabling the Elm Inspection ('Debugger') Tool

To use the [Elm inspection tool](https://youtu.be/NaIfq89UZ2U) in the frontend, open the page http://localhost:80/with-inspector instead of http://localhost:80
