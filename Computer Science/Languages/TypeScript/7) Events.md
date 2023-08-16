# About Events
* Events are things that happen in the system you are programming.
* The system fires a signal of some kind when an event occurs, and provides a mechanism by which an action can be automatically taken when the event occurs.
* Events are fired inside the browser window, and tend to be attached to a specific item that resides in it.
* This might be:
	* a single element
	* a set of elements
	* the HTML document loaded in the current tab
	* the entire browser window.

For example:
* The user selects, clicks, or hovers the cursor over a certain element.
* The user chooses a key on the keyboard.
* The user resizes or closes the browser window.
* A web page finishes loading.
* A form is submitted.
* A video is played, paused, or ends.
* An error occurs.

To react to an event, you attach an **event handler** (or an event listener) to it.
When such a block of code is defined to run in response to an event, we say we are **registering an event handler**.

## Example
Say we have a singe button on a page:
```html
<button>Randomize Colour</button>
```

We could change the colour randomly via the following:
```javascript
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.addEventListener("click", () => {
  const rndCol = `rgb(${random(255)}, ${random(255)}, ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
});
```

# Using `addEventListener()`
From the above example:
* We use the `addEventListener()` method on html elements to add an event listener
* The string (`"click"` in this case) indicates that we want to listen for a mouse click on the element we are adding the event listener to (the button)
* We are running an arrow function that generates a string that has a random `rgb` value using numbers between 0 and 255
* The document background colour is set to be the string
## Listening For Other Events
We are listening for a click in this example, but there are many other options, such as:
* `focus` and `blur` - when the button is focused and unfocused (try using tab)
* `dblclick` - when the element is double-clicked
* `mouseover` - when the mouse hovers over the element 
* `mouseout` - when the mouse moves off the button

Some events, such as `click`, are available on nearly any element. Others are more specific and only useful in certain situations: for example, the [`play`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/play_event) event is only available on some elements, such as [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video).

### List of events
|Event|Occurs When|Belongs To|
|---|---|---|
|[abort](https://www.w3schools.com/jsref/event_onabort_media.asp)|The loading of a media is aborted|[UiEvent](https://www.w3schools.com/jsref/obj_uievent.asp), [Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[afterprint](https://www.w3schools.com/jsref/event_onafterprint.asp)|A page has started printing|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[animationend](https://www.w3schools.com/jsref/event_animationend.asp)|A CSS animation has completed|[AnimationEvent](https://www.w3schools.com/jsref/obj_animationevent.asp)|
|[animationiteration](https://www.w3schools.com/jsref/event_animationiteration.asp)|A CSS animation is repeated|[AnimationEvent](https://www.w3schools.com/jsref/obj_animationevent.asp)|
|[animationstart](https://www.w3schools.com/jsref/event_animationstart.asp)|A CSS animation has started|[AnimationEvent](https://www.w3schools.com/jsref/obj_animationevent.asp)|
|[beforeprint](https://www.w3schools.com/jsref/event_onbeforeprint.asp)|A page is about to be printed|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[beforeunload](https://www.w3schools.com/jsref/event_onbeforeunload.asp)|Before a document is about to be unloaded|[UiEvent](https://www.w3schools.com/jsref/obj_uievent.asp), [Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[blur](https://www.w3schools.com/jsref/event_onblur.asp)|An element loses focus|[FocusEvent](https://www.w3schools.com/jsref/obj_focusevent.asp)|
|[canplay](https://www.w3schools.com/jsref/event_oncanplay.asp)|The browser can start playing a media (has buffered enough to begin)|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[canplaythrough](https://www.w3schools.com/jsref/event_oncanplaythrough.asp)|The browser can play through a media without stopping for buffering|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[change](https://www.w3schools.com/jsref/event_onchange.asp)|The content of a form element has changed|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[click](https://www.w3schools.com/jsref/event_onclick.asp)|An element is clicked on|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|[contextmenu](https://www.w3schools.com/jsref/event_oncontextmenu.asp)|An element is right-clicked to open a context menu|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|[copy](https://www.w3schools.com/jsref/event_oncopy.asp)|The content of an element is copied|[ClipboardEvent](https://www.w3schools.com/jsref/obj_clipboardevent.asp)|
|[cut](https://www.w3schools.com/jsref/event_oncut.asp)|The content of an element is cut|[ClipboardEvent](https://www.w3schools.com/jsref/obj_clipboardevent.asp)|
|[dblclick](https://www.w3schools.com/jsref/event_ondblclick.asp)|An element is double-clicked|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|[drag](https://www.w3schools.com/jsref/event_ondrag.asp)|An element is being dragged|[DragEvent](https://www.w3schools.com/jsref/obj_dragevent.asp)|
|[dragend](https://www.w3schools.com/jsref/event_ondragend.asp)|Dragging of an element has ended|[DragEvent](https://www.w3schools.com/jsref/obj_dragevent.asp)|
|[dragenter](https://www.w3schools.com/jsref/event_ondragenter.asp)|A dragged element enters the drop target|[DragEvent](https://www.w3schools.com/jsref/obj_dragevent.asp)|
|[dragleave](https://www.w3schools.com/jsref/event_ondragleave.asp)|A dragged element leaves the drop target|[DragEvent](https://www.w3schools.com/jsref/obj_dragevent.asp)|
|[dragover](https://www.w3schools.com/jsref/event_ondragover.asp)|A dragged element is over the drop target|[DragEvent](https://www.w3schools.com/jsref/obj_dragevent.asp)|
|[dragstart](https://www.w3schools.com/jsref/event_ondragstart.asp)|Dragging of an element has started|[DragEvent](https://www.w3schools.com/jsref/obj_dragevent.asp)|
|[drop](https://www.w3schools.com/jsref/event_ondrop.asp)|A dragged element is dropped on the target|[DragEvent](https://www.w3schools.com/jsref/obj_dragevent.asp)|
|[durationchange](https://www.w3schools.com/jsref/event_ondurationchange.asp)|The duration of a media is changed|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[ended](https://www.w3schools.com/jsref/event_onended.asp)|A media has reach the end ("thanks for listening")|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[error](https://www.w3schools.com/jsref/event_onerror.asp)|An error has occurred while loading a file|[ProgressEvent](https://www.w3schools.com/jsref/obj_progressevent.asp), [UiEvent](https://www.w3schools.com/jsref/obj_uievent.asp), [Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[focus](https://www.w3schools.com/jsref/event_onfocus.asp)|An element gets focus|[FocusEvent](https://www.w3schools.com/jsref/obj_focusevent.asp)|
|[focusin](https://www.w3schools.com/jsref/event_onfocusin.asp)|An element is about to get focus|[FocusEvent](https://www.w3schools.com/jsref/obj_focusevent.asp)|
|[focusout](https://www.w3schools.com/jsref/event_onfocusout.asp)|An element is about to lose focus|[FocusEvent](https://www.w3schools.com/jsref/obj_focusevent.asp)|
|[fullscreenchange](https://www.w3schools.com/jsref/event_fullscreenchange.asp)|An element is displayed in fullscreen mode|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[fullscreenerror](https://www.w3schools.com/jsref/event_fullscreenerror.asp)|An element can not be displayed in fullscreen mode|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[hashchange](https://www.w3schools.com/jsref/event_onhashchange.asp)|There has been changes to the anchor part of a URL|[HashChangeEvent](https://www.w3schools.com/jsref/obj_hashchangeevent.asp)|
|[input](https://www.w3schools.com/jsref/event_oninput.asp)|An element gets user input|[InputEvent](https://www.w3schools.com/jsref/obj_inputevent.asp), [Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[invalid](https://www.w3schools.com/jsref/event_oninvalid.asp)|An element is invalid|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[keydown](https://www.w3schools.com/jsref/event_onkeydown.asp)|A key is down|[KeyboardEvent](https://www.w3schools.com/jsref/obj_keyboardevent.asp)|
|[keypress](https://www.w3schools.com/jsref/event_onkeypress.asp)|A key is pressed|[KeyboardEvent](https://www.w3schools.com/jsref/obj_keyboardevent.asp)|
|[keyup](https://www.w3schools.com/jsref/event_onkeyup.asp)|A key is released|[KeyboardEvent](https://www.w3schools.com/jsref/obj_keyboardevent.asp)|
|[load](https://www.w3schools.com/jsref/event_onload.asp)|An object has loaded|[UiEvent](https://www.w3schools.com/jsref/obj_uievent.asp), [Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[loadeddata](https://www.w3schools.com/jsref/event_onloadeddata.asp)|Media data is loaded|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[loadedmetadata](https://www.w3schools.com/jsref/event_onloadedmetadata.asp)|Meta data (like dimensions and duration) are loaded|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[loadstart](https://www.w3schools.com/jsref/event_onloadstart.asp)|The browser starts looking for the specified media|[ProgressEvent](https://www.w3schools.com/jsref/obj_progressevent.asp)|
|[message](https://www.w3schools.com/jsref/event_onmessage_sse.asp)|A message is received through the event source|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[mousedown](https://www.w3schools.com/jsref/event_onmousedown.asp)|The mouse button is pressed over an element|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|[mouseenter](https://www.w3schools.com/jsref/event_onmouseenter.asp)|The pointer is moved onto an element|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|[mouseleave](https://www.w3schools.com/jsref/event_onmouseleave.asp)|The pointer is moved out of an element|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|[mousemove](https://www.w3schools.com/jsref/event_onmousemove.asp)|The pointer is moved over an element|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|[mouseover](https://www.w3schools.com/jsref/event_onmouseover.asp)|The pointer is moved onto an element|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|[mouseout](https://www.w3schools.com/jsref/event_onmouseout.asp)|The pointer is moved out of an element|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|[mouseup](https://www.w3schools.com/jsref/event_onmouseup.asp)|A user releases a mouse button over an element|[MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)|
|mousewheel|Deprecated. Use the [wheel](https://www.w3schools.com/jsref/event_onwheel.asp) event instead|[WheelEvent](https://www.w3schools.com/jsref/obj_wheelevent.asp)|
|[offline](https://www.w3schools.com/jsref/event_onoffline.asp)|The browser starts working offline|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[online](https://www.w3schools.com/jsref/event_ononline.asp)|The browser starts working online|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[open](https://www.w3schools.com/jsref/event_onopen_sse.asp)|A connection with the event source is opened|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[pagehide](https://www.w3schools.com/jsref/event_onpagehide.asp)|User navigates away from a webpage|[PageTransitionEvent](https://www.w3schools.com/jsref/obj_pagetransitionevent.asp)|
|[pageshow](https://www.w3schools.com/jsref/event_onpageshow.asp)|User navigates to a webpage|[PageTransitionEvent](https://www.w3schools.com/jsref/obj_pagetransitionevent.asp)|
|[paste](https://www.w3schools.com/jsref/event_onpaste.asp)|Some content is pasted in an element|[ClipboardEvent](https://www.w3schools.com/jsref/obj_clipboardevent.asp)|
|[pause](https://www.w3schools.com/jsref/event_onpause.asp)|A media is paused|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[play](https://www.w3schools.com/jsref/event_onplay.asp)|The media has started or is no longer paused|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[playing](https://www.w3schools.com/jsref/event_onplaying.asp)|The media is playing after being paused or buffered|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|popstate|The window's history changes|[PopStateEvent](https://www.w3schools.com/jsref/obj_popstateevent.asp)|
|[progress](https://www.w3schools.com/jsref/event_onprogress.asp)|The browser is downloading media data|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[ratechange](https://www.w3schools.com/jsref/event_onratechange.asp)|The playing speed of a media is changed|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[resize](https://www.w3schools.com/jsref/event_onresize.asp)|The document view is resized|[UiEvent](https://www.w3schools.com/jsref/obj_uievent.asp), [Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[reset](https://www.w3schools.com/jsref/event_onreset.asp)|A form is reset|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[scroll](https://www.w3schools.com/jsref/event_onscroll.asp)|An scrollbar is being scrolled|[UiEvent](https://www.w3schools.com/jsref/obj_uievent.asp), [Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[search](https://www.w3schools.com/jsref/event_onsearch.asp)|Something is written in a search field|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[seeked](https://www.w3schools.com/jsref/event_onseeked.asp)|Skipping to a media position is finished|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[seeking](https://www.w3schools.com/jsref/event_onseeking.asp)|Skipping to a media position is started|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[select](https://www.w3schools.com/jsref/event_onselect.asp)|User selects some text|[UiEvent](https://www.w3schools.com/jsref/obj_uievent.asp), [Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[show](https://www.w3schools.com/jsref/event_onshow.asp)|A `<menu>` element is shown as a context menu|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[stalled](https://www.w3schools.com/jsref/event_onstalled.asp)|The browser is trying to get unavailable media data|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|storage|A Web Storage area is updated|[StorageEvent](https://www.w3schools.com/jsref/obj_storageevent.asp)|
|[submit](https://www.w3schools.com/jsref/event_onsubmit.asp)|A form is submitted|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[suspend](https://www.w3schools.com/jsref/event_onsuspend.asp)|The browser is intentionally not getting media data|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[timeupdate](https://www.w3schools.com/jsref/event_ontimeupdate.asp)|The playing position has changed (the user moves to a different point in the media)|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[toggle](https://www.w3schools.com/jsref/event_ontoggle.asp)|The user opens or closes the `<details>` element|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[touchcancel](https://www.w3schools.com/jsref/event_touchcancel.asp)|The touch is interrupted|[TouchEvent](https://www.w3schools.com/jsref/obj_touchevent.asp)|
|[touchend](https://www.w3schools.com/jsref/event_touchend.asp)|A finger is removed from a touch screen|[TouchEvent](https://www.w3schools.com/jsref/obj_touchevent.asp)|
|[touchmove](https://www.w3schools.com/jsref/event_touchmove.asp)|A finger is dragged across the screen|[TouchEvent](https://www.w3schools.com/jsref/obj_touchevent.asp)|
|[touchstart](https://www.w3schools.com/jsref/event_touchstart.asp)|A finger is placed on a touch screen|[TouchEvent](https://www.w3schools.com/jsref/obj_touchevent.asp)|
|[transitionend](https://www.w3schools.com/jsref/event_transitionend.asp)|A CSS transition has completed|[TransitionEvent](https://www.w3schools.com/jsref/obj_transitionevent.asp)|
|[unload](https://www.w3schools.com/jsref/event_onunload.asp)|A page has unloaded|[UiEvent](https://www.w3schools.com/jsref/obj_uievent.asp), [Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[volumechange](https://www.w3schools.com/jsref/event_onvolumechange.asp)|The volume of a media is changed (includes muting)|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[waiting](https://www.w3schools.com/jsref/event_onwaiting.asp)|A media is paused but is expected to resume (e.g. buffering)|[Event](https://www.w3schools.com/jsref/obj_event.asp)|
|[wheel](https://www.w3schools.com/jsref/event_onwheel.asp)|The mouse wheel rolls up or down over an element|[WheelEvent](https://www.w3schools.com/jsref/obj_wheelevent.asp)|

## Removing Listeners
If you've added an event handler using `addEventListener()`, you can remove it again using the [`removeEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener) method. For example, this would remove the `changeBackground()` event handler:
```javascript
btn.removeEventListener("click", changeBackground);
```

