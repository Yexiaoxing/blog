---
title: Quickstart\: Use Persistent Connections of SignalR in ASP.Net
date: 2018-01-02 02:22:12
categories:
  - Tutorials
tags:
  - Tutorials
  - SignalR
  - asp.net
---

## Code 

> ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications. Real-time web functionality is the ability to have server code push content to connected clients instantly as it becomes available, rather than having the server wait for a client to request new data.

1. Install the package

    Go to NuGet and install the SignalR package:

    ```
    Install-Package Microsoft.AspNet.SignalR
    ```

2. Server Connection

    Connection class for receiving and sending messages.
    
    ```cs
    using System.Threading.Tasks;
    using Microsoft.AspNet.SignalR;

    public class ImageConnection : PersistentConnection
    {
        protected override Task OnReceived(IRequest request, string connectionId, string data)
        {
            // Broadcast data to all clients
            return Connection.Broadcast(data);
        }
    }
    ```
    
3. Startup class

    ```cs
    using System;
    using System.Threading.Tasks;
    using Microsoft.AspNet.SignalR;
    using Microsoft.Owin;
    using Owin;

    )[assembly: OwinStartup(typeof(WebSocketPerformance.Startup))]

    namespace WebSocketPerformance
    {
        public class Startup
        {
            public void Configuration(IAppBuilder app)
            {
                // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=316888
                var hubConfiguration = new HubConfiguration();
                hubConfiguration.EnableDetailedErrors = true;

                app.MapSignalR<ImageConnection>("/echo", hubConfiguration);
            }
        }
    }
    ```
    
4. HTML & JavaScript

    ```javascript
    <script src="http://code.jquery.com/jquery-1.6.4.min.js" type="text/javascript"></script>
    <script src="Scripts/jquery.signalR-2.3.0.min.js" type="text/javascript"></script>
    <script type="text/javascript">
    $(function () {
        var connection = $.connection('/echo');

        connection.received(function (data) {
            $('#messages').append('<li>' + data + '</li>');
        });

        connection.start().done(function() { 
            $("#broadcast").click(function () {
                connection.send($('#msg').val());
            });
        });

    });
    </script>

    <input type="text" id="msg" />
    <input type="button" id="broadcast" value="broadcast" />

    <ul id="messages">
    </ul>
    ```
    
    
## Reference
1. [QuickStart (Persistent Connections)](https://github.com/SignalR/SignalR/wiki/QuickStart-Persistent-Connections)
