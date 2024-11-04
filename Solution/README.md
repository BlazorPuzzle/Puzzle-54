# Blazor Puzzle #54

## Time for Timer!

YouTube Video: https://youtu.be/B6PmGo8kzRM

Blazor Puzzle Home Page: https://blazorpuzzle.com

### The Challenge:

This is an interesting demo where we are tracking user activity.

Navigate to the Timer Demo page, which shows some content and also the Weather page inside an `<iframe>` element.

If you don't move your mouse or press a key for 5 seconds, you will see a warning message.

However, if you move your mouse inside the `<iframe>`, the timer never resets.

Once the time is up, you will have to refresh the page to test it again.

How can we capture the events inside the `<iframe>`?

### The Solution:

The solution is in the JavaScript.

Rather than add event listeners to the `<iframe>` itself, we need to add them to the `<iframe>`'s `contentWindow` object:

```javascript
window.RegisterTimer = (dotNetObject) => {
    console.log("GOT HERE");
    document.addEventListener('mousemove', resetTimer);
    document.addEventListener('keypress', resetTimer);
    document.addEventListener('click', resetTimer);
    document.addEventListener('scroll', resetTimer);

    var iFrame = document.getElementById('myIFrame');
    if (iFrame) {
        console.log("REGISTERING");
        // The fix is to add the listeners to the iFrame.contentWindow
        iFrame.contentWindow.addEventListener('mousemove', resetTimer);
        iFrame.contentWindow.addEventListener('keypress', resetTimer);
        iFrame.contentWindow.addEventListener('click', resetTimer);
        iFrame.contentWindow.addEventListener('scroll', resetTimer);
        setTimerInterval(dotNetObject);
    }
    else
    {
        console.log("NO IFRAME");
    }
}
```

Boom!