Event handlers can also be removed by passing an [`AbortSignal`](https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal) to [`addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener "addEventListener()") and then later calling [`abort()`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController/abort "abort()") on the controller owning the `AbortSignal`. For example, to add an event handler that we can remove with an `AbortSignal`:
```javascript
const controller = new AbortController();

btn.addEventListener("click",
  () => {
    const rndCol = `rgb(${random(255)}, ${random(255)}, ${random(255)})`;
    document.body.style.backgroundColor = rndCol;
  },
  { signal: controller.signal } // pass an AbortSignal to this handler
);
```

Then the event handler created by the code above can be removed like this:
```javascript
controller.abort(); // removes any/all event handlers associated with this controller
```

For simple, small programs, cleaning up old, unused event handlers isn't necessary, but for larger, more complex programs, it can improve efficiency. Also, the ability to remove event handlers allows you to have the same button performing different actions in different circumstances: all you have to do is add or remove handlers.

## Adding Multiple Listeners For a Single Event
By making more than one call to [`addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener "addEventListener()"), providing different handlers, you can have multiple handlers for a single event:
```javascript
myElement.addEventListener("click", functionA);
myElement.addEventListener("click", functionB);
```

Both functions would now run when the element is clicked.

# Other Event Listener Mechanisms
`addEventListener()` is recommended for adding event listeners, however there are other ways to register events. These are _event handler properties_ and _inline event handlers_.

