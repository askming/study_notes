# Inside look at modern web browser

## [Part 1: CPU, GPU, Memory, and multi-process architecture](https://developers.google.com/web/updates/2018/09/inside-browser-part1)
  - Usually, applications run on the CPU and GPU using mechanisms provided by the Operating System. (computer architecture)

     <img src="https://wd.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/9M8aKlSl3207o9C3QVVp.png?auto=format&w=1600" width="65%">

- A **process** can be described as an application’s executing program. A **thread** is the one that lives inside of process and executes any part of its process's program.
- When you start an application, a process is created.
  - The Operating System gives the process a "slab" of memory to work with and all application state is kept in that private memory space. When you close the application, the process also goes away and the Operating System frees up the memory.
- If two processes need to talk, they can do so by using Inter Process Communication (IPC).

### Browser architecture
- it could be one process with many different threads or many different processes with a few threads communicating over IPC.
  - these different architectures are implementation details. There is no standard specification on how one might build a web browser.

    <img src="https://wd.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/BG4tvT7y95iPAelkeadP.png?auto=format&w=1600" with="60%">

- Chrome's architecture

  <img src="https://wd.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/JvSL0B5q1DmZAKgRHj42.png?auto=format&w=1600" width="60%">

  - click the options menu icon more_vert at the top right corner, select More Tools, then select Task Manager. This opens up a window with a list of processes that are currently running and how much CPU/Memory they are using.

### Which process controls what

<img src="https://wd.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/vl5sRzL8pFwlLSN7WW12.png?auto=format&w=1600" width="60%">

### The benefit of multi-process architecture in Chrome
- If one tab becomes unresponsive, then you can close the unresponsive tab and move on while keeping other tabs alive
- Another benefit of separating the browser's work into multiple processes is security and sandboxing.

### Per-frame renderer processes - Site Isolation
- [Site Isolation](https://developers.google.com/web/updates/2018/07/site-isolation) is a recently introduced feature in Chrome that runs a separate renderer process for each cross-site iframe.
  - Site Isolation isn’t as simple as assigning different renderer processes; it fundamentally changes the way iframes talk to each other. Opening devtools on a page with iframes running on different processes means devtools had to implement behind-the-scenes work to make it appear seamless
  
    <img src="https://wd.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/7ilepBEw6b2yUuyABbpZ.png?auto=format&w=1600" width="60%">


## [Part 2: What happens in navigation](https://developer.chrome.com/blog/inside-browser-part2/)

### A simple navitagion

```mermaid

flowchart TD
    A[handling input] --> B[Start navigation]
    B --> C[Read response]
    C --> D[Find a renderer process]
    D --> E[Commit navigation]
```

### Navigate to a different site

### In case of service worker

### Navigation preload


## [Part 3: Inner workings of a Renderer Process](https://developers.google.com/web/updates/2018/09/inside-browser-part3)

### Parsing
- When the renderer process receives a commit message for a navigation and starts to receive HTML data, the main thread begins to parse the text string (HTML) and turn it into a **Document Object Model (DOM)**.
    - The DOM is a browser's internal representation of the page as well as the data structure and API that web developer can interact with via JavaScript.
- Parsing an HTML document into a DOM is defined by the HTML Standard. 

```mermaid
flowchart TD
    A[Style] --> B[Layout]
    B --> C[Paint]
    B --> D[Layout Tree]
    D --> E[Layer Tree]
    D --> F[Paint Record]
    C --> F
```
- The main thread parses CSS and determines the computed style for each DOM node. This is information about what kind of style is applied to each element based on CSS selectors. 

    <img src="https://cdn.jsdelivr.net/gh/askming/upic@master/uPic/RNkSv2_2022_03_25.jpg" width="60%">

- The layout is a process to find the geometry of elements. The main thread walks through the DOM and computed styles and creates the layout tree which has information like x y coordinates and bounding box sizes.

    <img src="https://cdn.jsdelivr.net/gh/askming/upic@master/uPic/XIz4o6_2022_03_25.jpg" width="60%">

- Paint record is a note of painting process like "background first, then text, then rectangle".

    <img src="https://cdn.jsdelivr.net/gh/askming/upic@master/uPic/8pemXJ_2022_03_25.jpg" width="60%">

- In order to find out which elements need to be in which layers, the main thread walks through the layout tree to create the layer tree (this part is called "Update Layer Tree" in the DevTools performance panel).

    <img src="https://cdn.jsdelivr.net/gh/askming/upic@master/uPic/EvJkyy_2022_03_25.jpg" width="60%">

## [Part 4: Input is coming to the compositor](https://developers.google.com/web/updates/2018/09/inside-browser-part4)

- Since running JavaScript is the main thread's job, when a page is composited, the compositor thread marks a region of the page that has event handlers attached as **"Non-Fast Scrollable Region"**. 
    - By having this information, the compositor thread can make sure to send input event to the main thread if the event occurs in that region. If input event comes from outside of this region, then the compositor thread carries on compositing new frame without waiting for the main thread.

- A common event handling pattern in web development is **event delegation**. Since events bubble, you can attach one event handler at the topmost element and delegate tasks based on event target. 