## Event Handler Properties
Objects (such as buttons) that can fire events also usually have properties whose name is `on` followed by the name of the event. For example, elements have a property `onclick`. This is called an _event handler property_.

For example:
```javascript
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.onclick = () => {
  const rndCol = `rgb(${random(255)}, ${random(255)}, ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
};
```

With event handler properties, you can't add more than one handler for a single event. Any subsequent attempts to set the property will overwrite earlier ones as it is just a variable:
```javascript
element.onclick = function1;
element.onclick = function2;
```

## Inline Event Handlers - don't use these
The pattern is as follows:
```html
<button onclick="bgChange()">Press me</button>
```

```javascript
function bgChange() {
  const rndCol = `rgb(${random(255)}, ${random(255)}, ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
}
```

It is not a good idea to mix up your HTML and your JavaScript, as it becomes hard to read. What if you had 100 buttons? With JavaScript you could do something like the following very easily:
```javascript
const buttons = document.querySelectorAll("button");

for (const button of buttons) {
  button.addEventListener("click", bgChange);
}
```

Many common server configurations will disallow inline JavaScript, as a security measure.

# Event Objects
Sometimes, inside an event handler function, you'll see a parameter specified with a name such as `event`, `evt`, or `e`. This is called the **event object**, and it is automatically passed to event handlers to provide extra features and information. For example, `e.target` is the object that triggered the event. If using a keyboard, `e.key` is the key you pressed. Rewriting the random colour example again slightly:

```javascript
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

function bgChange(e) {
  const rndCol = `rgb(${random(255)}, ${random(255)}, ${random(255)})`;
  e.target.style.backgroundColor = rndCol;
  console.log(e);
}

btn.addEventListener("click", bgChange);
```

We now are changing the background colour using `e.target.style.backgroundColor`. Note the American spelling.

# Preventing Default Behaviour
As an example, say we wanted to check if the user submitted data into a form correctly:
```html
<form>
  <div>
    <label for="fname">First name: </label>
    <input id="fname" type="text" />
  </div>
  <div>
    <label for="lname">Last name: </label>
    <input id="lname" type="text" />
  </div>
  <div>
    <input id="submit" type="submit" />
  </div>
</form>
<p></p>
```

We can use `e.preventDefault` to stop the default behaviour. Simply put this in a function for the event in question.
```javascript
const form = document.querySelector("form");
const fname = document.getElementById("fname");
const lname = document.getElementById("lname");
const para = document.querySelector("p");

form.addEventListener("submit", (e) => {
  if (fname.value === "" || lname.value === "") {
    e.preventDefault();
    para.textContent = "You need to fill in both names!";
  }
});
```

# Event Propagation
## Event Bubbling
Event bubbling describes how the browser handles events targeted at nested elements.

Consider:
```html
<div id="container">
  <button>Click me!</button>
</div>
<pre id="output"></pre>
```

What happens if we add a click event handler to the parent of the button (the div), then click the button?
```javascript
const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

const container = document.querySelector("#container");
container.addEventListener("click", handleClick);
```

You'll see that the parent fires a click event when the user clicks the button.

What happens if we add event listeners to the button _and_ the parent?
```javascript
const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

const container = document.querySelector("#container");
const button = document.querySelector("button");

document.body.addEventListener("click", handleClick);
container.addEventListener("click", handleClick);
button.addEventListener("click", handleClick);
```

You'll see that all three elements fire a click event when the user clicks the button:
- the click on the button fires first
- followed by the click on its parent (the `<div>` element)
- followed by the `<div>` element's parent (the `<body>` element).

We describe this by saying that the event **bubbles up** from the innermost element that was clicked.
This behaviour can be useful and can also cause unexpected problems.

### Video Player Example
Consider:
```html
<button>Display video</button>

<div class="hidden">
  <video>
    <source
      src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm"
      type="video/webm" />
    <p>
      Your browser doesn't support HTML video. Here is a
      <a href="rabbit320.mp4">link to the video</a> instead.
    </p>
  </video>
</div>
```

```javascript
const btn = document.querySelector("button");
const box = document.querySelector("div");
const video = document.querySelector("video");

btn.addEventListener("click", () => box.classList.remove("hidden"));
video.addEventListener("click", () => video.play());
box.addEventListener("click", () => box.classList.add("hidden"));
```

When you click the button, the box and the video it contains are shown. But then when you click the video, the video starts to play, but the box is hidden again due to event bubbling.

We do not want the box to be hidden again when playing the video.

#### Fixing the Problem With `stopPropagation()`
The [`Event`](https://developer.mozilla.org/en-US/docs/Web/API/Event) object has a function available on it called [`stopPropagation()`](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation) which, when called inside an event handler, prevents the event from bubbling up to any other elements.

```javascript
const btn = document.querySelector("button");
const box = document.querySelector("div");
const video = document.querySelector("video");

btn.addEventListener("click", () => box.classList.remove("hidden"));

video.addEventListener("click", (event) => {
  event.stopPropagation();
  video.play();
});

box.addEventListener("click", () => box.classList.add("hidden"));
```
We are calling `stopPropagation()` on the event object in the handler for the `<video>` element's `'click'` event. This will stop that event from bubbling up to the box.
## Event Capture
An alternative form of event propagation is _event capture_. This is like event bubbling but the order is reversed: so instead of the event firing first on the innermost element targeted, and then on successively less nested elements, the event fires first on the _least nested_ element, and then on successively more nested elements, until the target is reached.

Event capture is disabled by default. To enable it you have to pass the `capture` option in `addEventListener()`.

For example:
```html
<body>
  <div id="container">
    <button>Click me!</button>
  </div>
  <pre id="output"></pre>
</body>
```

```javascript
const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

const container = document.querySelector("#container");
const button = document.querySelector("button");

document.body.addEventListener("click", handleClick, { capture: true });
container.addEventListener("click", handleClick, { capture: true });
button.addEventListener("click", handleClick);
```

In this case, the order of messages is reversed: the `<body>` event handler fires first, followed by the `<div>` event handler, followed by the `<button>` event handler:

## Event Delegation
Event bubbling isn't just annoying, though: it can be very useful. In particular, it enables **event delegation**.

Here, when we want some code to run when the user interacts with any one of a large number of child elements, we set the event listener on their parent and have events that happen on them bubble up to their parent rather than having to set the event listener on every child individually.

Suppose that a page is divided into 16 tiles, and we want to set each tile to a random colour when the user clicks that tile:
```html
<div id="container">
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
</div>
```

```css
.tile {
  height: 100px;
  width: 25%;
  float: left;
}
```

```javascript
function random(number) {
  return Math.floor(Math.random() * number);
}

function bgChange() {
  const rndCol = `rgb(${random(255)}, ${random(255)}, ${random(255)})`;
  return rndCol;
}

const container = document.querySelector("#container");

container.addEventListener("click", (event) => {
  event.target.style.backgroundColor = bgChange();
});
```

We could have added a click event handler for every tile, but it is much simpler and more efficient option is to set the click event handler on the parent, and rely on event bubbling to ensure that the handler is executed when the user clicks on a tile.